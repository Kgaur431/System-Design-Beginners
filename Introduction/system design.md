## System Design
###  System design in general  is **we have some set of requirements & we want to build a product out of  it** this is  about the system design.
``` 
    let say we want to design a system. we are trying to say that 
        "we need to decide the architecture of the system", 
            like If we want to build the Facebook then what all things would I required. 
            eg:-    we required DB to store the data, we required Servers to server the request, we required CDN to servers the Images and all. means we are talking about the High-level Architecture of the system. 
        
        "we need to decide the Components"
            means if we talk about the facebook that means we are going to design entire Facebook, that is our entire system.
            but In Facebook there is one module which will take care of Authentication & Autherization.
                within the Authentication & Autherization we have one web server, we have one Db, we have one cached. so these are the components within it.
        
        "we need to decide the Modules"
            means while we implement this Authentication & Autherization what all modules do I requried.  
             
    so In system design, we typically talk about these three  design architecture, design Components, design Modules.
    when we design these three ( architecture, Components, Modules) How diff parts of the system (that we design) are interact with each other to solve the bigger problem (to build Facebook). 
        means we will solve individaul problems & we define how all of them interact with each other that makes our system almost complete.
    this is how product developement happens in any organization. like we decide the architecture, decide the Components, decide all the Moduels.
```
###  Why System design is so popular ? 
``` 
    Becoz every single tech product is a system that has been designed. 
    companies are building product and need all of us to design it.
        means companies want to build a system & we as a engineer / product manager etc. we have to design that product.
``` 
###  Why understanding system design is important ? 
``` 
    It is really important to understand, becoz 
        1.  what people do at work:-
                engieers / product managers etc. this is what we have suppose to do when we join a particular organization. like we have to build products, we have to solve real world problems.
        2.  everything is practical. 
                means we solve problems using real things like Databases, caches, cdn's, server, compute, encryption-decryption etc.  this all practically done in the org.    
        3.  we can see it from day 1. 
                means after joining the team. the first thing they given to me is like this is the products that we are going to build, these are the things that we tried, this is the architecture of it, these are design docs of it etc. I have to go through it.  
                this is what about the system design.
        4.  as we grow in our career, we will spend 80% of time doing this. 
                means system design is something that is really very close to what we do in a day to day life.    
                    means we are not solving something which has made up, we are sovling that something actually being done on day to day basis. 
                Implementation we will still do. as we grow in career, its all about system design.
                    like 'how we are designing the best architecture that would help companies to build best solution with minimal cost'.  
```

##  In system design we solve real problems. clients are using that product.    when we design a sytem then we learn how to break down a big problem statement into smaller solving sub problem statement. like we solve individually in efficient way & then combine them to solve bigger problem.
##  system design will rewire our brain to think in a structured way. when we think in structured way then it would help us not only in to designing good system. it would help us in other aspects of our life as well. means **it makes us a Better Thinker in general**. 

### what will we do, when we desing a system ? 
``` 
    we break down a problem statement into solvable sub problem. then we decide the key responsibilities on each component that we have. we decide the boundaries of each component (like if one component is taking care of authentication then why should other components take care) means we don't want 
    to have two components doing something very similar, we just reuse it. & for every single component  that we have, we talk about the key challanges that comes in scalling it. while we make the system scalable, then we are not just talking about the scale but also about the fault-tolerant & availability.
        assume when small component goes down then how our system recover from that. 
    these are very important when we desing real system.   
```

