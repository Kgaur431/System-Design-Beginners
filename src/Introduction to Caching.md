## Caching
``` 
        -   it is very popular way through which we can get perfromance out of our system.
        -   the idea is simple behind the caching:-
                    'we put the frequently access data in a place which is faster'. 
        -   caching is a technique. caches are the place where we store the data.  
                caches as a place is anything which helps us to avoid an expensive network I/O,  disk I/O or computation.
                    like:-  for any computer in the world there is only three factors that we have,
                                1.  Network     
                                2.  Disk
                                3.  CPU. 
                                                we are not consider the RAM, becoz RAM is the place where we cached the data.
                    so whatever helps us to avoid an expensive network I/O,  disk I/O or computation that is cache for us.   
         
                eg1:-   we want to get a profile information for a user, to do this what we can do ?
                                we make a api call that will talk to api server, api server will talk to database, assume database will join 3-4 tables to fetch the profile info & then send it back to the user.      
                                    here joining 3-4 table is an expensive process. so what we can do ?
                                        'instead of going through db everytime, once we have the profile info (like first time we fetched) we cached it in a cache.     
                                         like we use Redis to store this info. 
                                         now the next time when the request comes to the api server then it talk to redis & fetch the info. so no need to make the db request & perform the join operation again.     

                eg2:-   when we want to read a specific line from a particular file.
                                if we are repeatedly reading this line then we can cached that info.        becoz this is an expensive disk I/O that we are doing. 
                
                eg3:-   when we perform multiple joins in the db.       
                img 115. 
        
        -   In most of the case, caches are the temporary storage location. which means if the cache goes down then there is not much impact. like we can face some performace impact but anyway our data is coming from the db (img 119). 
                eg:-    assume user made a api call it came to api server, what api server could do ?
                                api server first check the data into the cache (img 117), if cache has a data then api server fetch the data from the cache & retun it. if the data is not there then api server makes a call to the db & fetch the data from it & then put that info into the cache & then api server will finaly 
                                gets the response & send the response to the user (img 118).   
                                
                                this way the cache is always updated. 
                cache will make things faster. 
        
        -   when we store the data into cache, cache need to be faster then the db.
                the things that are faster are typically more expensive.
                so we cache the data which we very likely to required in next few minutes.  
                    like we can't cache the entire data.   like Redis store the data in the memory (RAM).  RAM is very costly so if we want to store the info in RAM then we need huge amount of RAM, it become very expensive (img 120).
                    
                How do we know which data should we cache ?
                    we can see the pattern of our system. 
                    eg:-    tweets, when someone tweets on twitter that tweet has sent to everyone of that followers. (we follow a job_skeers page on twitter, it tweet a job application so whoever has follow that page, all of them want to see the tweets to apply the job) everyone is interesting to see the
                                recently published tweet. so it does not make sence to call db everytime. instead we can cache the recent tweets into cache. therefore when request come to api server to read the tweets (img 121).
                
                caches help to overcome the load on the db, it saves us to avoid expensive operations. 
```
### Caches are not restricted to RAM based storage.
``` 
        Redis store the data into RAM. it does not mean that all the caches are store data in RAM.
        the idea is simple:-
                a cache is anything which help us to save from expensive computation.    
                so we store it in a place means any storage that is 'nearer' so that we can avoid expensive computation (img 122).  
                    eg:-    sometime the disk of api server becomes a cache. In img 12, we have a cache that is near to the api server.
                    
       cache can be any storage that is nearer to the api server. so that we can avoid expensive disk I/O or db I/O. 
       
       In general cache has key-value based pattern, we can put something in a cache with some key & we get the data with the help of key. like hash tables.  
        
        examples of caches
            img 123
            img 124 :- auth tokens are cached in cache to check the validation of the token from cache itself. like when user request on backend so every req has to be validate so doing db call for validation is not good.
            img 125
            
        When should we use cache ?
            wherever we see a pattern there the particular thing is accessed frequently, that is the place where we can put the cache.  means we can cache this thing.
        
        exercise:- watch 11, from 10:30 to end.
```