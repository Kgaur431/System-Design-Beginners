### Populate the cache
``` 
    -   cache sits b/w the api server & the db. it is not an hard & fast rule, it sits there logicaly.  phically it may be anywhere.  
    -   when we have a api-server, cache & db then the flow go like this.
            1.  Lazy Population:-
                        assume when a user made a api call to the api server, api server will checks the data into the cache, 
                            if the data is present then api server can fetch the data. & send back to the client (img 126).  
                            if the data is not present then api server talks to db & fetch the data. & push that data into the cache & then  send back to the client (img 127).  
                        this is most popular way of populating the cache, it is called Lazy Population. 
                        mean "we are not eagerly putting the data into the cache, we are letting a request come to api server &  when it have a cache missed (means it can't find the data into the cache) then we are going to db & populating the cache into the cache (img 128)". 
                        
                        why we call it as lazy population ?
                            becoz we are not pro-actively pushing the data into cache. 
                            cache will only contain a data for which any request came in. 
                                means we are only push the data into the cache when any req come for that data & if the data is not present then we fetch the data from the db & push into the cache.
            
                when we set something in cache then we would alwasy set an expiry of it (depending on the usecase).    
                        what does expiry ensures here ?
                            suppose when the expriy time comes in.  
                                like we set a particular key with the expiry of 5 min, once that 5 min end then the key is automatically deleted from the cache. it helps to reduce the memory footprint of the cache. 
                                becoz if we set something into the cache with no expiry then that key would be store forever. this is how the memroy leak happens. 
                                assume the RAM of our machine has some limitation like 4gb, 16gb etc. becoz of the limitation we can filled the data to some limit, we can't fit more data into it.           
           
               img 129.
                          
               example:- caching blogs.
                    let say we are building a bloging app, In order to fetch blog details we would join 3-4 tables (blog, author, tags etc). 
                    so for a given blog-id we are creating this blog obj this is an expensive oprertaion (joining multiple tables).
                    what if we store that json obj (blog details that we have get by joining multiple table) in the cache.
                        like for a particular blog-id we can store the actual json obj (blog details).  
                    so when the subsequent request comes for that blog-id, then request goes to the cache & finds the exact json obj, it return to the user.    img 130. 
                                     
            2.  Eager Population:-
            
                        -   when we pro-actively trying to push a data into cache. 
                        -   there are two ways to do eager population. 
                                1.  when a api server getting a write request then we are not just writing to the db only, we will make two writes one to the db & another to the cache.        writing means adding.
                                     img 131.           
                                        eg:-     let say we are building crickbuzz, when the cricket match is happening live than millions of users are reading the score, when they read the score what would happen ? 
                                                        we need to handle a high amout of read requests,   so we will put the cache in b/w & let this cache to serve all the updates.  
                                                        means let say comentator updates the score then we updated into the db only (img 132). 
                                                            now In the cache, we are realying on the expiration of the previous score to happen means we are talking about the cache missed (We read in lazy population).
                                                             like update req will go to the db & then req will go to the cache & push the score into a cache (img 133). 
                                                             this is where the problem happen let say the score has updated & we are waiting for the key to expire then we would update the score in the cache.
                                                             but the better would be "when we update the request into the db then we should update the request into the cache as well" (img 134).
                                                        so that the people who are reading the commentry they will gets the updated score from the cache itself.
                                        means we are not waiting for key (previous score id) to expriy instead we are pro-actively updating the cache.  means api server is updating the data in both place in the same request.
                                        like api-server itself is pro-actively wirting the score into db as well as cache.
                                
                                2.  when we pro-actively push the data into the cache. means it is not like that api-server is writing the score at two places. 
                                        assume a tweet is made by a very popular person, let me pro-actively push that tweet into the cache. it is not like that apir-server is writing the tweets into two places with the same request.  it is like "asyn way of doing it".
                                        that tweet will definately go viral becoz it made by popular person so we are pro-actively push that tweet into the cache. 
                                        eg:-
                                                when we open youtube we see some recommended videos (the videos which shows to the people in the recommended section are very likely to to be accessed. becoz we showing these videos as a first place on the youtube) so people will click on it. 
                                                now we know that we are pushing some videos to most of the people recommended section (this video is not recently created, it might be created 2 year ago) or the recommended system pushing these video on the recommended section of the lots of people,
                                                we pro-actively cached those videos in the cache. 
                                                this way the cache missed does not happen.  
                                                becoz we know that, these videos will be showing in recommended section so lots of people can click on it. therefore we are pro-actively cached those videos in the cache. 
                                        here we know that, the things may access very frequently in the coming time so we are pro-actively push it in the cache.
                                        img 135.
```
### Scaling cache
``` 
        Cache is similar to a db. 
        so scaling techniques of db are similar to scaling techniques of caches. 
        1.  Vertical scaling:-
                img 136.
                    if we want to cache more data. we can put more RAM, more disk, more CPU into my cache. 
        
        2.  Horizontal scaling:-
              1.    Read-Replica (scaling reads):-
                        img 137.
                        means when our cache has read heavy (we getting a lot of read requests) then one node is enought to do that then we have master replica set up.
                            api-server knows where to route the request.  
             
             2.     Sharding (scaling writes):-
                        img 138.
                        means when our cache has writes heavy (we have huge amount of data to cache) then one node is enought to do that then we go for distributed cache or sharding. 
                             means instead of having one cache we have multiple caches, each handling mutually exclusively subset of data.
                            api-server knows where to route the request.  
                              
             




```