## How to approach System Design ?
``` 
    let say product manager has thrown a real world problem, Now how would we apprach that. 
       like it seem overwhelming becoz so many modules, so many things to decide and so on. 
       
       0.   always keep in mind that "System design is exteremely practical". 
                whenever we are designing a system then "think in a very realistic manner". means we have to pretent / feel that I am the one who are building it, who are going to code it out. then how am I design it.
                means we have to think in a very practical way & then we tackle one problem at a time.
                eg:- Arpit way of doing this is "Take baby steps, no matter what!". means he has taken really a small steps at a time & eventually he build a very good system. 
        
       1.   Understand the Problem Statement
                we should have very good understanding of the problem statement. becoz if we don't have  then we will go in any random directions. means we are not focusing on the problem that needs to be solved.  that's now we should do.
                so whenever we have given a problem statment then understand what we are suppose to do, understand the constraints of the problem statement. 
                eg:-
                        lets design URL Shortner which would handle 100 million requests per month.
                        we feel like oh my god 100 million request per month.  
                            we are suppose to only handle 100 million req per month but if we are building systems that handles 1billion req per month, it easy to digress.  means we are doing over-engineering when we designing the system. 
                                becoz when we have given a certain a constraints & we live within that box (constraints) then we design highly optimised system & if the system is highly optimised then we have to beat 100 million, 200 million, 300 million requests, 1 billion etc. 
                                it some small changes here & there. & we could be able to handle that scale.
                            but it is very essential for us to limit yourself into the constraints that we have. 
                                means we have to ask the ques from ourself that 'whatever system we are desigin', 'how we are designing', 'what are constraints', 'what are the cofeatures that we definately want to have' etc. & draw boxes arround these questions. means we don't have to digress from those things.
                means build the solid understanding from the problem statement.
       
       2.   we break the problem statement down into components (really essentials) 
                lets say we have to design a system which is as big as Facebook. the first thing that should comes in our mind is 'what are the features of whatever we are desiging (facebook)'. 
                so we have to break the problem statement becoz no one can tackle the entire facebook in one shot. it really important to split that into multiple components / features. 
                    eg:-    we design facebook, so there would be one component that will take care of Authenticaiton, one component that will take care of Notification, one component that will take care of  Feed etc.  here we just took 3 simple features that we use big enough to be its own Microservice.
                                then we tackle one at a time.
                                
                summary:- we are trying to say is "if the problem statement is big enough like facebook,  then split it into features".   
                                    once we do it then we should pick one feature at a time & go deep into it.
       
       3.  Dissect each component (if required) 
                when we pick up the component then we dissect the component & then go deeper into it.
                let say we picked up the Feed for it implementation, now think In order to implement the Feed what would I need. 
                    we definately need one web server (who is serving the feed),  we need a database. 
                        like whenever the user comes online means user would make a request then web server will talk to the database & gets the feed & sends to the user.  
                        we still not designing it, we are just breaking it into smaller sub-components if we requried.  like web server, db etc. 
                        that's how we solve the reading part of it. means how user can get the feed. what about the writing part || creating feed ? || who would write the feed into db ? 
                            we need the genrator (who is generating the feed for us) . means we don't know how it will generate the feed, we still not decided. we just drawn the boundary like the webserver is the one who serve the feed from the db, there has to be something that is putting the feed in the db.
                            so we need the genrator that will put the feed in the db. like we have drawn the line towards db (06:19 - video3)
                        once we have designed the enough systems (like webserver, db, generator) then we realise that, let say we might want to aggregate my feed like there are so many posts which we want to aggregate in one post || we can filter things out to optimise. 
                            so we need the optional aggregator (it will aggregate the feeds for us, it will filtering out the feeds for us etc). if we don't sure about the functionalities of aggregator then we can skip it. means its better to not draw a boxes for the shake of it.       
                                assume we are not confident about something, 
                                    like we know about the webserver that will serve the feed from db,  we know the generator to generate the feed, but we are not sure about the aggregator we just came up with the fancy word called aggregator & we added in the diagram but we are not sure that what it will do. 
                                    so when we are in doubt then we have to skip it.
                                    becoz when we go deeper into each of the sub component then we would realise that 'this is something that we will need', then we can add it. 
                this is how we think in a structured way.
                        means whenever we are in doubt then skip it, once we go deeper in the sub components then we would realise that 'something like this need to there' then we can add it. so "drawing boxes for shake of it" is not good.
                        as of know we removed the aggregator.  
    
    4.      for each sub-component look into
                once we go into the enough depth, now its time to make the technical decisions. which is where we take these 4 technical decision for any component that we have.    
                first technical decision that we have to think about is:-
                  1.  Database and Caching
                                like how are we storing the data, do we need caches if yes then where would it lie, how would the sub-components talk  to that etc. 
                                so first decision that we take in the sub-component about the storage. 
                  2.  Scaling & Fault Tolerance
                                like how would the web-server scale (if we have one server & it get 1 million req then it will not able to handle it so we need have multiple server, we have load balancers). then about the fault tolerance (what if the db craches, what will be the strategy to recover from it.)  
                                these decision we need to take.  so when we talk about the scaling & fault tolerance then we should consider it  for every techincal things that we are adding, think about those. like how would each component scale, how would each component be fault-tolerance.

                  3.  Async Processing
                                we take decision like:- if the webserver want me to generate the feed, does it need to be done while user makes a request || we should generate the feed before the req made. 
                                like user login into instagram & insta taking 5 minutes to load the feeds becoz we are generating the feed while user login into the insta. this is not an good idea. so we will do it in async way like behind the scene it is generating the feed & keep it ready for user.
                                this is async processing.                   
                  
                  4.    Communication
                                how diff components will communicate with each other. here comunication means we are talking about the technical details. like TCP, UDP, HTTP, Web-sockets, GRPC etc. 
                                like how would webserver communicate with database, how would generator service & webserver is communicate and so on.
               
                these are the 4 things that we would be looking at into all the sub components that we designed.   becoz that's when we know that we are designing the good system.
                
    Now we have went into the details of each of this (webserver, generator, db, aggregator). but now we might find a need in case of generator  like we are not sure about the generator means we can even split the generator into something more. becoz the server is not an stand alone thing.
    it would need to read from other places (generator need to read something from other places to generate the feed). so now we start to think of 'do I need to split my generator into even smaller sub components & each one having its own responsibilities'.      
    we are trying to understand is:-  if we find a need to split a component which is generic enough as a generator. how do we write a code to implement a generator. 
        when we go into these details then we got to know that ' we might need  that is something reading from other services & merging it. after merging, there is some db where we storing some temporary info. now it (14:00 - video 3) becomes a generator. now generator have 2-3 sub components.
        it needs interaction (from postservice, recomendation service & follow service to the merger). becoz for feed-generator to generate a feed, its need to know something, it needs to use info from recommendation service, it needs to use postservice to populate the feed). 
        so we take the info from all the sub components & merge that info & put into the feed db that is used by webserver.
    therefore when we start thinking of implementation, we realise that these are the diff things we would need to consider.        
    
    we can implement webserver in any of the lang like go, java, python, nodejs etc.
    
    "approach it in a very structured way, always better to go thinks top-down when we given a big problem".
        like design facebook. we start with big features & each one of them down then solve it.
        if we given a very specific problem then we go with bottom-up, like start designing the db.  
        this is how senior engineers apprach system design in general.        
```
### every system design problem is extremely practical. it needs to be approached in very structured way. it is something that we need to build the intution.


### How do we evaluate that we have built a good system
``` 
        as of now, we have skipped. 
```
    



















```