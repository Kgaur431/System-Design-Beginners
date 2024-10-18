``` 
        Circuit Breakers (cb) prevent "Cascading Failure".   
        Circuit Breakers are like as Load balancers (lb).
            when we designing a system for a Resiliency.  like we would want to minimize the time that are having complete outage.      partial outage are happening every now & then. but we don't want the complete collapse of our infrastructure.    cb are playing very important role in that.
        
        img 217.
            Cascading Failure means 'failure in one that leads to failure other & faliure in that leads to failure other and so on".    like it's cascaded downwards.   this is all about cascading failure. 
            Practical example to understand the what is Cascading Failure:-
                    let say we are building the social network (where we are focuing on feed of the particular user like we have feeds that is being served to the particular user). 
                    try to understand the flow:-
                        the Feed service depends upon the Recommendation service.                                like we might have some Recommendation service which is generating the feeds that we can put into the particular users feed. 
                        the Feed service also depends upon the Trending service.                                      like we might also want to put some feeds which are trending, that we can put into the particular users feed. 
                            the Trending service depends upon the Post service to get the posts.              like trending service generates the id's of the posts which are trending.
                            but the Post service needs Profile info to get the profile id. so the Post service depends on the Profile service.   
                            the Recommendation service has the posts that should be there. but it also dependent on Profile service to get the profile info.    
                            the Profile service is depend on the profile db, from which it getting the user details.     
                            the Post service is depend on the post db, from which it getting the posts details.     
                    all of them are combining to feed & then the feed serve to the user (img 218).     
                    
                    Now let say due to some reason the Profile db under heavy load. what would happen ?
                        the req is being made by Profile service takes longer than usual to get the profile info (img 219).
                        now the Profile db is overwhelmed then the Profile service response time will go high.  therefore any services which are depending on the Profile service would become slower.
                            like:- Post depends on Profile, Profile service become slower.
                                     Trending depends on Post, Trending service become slower.
                                     Trending depends on Post, Trending service become slower.
                                     Recommendation depends on Profile, Recommendation service become slower.
                             we can ok with slowness na.    what should be the problem ?
                                the problem starts when it becomes too slow.    means it it becomes too slow that means our "HTTP connections are open for too long time" that means we are hogging/collecting the tcp connections there. 
                                means the req's are not terminated & every machine has a limit on the no. of TCP connections that it can handle.    when it happen?   
                                    like whenver we establish an HTTP connection with other service, there is always a timeout associated with it. 
                                        let say the timeout is 10sec. when the service is slowing down then the req that are not served in 10sec then those connections are broken.
                                        now slowly what happens is ?
                                            the Profile service are getting down & the request are break over here.
                                            the Recommendation service are getting down & the request are break over here.
                                            the Feed service are getting down & the request are break over here.
                                            and so on.
                        so the slowing down the db would starts to break everything slowly.  
                        assume the situation:-
                                the incoming request are comming continuously (img 220), this would amplify the slowness & entire infrastructure become suffered. and slowly it would become complete outage.
                    This happens "when a one service is transitively depends on the other service".  
                    so slowness In one db transitively cascade the failure to colapsing the entire infrastructure.
                    
                    How we can prevent it ?
                        we could prevent this like:-    
                            If Recommendation service knows that there is an outage at Profile service so Recommendation service should not make a req to the Profile service. 
                            Hypothetical usecase:-
                                if it is possible, Recommendation service can serve without making a call to Profile service. then we go for it.
                            this is all about the Circuit Breakers.
                   
                    "whenver we are making a call to the service, we would only make a call only if the service is healthy, if the service is unhealthy then we would not make a call".
                        so instead of making call, we can return some default stuff if the service is unhealty. 
                        like when the Profile service is unhealthy then the Recommendation service render some default profile (may be it can take the default profile from the clients side & render it).
                        the sol for the default value would be depends on the usecase.
                        
                        "Recommendation service serve the recommendation without the profile info || it serve the recommendation with the default value".  
                        img 221.

            How we can implement the Circuit Breaker:-      img 222.               
                        the idea is :-
                            we have a db in which for every service we are storing the healthy status for it. like yes no.
                            it is not automatically populated, someone is saying that this service is healthy, he want to call to the service, at that moment we can call the service becoz it is healthy. assume it's unhealthy then we can't make a call.
                            any service before making call to another service, checks this db & confirm that the particular service is healthy or not.
                        img 223.
                            here Feed service has a seperate db called circuit breaker db.   like we can use key-value pair, relational db  where id is the key & status is the value that's it.
                            keep qeurying to this db, before any service make a call to the any other service.
                            every service is talking with circuit breaker db before any of the service make a call to another service.
                            let say we know that profile service is down then we can update the value of the status in the circuit breaker db. 
                                when Recommendation service make a call to the Profile service, Recommendation service would first checking the status into the circuit breaker db. if the status = falue then Recommendation service skip making call to the Profile service.
                            this is how we are prevent our infrastructue from getting collapse.
                        
                        this is the manual way of using Circuit Breaker, but in comapnies we can find it automate.
                        img 224.



```