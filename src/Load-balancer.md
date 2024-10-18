``` 
        let say when we want to scale the particular api server so we just simply put the load balancer behind it.                                  load balancer == lb
        -   it is very essential in building horizontally scalable system.
        -   img 204.
        -   it is the only point of contact.
                means whenever a user reach out to any one of the backend server, they have to go through load balance. what load balancer gives us?
                    =   it gives us a single point of contact to every request for that particular api-server. 
                    =   let say what if we don't have any load balancer ?
                                we have bunch of api servers. it would be an client responsibility to know the ip address of api-server to talk to.   now how load balancer handle it ?
                                     load balancer typically have static IP address or static DNS name. with this what happen ?
                                        the clients only need to know one thing to talk to it.  they don't have to know about what all api servers we have. 
                    =   it is abstract out the things(api servers) for the client. 
        -   Whne we have a load balancer, How does the Request flow look like ?   img 205.
                =   Client would already need to know the ip address or domain name of the load balancer. once the client have this info then what happens? 
                         client makes the request to the ip address, the request comes to the load balancer. now the responsibility of the load balancer "forward the request to one of the api server that it has depending on the load balancing algo".
                            eg:- Domain name of the load balancer is :- auth.example.com     
                        then the api server would send the response that response comes to the lb, lb responds back to the client.
                =   here the client does not means the android app, ios app, web app etc.    client can be another service. 
                        like auth service wants to talk to profile service.     the api servers of auth service would make call to lb of profile service & get things done. means auth service does not need to know about the ip address of profile service. 
                        so when "any two service communicates that means one api server & one lb are communicating".
                        this is how lb abstract out the distributedness of the system.  
       -    the name suggest its an lb, so the job of lb is 'balancing the load'.       img 206.                    
            how it balancing the load ? 
                =      it balancing the load using some algorithm.
                        the main thing is 'picking one api server & forward the request to', how we can pick the server.  
                            that is done by load balancing.     
                        
                        1.  Round Robin:-               img 207.
                                =   it is the most basic algo.
                                =   whenever a request comes to lb, lb picks the first server to forward the req to, when the second req comes then lb picks the second server to forward the req to, when the third req comes then lb picks the third server to forward the req to,
                                        when the fourth req comes then lb picks the first server to forward the req to, when the fifth req comes then lb picks the second server to forward the req to, when the sixth req comes then lb picks the third server to forward the req to,and so on (img 208). 
                                =   as soon as the new api server added then it will get the req in the round robin fashion.
                                =   this way we can see a near 'uniform distribution of requests' happening across the infrastructure.
                        
                        2.  Weighted Round Robin:-      img 209.
                                =   let say we have three servers.
                                =   the way in which we have defined that servers like first server has 4gb ram, secod server has 8gb ram, third server has 4gb ram etc.    means the insfrastructure has not uniformed. 
                                        if we do round robin (where the insfrastructure has not uniformed) then we would distribute the requests like:-
                                            first server               <--        first req,  fourth req, 
                                            second server         <--        second req, fifth req, 
                                            third server              <--        third req, sixth req, and so on.
                                        there is an problem in that. 
                                        our second server is 2x large compare to first & third server so it has 2x capable to handle the request compare to first & third server. means second server has double ram compare to first & third server. 
                                        so we are underutilising the servers, if we send the N request to the second server. 
                                        if we are sending the N request to the first & third server then we have to send the 2N request to the second server so that we can utilise it properly. 
                                        so we have to round robining the request in the ration of 1:2:1. this is all about the weighted round robin.  
                                =   In Weighted Round Robin we can define the weight of the server specially when the infrastructure is non-uniformed (like the api servers not has the exact same configuration).
                                        now the request would go like this:-
                                            first server               <--        first req,                                fifth req,                                          1
                                            second server         <--        second req, third req,        sixth req, seventh req                   2
                                            third server              <--        fourth req,                            eight req, and so on.                     1
                                                
                        3.  Least Connection:-          img 210.         
                                       everytimes the req comes to the lb, lb make a req to one of the api server. once lb get the response it return to the clients.      when it done then the HTTP connection get ended || terminated.       like HTTP, REST works like this manner.
                                       when the above thing happens, it means that 'the no. of connections that the lb has to one of the api server determines how busy the api server is'.
                                        so In least connection we are picking up the server that has least connection from lb, which would means that this server is more likely to be 'free' from serving the exisitng the req.   that is where what lb do ?
                                        lb picking up the server which is relatively free as compare to other server.   this way we are minimizing the response time || the load on a particular server. this way we always pushing the req that is having least connection from the lb.
                                        this is all about Least Connection.                 watch video17, from 08:00 to 10:35.  
                                        
                                        eg:-    when the service (api server) that we have written, have very high variance response time. 
                                                    means let say we written a registration service, some http req might takes 1 sec, some http req might takes 5 min. this is very high variance. 
                                        let say we have very high variance, 'we would typically want to send req's to a server that is relatively free'.    so here we are always pritorizing the servers that would free up. 
                                        
                                        the degree of freeness is propotional to no. of connections that lb has with the api server.      
                        
                        4.  Hash based routing:-            img 211.  
                                =   when the lb gets the req, depending on some params (userid, ip of the user, url etc) it would pick the param & doing the hash of it & then do the mod of no. of api servers that we have then it would get one of the server & then lb forward that particular req to that server that we get by doing mod.
                                      becoz it is deciding based on hash function, hash funciton are deterministic so for the same param, the hash fun would give the same o/p. means we can build sticky sessions.
                                        what is stickiness here ?
                                            all the req of a particular user are go to a specific api server, we just have to used hashed based algo into the lb.   
                                            so it would be sticky w.r.t the param that we are passing in the req. 
                                                let say param is userid so all the req of a given userid are go to a specific api server. 
                                            userid passed through the hashed fun & get some no. & then we do the mod to the no.(which we get from the hash fun) with no. of server.
                                      when we want some stickiness then we can go with Hash based routing.
```
### advantage of load balancers
``` 
        1.  Scalability:-       img 212.
                        lb is abstracting out the distributedness of the api servers. what we can do ?
                                we can add more servers behind the lb.
                        eg:-    we want to handle 200 request per min (rpm). we know that our current api servers can handle 100rpm then we just add one more server and then we would start handling 200rpm (img 213).
                                        now we want to handle 300 request per min (rpm). we know that our current api servers can handle 200rpm then we just add one more server and then we would start handling 300rpm (img 214).
                        scaling the api server is big things so becoz of lb we can seemlessly horizontally scalling the api servers whenever we want without doing any complex. 
                            this is where the lb abstract out those complexities.   how ?
                                becoz lb are only point of contact to the api servers. 
                               
       2.   Availability:-      img 215.
                       lb are abstracting out the distributedness of the api servers. if any of the api server goes down then lb will not forward the req to that api server (img 216). the lb would send the req to the another servers that are available.
```

``` 
    Exercise:-  video17, from 14:45 to end.
```
                                     



