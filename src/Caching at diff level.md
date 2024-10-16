### diff places in our architecture where we can cache things. 
``` 
    img 139.
    warning:-   all though caching is good but it comes with it's massive disadvantage of staleness & invalidation data.
                            -   means "when we are caching something unless that key expires we are serving the stale data". assume in actual db the value would have changed but we are ok to serve stale data. 
                            -   Stale data is an problem in case we want the consistent or accurated data always then we can't cache things.
                                    eg:-    Bank account details.
                                            if the current acc bal,  if the bank has to render the balance data from the cache then what happen ?
                                                let say durga has made a transaction of 200$ in kartik account. now she is saying that can kartik open his account & check it once.   
                                                becoz of bank render the bal data from the cache, kartik see the old balance. due to which he blame on durga that you have not sent any money.
                                       we can't serve these data from the cache. becoz here the recensy is very important
                           -    we can cache the user profile info etc but something we still can't cache.  
                           -    Invalidation:-
                                        when we store something in cache we can store with an expiry, once the key expiries key get auto-deleted & when we do the new request then that key would be cached again. 
                                        what if we want to pro-actively populate or update the cache (we did in the crickbuzz). if we pro-actively update the cache then it called an invalidation. becoz we are marking the cache invalidate.  which mean we are deleting the key or updating the key. 
                                            we can possibly do both & none. it is usecase dependent. 
                                                In some usecase we can populate a cache right away. 
                                                In somecases, if we cache something at a particular place & we know that this particular record is updated so we can make a call to cache to delete that key.
    if we want the consistent data then we would not cache it, if we ok with stale data then we can cache it.
```
#### Diff layers in the infrastructure where we can cache things out.               
``` 
    1.  Client side caching:-       img 140.
            -   it is what the client holds.
                    eg:- client is connecting to a web server typically from a browser or web. so what if we use browser to cache something, what if we use mobile device to cache something.
                                like we cache in the browser side is something near constant data etc.   
                                like images, when we load images in the browser the browser typically caches those images. eg logo of a particular website. browser automatically caches the images of the website that we are requested. so next time we open the page browser doesn't have to do a 
                                    network call to load a image. 
                                like js files, browser automatically caches these.
                                like In amazon prime video whatever movie we see recently that browser can cache it so when we open again the prime video in the browser than we start the video from where we left out.
                                    so if we see the prime video in two diff devices then we can the difference. like In one device we get the diff movie which we was watching and where we have left out & in another device we get the diff movie  which we was watching and where we have left out. 
                                    becoz we are doing caching on client side.    we are getting the inconsistent data for the same amazon prime account, but it will gives the better experience. 
                                    as long as we are ok to serve the stale data. then we can cache things wherever we want.
                    if we cache the things at client side than it can boost the massive performance. becoz client is not making a req to the backend server, client is serving the request right away (at client side itself). 
    
    2.  Content Delivery Network (CDN):-        img 141.  
            -   it is one of the most interesting places to cache things out.
            -   CDN's are set of servers which are distribute across the globe (img 142). 
                    the idea is:-
                                if a user makes a request from a particular geography this request should go to nearest cdn server.
                                eg:-
                                        US folks are getting images from US server much faster then fetching it from india.
                                        basically let say we have hosted an platform (bunch of file, bunch of content etc) in india server (origin server). if someone form the US accessing what I have built, if my servers are in india & the request are from US to travel to india & then response send it back to Indai to US.
                                            so the request has to travel to massive distance to get the response, this is very time consuming. 
                                            what if we cached the things (which is not frequently changed) on cdn server which is closer to US. so when the req has made from the US, it gets to the cdn of US & immediately server from there. 
                                Note:-  we are not caching the business logic, we are just caching the static files (JS bundles, images etc).   like for static content why the US requests come to india.      
                                                         
            -   CDN stands on this principle "where a user makes a request get serverd from the nearest location not from the far location (origin server)".
                        origin server is where if the CDN server not have the data then where should CDN server make the request to. this is known as origin server.
            
            -   when we configure a cdn, what we have is ?
                        let say user makes a call to CDN (nearest cdn server), if cdn has the data then it serve it back (img 143).
                        if cdn does not have the data then cdn call the origin server (img 144) that to configure the cdn. 
                            so when we configure a cdn, we specify that 'this is our origin server if cdn server does not find the data then cdn server make a request to this origin server'.
                            assume we request for an image & cdn does not have that image then cdn make a same request to the origin server & get the image, then cdn server cache that image to itself & then send it back to the user as response (img 145, 146). 
                            when cdn cache the data, it has an expiry of that like normal caches have.
                            this is how CDN does Lazy cache population. we can do eager population but 99.99% of the scanerio we use lazy population.  
            
            -   best CDN server is Cloudflare.   
            -   eg:-
                        we have hosted our server in Sydney, we have configured our CDN infornt of my infrastructure (img 147).
                        assume a user from india who is requesting to our server so the request first go to the nearest cdn server like mumbai cdn server automatically. this is the magic of cdn like how the request go to the nearest cdn.
            
            -   CDN is nothing but an Geographically Distributed cache.
            
            exercise:-  watch video 13, from 12:00 to 12:55.
   
   3.   Remote Cache (Redis):-      img 148.
            -   Redis is actually a remote cache. means any cache which is centeralized but remotely accessible by anyone within a infrastrcuture.   
                    like a cache which is accessible by anyone over network within a infrastructure.           
                    
   4.   Database Caching:-  (database as a cache)   we use Relational db as a example.          img 149.
            eg:-    In a blogging application, user can publish multiple posts. we want to showcase the total no. of posts in the profile of the particular user. 
                        the way to do this :-
                                everytime we render a profile of the user & we make one query to the user table to get the info of the user & make another query to get the total no. of post in the post table. 
                                like make a query to counting the no. of posts everytime, it is an expensive query (if we do everytime).    what we can do ?
                                now we can do to speed the things  like we create an another col (total_posts) to the user table, where we store the actual count of the total posts (total posts is like caching). 
                                    means we are saving an expensive computation (expensive query) of my db by storing it in a precomputed way (we create the total_post col in the user table).
                                now whenever any post is created by the user, we would "eagerly go to the total_posts col & do the count++".        this is an eager population.    how ?
                                    whenever user create a post in the same transaction we would do 'update total_posts count of that user.  like update total_posts++ from User where user_id = 10.        img 150.
                                    
            with the help of eager populatio n & doing in the same transaction (to make them sync), we can able to save expensive query that we was firing.      
            this is also an example of caching.  
```
### we have to be very carefull into what we are caching, how we are caching & having an expiration to the cache.       we can't cache the entire db.

###     the more layers of caching we are add, the more staleness we are adding to the data. 
``` 
    eg:-    if we do cache something on the db & then we cache something on the Centralised cache & then we cache something on the loadbalancer & then we cache something on the cdn & then we cache something on the client side etc.
                when something get updated in the db, then we have to invalidate all of these Centralised cache, loadbalancer, cdn, client side.  it becomes very lengthy.
                
                img 151, 152.       just becoz we can cache everything that does not mean that we should cache everything.      we should know where we can cache, every single place where we have some ram, some disk then think of it as a cache.
```



