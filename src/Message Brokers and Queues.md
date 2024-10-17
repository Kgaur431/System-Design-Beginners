###  Asynchronous Processing
``` 
        let say user sends a request & we immideately handle it. this is an example of  Asynchronous Programming (img 153).
        some of the exmaples of synchronous programming:-   img 154.
        but there are something that should not be run in the synchronous way.
            eg:-    spining up a virtual machine.
                        let say we are spining up the EC2 server, assume it will take 5 min to spin up the virtual machine (img 155). & we are not getting anything in the response. that is not good. 
                        we should see something like 'request is processing', behind the scene it go & create the EC2 machine. this is an asynchronous communication (img 156).
        asynchronous communication gives us the better user experience for long running task.  
            understand in deep:-        when we do asynchronous communication, this will be the flow
                1.  Client makes a request, it comes to api server, api server typically register something in the db.
                2.  api server pushes task to a message broker & immidiately respond back to the user.
                       eg:- let say we spining up a EC2 machine, user made a request to EC2 backend, EC2 backend will make one simple entry that 'this particular user spining up the virtual machine'.
                                 the EC2 backend will created a task & put into the broker, from the broker there are worker loads they are pulling the task out & creating the massive processing & update into the db when its done.      
                                 this is what happen when we spin up a virtual machine.
                      user are not waiting, user would get an immidiated response that 'we received your request, please wait we are building the your virtual machine'.   
                        like when EC2 backend received a req, it will make an entry in the db 'we are spining up an virtual machine with the given set of requirments & update the status as InProgress'. then it send the task to broker & from broker the worker will pick up the task & build the virtual machine, 
                            once the task done then worker will update the same row with InProgress to Done. 
                        when the workers are spining up the virtual machine & updating the task etc. "we are not blocked, we can close the browser, we can wait, we can do whatever we want" becoz the things are happening behind the scene'. so once it's done then we can open browser & see the virtual machine.
                     (img 157). 
        
                    like:- these we can do in async way:-   
                            when encoding - decoding happen, encryption - decryption happen,  uploading huge amount of file etc.  
                
                the important component which will ensures to do a task in asynchronous way is Broker / Queue (Message Queues / Message Brokers).
                
        we learn more about Message Queues / Message Brokers:-  img 161.
                img 158.
                they help services / applications communicate through messages, these are asynchronous communication. 
                    like EC2 service put a message to the queue that has consumed by workers & the workers are doing the task.
                1.  Long-running task:-     spining up the EC2 machine takes 5 min so we can't let the http request to wait until 5 min.    this is know as asynchronous communication.
                2.  Trigger dependent tasks across machines:-   let say when something has done & we want to trigger something after that.  this is know as asynchronous communication.
                
                we will talk about Video Processing:-
                        whenever we upload a video (like 1080P), the video needs to be converted to various resolutions (360P, 480P, 720P). becoz on the small device we can't watch it. 
                        now when video upoad service uploads a video to S3, upload service pushes a message into message queue (SQS / RabbitMQ). when it send the message to message queue, that message has consumed by video processing service.
                            like In the message we send the details like videoid, actul video, creator details etc.
                        the video processing service would use these message details to process the video(that has uploaded at S3) & re-upload the video in the S3 in various formates.
                        What the video processing service worker do ?
                            from that message the would get the videoid.
                            from that videoid the would get the actual video that store in S3.
                            then the workers would download that video on this machine (img 159). & create multiple formate of it & then upload that video back to S3.
                        we just upload the video in 1080P to S3 & done. behind the scene this video processing done in asynchronous way.
                        this will not done synchronously becoz it might take 1hour or more & the http request can't wait for that much time. 
                            
                Features of Message Brokers (SQS / RabbitMQ) :-
                   1.   img 162:-    
                            it helps us connect two system. 
                            like video upload service to video processing service.          
                                video upload service uploaded a video in the single formate (1080P) to S3 while video processing service took that video and process it. 
                                here 'we had to notify the video processing service' like there is one video uploaded, process it.  "this notification was done by passing a message through message queue". 
                   
                   2.   img 163:-
                            messages queues are buffers the message. 
                                like In the the message broker we can put 100 of messages, these messages would be liying in the message queue then someone (Email service) pick that messages & do some processing (img 164). 
                                eg:-     let say we have order service where we want to send a email to everyone who has placed a order.
                                            assume there is an very high traffic for the order service. like lots of order has created. we want to send an email to every single one.
                                                so when we recived lots of order service then we would put messages into the message queue. "these messages are buffered" & we have an email sender which will pulling the messages out from the message queue & making call to the email api to send email to users.
                                                like email sender will consume messages as it want like one by one, slowly or faster.  it will take the message & send the email that's it. 
                                this is how the message queue can buffer lots of messages into it.
                   
                   3.   img 169:-
                                How long would message queues retain the buffer.        means hold the messages into the buffer.
                                    it is typically configurable like SQS will offer that for 14 days etc. means every queues have some sort of expiration.     img 164.
                  
                  4.    img 169:-
                                let say we want to send the email notification. like user placed an order then order service push the message to the message queue & the email sender will consume the message.     
                                    but before the email sender make a api call to email service assume the email sender crashed (img 165). now what happen ?
                                        the message was consumed by email sender but the email was not sent.
                                        every queues has diff way of handling this type of situation. but In general case messages queues gives an api to call to delete particular message. 
                                            means when email sender consume the message, email sender call an email api to send an email then email sender delete the messages in the queue (img 167). 
                                            we are trying to say that:- eamil send consume the message that does not means that the messages have deleted from the queue. "we have to explicitly consume then explicitly delete that message from the queue".
                                            this ways we are ensuring that email sender send the email to the user & it will explicitly delete the message.   
                                                assume email sender send the email to the user but when it going to delete the messgage that process / job get killed. what happens ?       ||  before deleting it the process killed. 
                                                    after some time the message get reappear at the top of the queue (img 168) later that message will consume by some other consumer (like email sender).
                                                    becoz we did not delete the message, it does not mean that the message is permanently gone. 
                                                        if we not delete the message then the message will reappear after some time.  mean 
                                                        Understand the above thing:-
                                                                assume kartik placed an order so the order service send the message to the message brocker, now the message in the queue so the consumer (email sender) will pull that message & make an api call to email service to send a email to the kartik. (let say kartik recieved an email).
                                                                now consumer has to delete the message explicitly, but while it is deleting assume that the process has been killed || system crashed. now what will happen ?
                                                                    the message will get reappear at the top of the queue. now the consumer will process the same message again & then delete the message explicitly once it's consumed the message. 
                                                                that's how kartik receive email twice for a single order.
                                conclusion:-
                                        when we done with the processing then we are going to delete the message. before we can able to delete, the process crashed. then the message would get reappear after some time then we can reprocess the message. this is the beauty of message brockers.
                                        message brockers ensures that we process & delete every single message that consumcer has consume.
                                        
                                        "when we design a system with a message broker, we should remember the fact that some messages might be deliver to consumer more than once". we should be ok with that. means our business logic at the email sender have to handle this situation. 
                
                Typical flow while using a message queue:-
                    img 170.
                        eg:-    let say user uploaded a video, video got uploaded to s3(img 171). we want to do auto-captioning.
                                        we have some machine learning algo that can understand the video & get the audio out of video & create a subtitle from it.
                                        so when the video has uploaded to S3, api server would send the message to the queue, these messages will consumed by captioner (img 173) & then captioner download the video from S3 & create the caption by using machine algo & then updates the caption in to the db for that video(img 172). 
                                        'creating a subtitle can not done synchronously, it's an time consuming process.    so user uploaded a video & letting people watch it. after some time subtitle would start displaying automatically.  img 174.
                
                Exercise watch video 19, from 14:40 till end.
```