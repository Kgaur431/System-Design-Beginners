``` 
    Apache Kalfka is an Message streams.
    img 176.
    -     Message streams are very simial to Message queues, just have some diff. 
          we understand it by an example:-      we are building Medium.com
            =   On Medium.com people publish blogs.  what we are trying to do ?
                    let say everytime publish a blog by the user, we do two things.
                        1.  we index that blog in Elastic search. so that we can power a search engine with that. 
                        2.  we want to do in User tables count++. like total no. of posts that user has written, we want to do count++. 
                    How can we do it ? 
                        Approach1:- we use one Message queue and add logic in consumer (img 177). 
                            whenever a blog has published. api-server makes an entry to the db for the newly published blog & push the message into the message queue. from the message queue, one of the consumer will pull the message & then the consumer will add the index in the elastic search & 
                            that consumer also fires an update query into the db by doing total post++ (img 178).  
                            here 'the consumer is doing two things'.    the issue with this ? 
                                what if the consumer can able to add the index into the Elastic search but it unable to update the count in the user table like db crashed || consumer crashed.   
                                if we search the user posts then In search box we can see that user has 6 posts but the total count is showing only 5 posts.    means the data become inconsistent (img 179). 
                        
                        Approach2:- we use Two message queues to solve that problem (img 180).
                            whenever a blog has published. api-server makes an entry to the db for the newly published blog & push the message into the two message queue. from the message queue, 
                                from one queue we have consumer called search consumer, it will pull the message & then the search consumer will add the index in the elastic search.
                                while from another queue we have consumer called counter consumer, it will pull the message & then the counter consumer will fires an update query into the db by doing total post++.
                            Now, assume search consumer will go down & unable to add the index into the Elastic search, but the counter consumer updated the count so we sitll have inconsistent data.
                                 
                    The root cause of this problem, we don't want to have multiple calls going from anything, it should be just one.    watch video15 from 04:19 to 15:00
                    we solve this problem using Kalfka.             
            =   Understand the Message streams:-        
                        read img 181.
                        In case of Message Queue:- the Message in message queue was send to only one of the consumer (img 182). all the consumers are homogenous means any of the consumer will pull the message from the queue, all of them doing same thing.  
                        what message streams suggest ?
                            instead of one consumer pulling that message, we can have multiple type of consumers that will pull the exact same message.              
                       
                       Approach3:-  Using Streams & Multiple types of consumers (img 183).
                            here we have replaced the Message Queue with Message Streams. 
                                "form the same message stream, we can have two diff types of consumer all together were consuming those messages".
                                let say user publish a blog, api-server put one event in kalfka & now that event has consumed by both the consumers (search &  counter consumers).  
                                    then search consumer put the index into the Elastic search & counter consumer will fires an update query into the db by doing total post++.
                            we can see that "Api server can writing the event into one message stream & from the message streams, the same message will consumed by both the consumers". 
                            
                            we will understand in deeper that how it happen:-
                                    first we understand the Message queues:-    (img 184).
                                            let say api server have to send 4 messages in the message queue. we have 3 consumers which are doing exact same thing. what happens ?
                                               api server send a messages in the message queue (img 185). now one of  the consumer will pulls the message from the queue.
                                               let say consumer1 picks the message1 & consumer1 start processing message1.
                                               let say consumer3 picks the message2 & consumer3 start processing message3.
                                               let say consumer2 picks the message3 & consumer2 start processing message2.  img 186.
                                           In Message queue, it does not matter that "to which consumer the message queue is sending the message to process it, becoz all the consumers are doing the exact same thing".                Imp Note.
                                               let say consumer1 completed the processing  so it's deleted the message1 (img 187).
                                               now let say consumer1 picks the message4 & consumer1 start processing message4.
                                           
                                           we can see that How one message can be process by any one consumer. all the consumers have to do the same thing.                             remember we are forcing on this "all the consumers have to do the same thing".
                                           now we go to next approach. 
                                                
                                           
                                    second we understand the Message streams:-      (img 188).
                                            let say api server have to send 4 messages in the message streams. we have 2 consumers which are doing diff things. what happens ?
                                               api server send a messages in the message streams (img 190). now we have diff types of consumer, which we called as Consumer groups.
                                                    first we have Search Consumer groups, it has it's own set of servers. 
                                                    second we have Counter Consumer groups, it has it's own set of servers.         eg:-     consider this (img 189) as one Consumer group. assume it as an Search Consumer group which has multiple sub-consumer which will do the exact same search thing. 
                                               Now we have two consumers which is handling totally diff types of usecases that needs to process exact same event.
                                               let say everytime the blog was published, api server can put the events into the message streams. now the same event will be consumed by both of the consumers.  what will happen ?
                                                    events / messages are lined up in the kalfka,   Must Watch video15, from 09:19 to 11:30
                                                    
                                                    messages are store in kalfka always. we can configure the expiry policy. 
                                                    In message streams, same set of messages are iterated by diff types of consumers.   
                                                    
                                                    assume the search consumer will iterate on the message streams & it get the message1. now the search consumer will process it, means let say search consumer has 3 sub consumers which are doing the same thing means   
                                                    after that counter consumer will iterate on the message streams & it get the message1. now the counter consumer will process it, means let say counter consumer has 3 sub consumers which are doing the same thing
                                        
```
### Kalfka Essentials   (Every engineers must know)
``` 
        let say we have build a blogging app. whenever a blog has been published, we want to push a message into the message streams. so the name of the topic of the blog is onPublished. 
        In kalfka whenever we push a particular message for the topic.
        In kalfka each topic has partition.  like we can assume one piple(img 191) we draw as one topic & in which we have some partition (img 192) of that topic. 
        so when the blog has published & we push a message in kalfka then what happens ?
            The no. of partition that we provide & the message that we have. message is hashed. the message is know that this is my partitioning key (hashed), depending on the partitioning key the message will take one partition to push into.
            let say  partitioning key is user-id. so user-id is pass through the hashed function to gets an index & then user-id would go through in one of the partition like 3 partition.  
                becoz hashed function is deterministic so we would always get the same value for the exact same input.  
            so all the events / messages for one particular user will always go from one particular partition.  
                it does not mean that 1 user = 1 partition. so if we have  1 million user then we need 1 million partitions.    it's more about "we have limited no. of partition, hashed function modulas of the no.of partition we have". so each partition can handle 333000 users. 
        that is what hashed based partitioning would do.
        so the one message is going through the one partition & sit infront of the top (img 193).   this is what happen when we sending the message. 
        img 194.
        summary in one shot:-
            -   In kalfka we have a topic & topic has multiple partition. 
                    so when we are pushing a message in the stream that time we define the partitioning key into the message. so the message is read the partitioning key, passes through the hash function & gets which partition the message should go to & then the message go through that partition & sit on the top of the head.
            -   "all the messages belonging to particular user will always go to one partition". which means we can apply some logic on the consumer side if we want to. 
                    this is how the messages would go into kalfka. 
        Now all the messages are into the kalfka (img 195).
        Now we want to read those messages, How we can read it ?
            what happens when we reading the topic using consumer from the kalkfa. 
            let say for this kalfka which has 3 partitoin(img 196), which means at max we can have 3 consumer of a particular consumer group in parallel.   
                eg:-    we have 3 partitoin, which means for the search consumer I can have at max 3 consumers runing. these consumer will consuming message from kalfa. so internally what the kalkfa does ?
                                whenever a particular consumer is consuming the topic from kalfka then the kalfka assigns one partition to one consumer. 
                                    like:-  first partition is consumed by consumer 1.
                                               second partition is consumed by consumer 2.
                                               third  partition is consumed by consumer 3.
                                    but let say we have spin up 5 consumers then what will happen ?
                                            the consumer 4 & the consumer 5 will not get any messages.    (img 197).      
                                            becoz "it is one to one mapping b/w partitioning & consumers that we are running. so that when consumer1 is iterating over the partiton1 then that consumer will get all the messages as part of partition1.    (img 198).
                                            "therefore the parallelism of the kalfka is limited by the no. of partitions in kalfka for that particular topic".  so if we want to consumes 5 things in parallel then we have to change the no. of partition as 5.  
                                            kalfka gives the ability to configure the no. of partitions.
                kalfka is limited with this factor, that 'one consumer can interate through one partition only' ||  'one partition can be consumed by one consumer only'. 
                so "the maximum no. of consumers that can consume from a particular topic is equal to the no. of partition of that topic". 
                like search consumer consume the partitions (img 200), we can also have counter consumer consuming the partitions (img 199).  becoz the same set of messages are iterated by diff types of consumer. 
                                    
       "when consumer done with reading / processing some messages, we can commit it & kalfa will store it for us. like consumer have read till this particular message. so the next time when consumer restart consuming then it restart from the last commited point".                
            so we can start from the middle by commiting to that.
            similarly like we were deleting the messages in the message queue after processing the messages same way here we will be commiting the messages after processing the message.    
                assume if we delete the message then how other type of consumer can consume it. 
                watch video15 from 19:00 to 20:00 etc
      
      Kalfka holds the message untill as per the deletion policy. it will keep the message there.
       
       Execrice:-     watch video15 from 20:00 to 21:00.
```




