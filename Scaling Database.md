``` 
    The strategies which we are going to learn in below part are not just for RDBMS, we can apply to Non-Relational db as well. these strategies are very common way to scale any statefull thing like db.  (img 53).
    
    1.  Vertical Scaling:-
            vertical scaling is all about adding more cpu, ram, disk to the db.
                eg:- let say when we start building our product then we have took 4gb of ram is enough to store the data that we have. but when we get the huge amout of load are coming then the 4gb ram is not enough. 
                            so for me to handle huge amount of load, we would have to pour in more resources into the db.  like adding more cpu, ram, disk to the db & make the db big enough (img 54). 
                
            Now days we are using cloud. they give one click solution to upgrade the db to a higher infrastructure level like 4gb ram to 8 gb.  although the cloud providers are abstract out all the complexities of it but it still require downtime. 
                becoz database could reboot at that time. 
                    so whenever we are scaling up vertically, there is a short duration of time where the database could be unavailable which means our system would have small downtime.
               
           but it gives us the ability to handle more load that we are receiving.
           
           whatever we are scalling vertically, it has a limit.
                eg:- whenver we go any cloud console, we would see that only up, until the certain point the option are there. means we can't go beyond that. becoz "vertical scaling has a physcial limit".
                        like:-  let say we have purchased a smartphone, that smartphone comes with the expandable storage. like for the expandable storage, we get the option to put  at max 128gb as expandable storage.
                                    what happens if we put 1tb as a expandable storage. 
                                        smartphone will not recoginse it, becoz of the physical limit.  (if we want to see the deep view :- like we can see the cached line, bus ratio etc means there is an actual hardware integrated circuit layer will have the limitation becoz of which we can't go  beyond a certain level).
      
      Vertical scalling is very easy, like two clicks + one small downtime = we have our database upgraded to handle more load.
      
      
    2.  Horizontal Scaling:-
        img 57.
        if the amout of load that we are handling is very huge, we have huge data that is where we can go for the horizontal scalling.           
        In Horizontal scaling first we learn about the "Read Replicas".     
      
        Read Replicas:-         first way.
            "it is an standard way to scale the reads".
                like In most of the work load across the companies, we would always see very high 'read to write ratio'. means for every 100 operations are happening on the db, in which 90 operations are read & only 10 operations are write.
                so there is very huge diff from in read to write ratio.  which means if there is lots of reads request coming, what if we put all the read request on to Replicas (it is know as read replicas) (img 55)& while the Master database is handling all the write requests (img 56).
                this way we move the read request into the another db & write request to the another db. this is how we scale the read requests independently. this is the concept of Read Replicas.
            
            Ques:-  Now we have two db, How we can ensure that Read request need to go to Replica & Write requests go to Master ?
            Ans  :-  very simple way to do this, do it at the API server level. 
                          means In the Api's that we create, we create a db connection obj (it typically create connection with one db). instead of creating one connection we have to create two connection obj for the two database. like one with replica, another one with master.
                            Now when we write the business logic that time we choose the connection obj of replica to handle the read requests. when we know that this is the write reqests so we choose the connection obj of master to handle the write requests. 
                            
            we know that "Master & Replica are connected via Replication".
                this Replication determines "How writes from Master go to Replica" (img 59). 
                
                there are two modes of Replication.
                    1.  Synchronous Replication:-       (img 60).
                            -   In Synchronous Replication, when the write req comes to the api server then our api server make the write request to the Master db (img 61). now from the master, writes are happening / push into the replica.
                                    here In Synchronous Replication, the writes req from the master goes to replica (img 62).  
                                        so either the master make the call to replica or api server make the call to replica irrespectively (img 63). 
                                            but the idea is that "the api server does not respond back to the client unless the write has happened on the master & on the replica as well, 
                                            then only the write request will be consider as completed & then only api server respond back to the client (img 64).  
                                    this is way both the db are perfectly sync. there will be very strong consistency b/w these two. becoz we are not terminating the wirte request untill the write req pass through the (is done on the) replica & then only api server respond back to the client (img 65).
                                        means write req's pass through the two databases & then return the response.
                            -   write req is very slow.
                            -   we can see the slightly higher latency, but we are getting the 0 replication leg b/w the two db (consistency is high).      
                                 
                    2.  Asynchronous Replication:-  (img 66). 
                            -   mostly all of the db's follow the async replication.
                            -   In Asynchronous Replication, when  when the write req comes to the api server then our api server make the write request to the Master db (img 67) & then the response has taken by the api server from the master, & send back to the client (img 68). 
                                        means Master db does not have to write into the replica immediately. 
                                        this is how the write request get the response from db faster (img 69). so client see the very fast write request execution. means users does not have to wait for the master to propogate the write request into replica immediately.
                                        
                                        Ques:-  How does replica get the updates of Master ?
                                        Ans  :-  all most all the db's come up with ways to create replica.     like postgres, sql, mongodb etc. everyone has the replica.
                                                     we just have to do some configuration. "Replica periodically pulls the data from the Master & update itself".      like whatever write query happend on the master that will be pulled by replica & update on itself, every certain period of time.
                                                     "instead of pushing the data from master to replica, now replica pulls the data from the master periodically". 
                           -    there might be some delay, after which a replica pull the data from the master. which means there will be no strong consistency b/w these two db's.   
                                some of the challanges with it:- 
                                    1.  there would be a chance that 'write req has happend on the master but before it has pulled by replica, before that there would be another read request come to replica (it will get the old reads).  this challange is known as replica lag. (img 70).
                    
            -   when we want higher consistency b/w the master & replica then we can go for Synchronous Replication. 
            -   when we want to scale the reads becoz of the 90% of the operations are read operation. so we can scale it by adding multiple replica's (img 72).        
                    
       Sharding:-       second way.          
            img 71. 
            if we get the huge amount of write requests (we scaled the read's),  we can do vertical scalling but after certain point we can't do this also. this is where sharding comes,
                sharding suggests like 'becoz one node cannot handle the data / incoming-load (write req), we can split it into multiple exclusive subsets of the data.  
                    like instead of having one data node that holding all the data. we have three nodes containing three subsets of data.
                eg:-   let say we have key-value db, in which we are storing keys starts with A to Z. 
                            so we can put the keys from A to J in shard1, K to T in shard2, T to Z in shard3 (img 74).  means we are defining our own logic. this is how we created the mutually exclusive subsets of the data & each shard is responsible for own things.  this is know as Sharding.
                          here the Api server knows about all the shards that we have & it know which request go to which shard.   this is an routing logic. 
                            like if the write req have a key of A to J then api server route the request to the shard1, and so on.   shard's are kind of sub databases.
                                so if the write req comes to the api server then it know where to route that requset by creating the connection obj of it & then route the request.     means we define the routing logic at the api server side. 
                that's how we divide the load into the factor of three.  like we divide the single db into three sub-db.    (img 73). 
```