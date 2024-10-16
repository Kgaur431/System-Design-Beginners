``` 
    always remember:-   whenever a database is created, the creater create the database thinking of solving one problem really well.        img 105.
                                          assume someone is desiging in-memory key value store then the usecase for that is like he wants to use it as a cached. 
                                                         someone is desiging the db where they want high consistency, integrity of data then it's relational db etc.
                                         so whenever the db is conpecutalised, it conpecutalised to solve one problem very well. 

    each db picks a small niche & there might be db's they are overlap in those niche.      img 106.
        so we should know what we are looking for, then we should pick the db. 
        
    misconception:-     we pick the Non-relational db becoz relational db do not scale.  
        there are 1000 of place where the relational db has scale very birliant way.  & there are 2000 of place where Non-relational db has failed to scale.
```
### why Non-relational db scale, lets understand.
``` 
        -   Non-relational db are able to scale becoz they do not have relations & constraints.     img 107.
            eg:-    In relational db, it gives us the foreign key relationships. 
                        like we can't create a post with a user_id, we can't add a user_id if it is not exists in the db.   these are constraints that relational db gives us.
                        so relational db would not work 'if the parent (user) reside in diff db & child (posts) reside in diff db. 
                            In this case how can we check for the parent that they reside in the db or not.     we can't do that.    
            that is where non-relational db, they don't offer us those constraints. so then obiviously they would scale.
        
       -    Data is modelled to be scaled.    
                becoz In Non-relational db, the way in which they define access pattern 'they start with an assumption that data is distributed across nodes'. means we would never be accessing the data that requires to make cross shard queries.
                if we think about it (above line), that we can do the same thing with the relational db. how ?
                     we don't use Foreign key check.    means we should desing the db model such that there is no foreign key check.
                     we don't use cross shard transaction. 
                     we can do manual sharding in relational db. 
                that's how we can horizontal scale the relational db.               {go deeper into it, if required}. 
        
       Does this mean that All db's are same || no db is different ?        (img 108). 
            No!!, every db has some peculiar properties & gurantees that only those db provides.        
                eg
                   peculiar properties of particular db:-
                        =   Redis is known for Advance data structure, it also know to be in-memory db that we can use as cached. we can't use mongoDb for cache purpose becoz mongoDb does not gives us that. 
                        =   Neo4j is known for Graph algo's.
                        =   DynamoDb is known for 'storing data in very sharded manner'.
                so every db has unique thing therefore as a engineer we need to know diff kind of db's which are there to offer to us. like we need to know the key properties of db that they gives us so that we can make better decision.          
```
### How does this help in designing system ?
```  
        Note:-  while designing a system always remember "never jump to a particular db right away'.    like we use the particular db without thinking of any db. we should not do this.        we should be very open minded.
         understand what we need to do.
                means we choose a db || select a db is the last thing, first we have to understand the properties that we need from the db & then we make a selection like this particular db is fits as per my requirments so we can use this db.  
                what all question should we ask to yourself ?
                1.  Understand what data we are actually storing in the db.
                        means what kind of data are we storing like Is the data structured || semi-structured || un-structured etc.
                2.  Understand how much of the data we are going to store.
                        means if there is too much data that we cannot handle then we need sharding. so we can't spend time on sharding things. if the db is too less that we can put in one machine then we no need of sharding so why pick the db which used sharding internally. 
                            that's why we while select the db we should know how much data we are going to store.
                3.  Understand how we will be accessing the data.
                        means what will be the access pattern as per our need. 
                4.  What kind of Queries we will be firing.
                        means how are we querying the data like do we want complex queries or not.
                5.  any special feature that only a certain db gives us.
                        eg Expiration, bloom-filter etc.
        depend on these we can pick the correct db for our system.              (img 109)
```
###     On diff situation which db we should pick ?
```     
        1.  if the data can fit in a single db / node and we "need strong consistency & data correctness is very important".      ==>         we can go for Relational db (img 110). 
                eg:- Payment system. 
        
        2.  if the data can fit in a single db / node and we "need complex queries, aggregation".      ==>         we can go for Relational db (img 111). 
        
        3.  if the data can fit in a one db / node and we "want key value based access & we want it to be very fast ".      ==>         we can go for Redis db (img 111). 
                means when we know that we want the data accessing very fast means data need to be there in-memroy given it's key value.                hint :-     key-value & in-memory.
        
        4.  if we "want the advance data structure".      ==>         we can go for Redis db (img 112). 
        
        5.  if the data can't fit in a one db / node and we "have experties in sql & we can do manual sharding".      ==>         we can go for Relational db but droping the constraints(img 113). 
                means when we have very huge amout of data.
                like we don't need any foreign key checks or any constraints.
                we can shard our Relational db (manual sharding) so my api server knows to which shard do I have to connect to.   
            for this scanerio (We can't store the data in one node) people go for Non-Relational db but we choose Relational Db becoz me & our team has expertise in sql so we don't want to spend time in learning a new db. we can leverage the exisiting db (Sql) & existing experties in writing query in sql
                 therefore we use Relational db.
                   
        6.  if the data can't fit in a one db / node but we "have simple key-value based accessl".      ==>         we can go for DynamoDb || MongoDb (img 113).            
                like MongoDb is document based how it matching with our requirments. 
                           we should not use document based things, we can store normal value in the json & we have a key (id) so the problem solve. 
               
               this is how we should think,         there is no strict line that is being drawn.  
                    like we have simple key-value base access & one node can't handle the data. so any db's which is sharded & persistance we can go for it. 
        
        7.  if we required very sophishicated graph algo's.         ==>         we can go for graphDb || Neo4j (img 113).
        
        8.  if we nothing specific || no specific requirments but we don't know that what lies in the future.           ==>         we can go for Document base db like MongoDb (img 114).
                like we think that there is something that we might need in future. 
                        we don't use the sql not becoz that we don't have expertiese in sql,  but we know that our data is very huge it typically need to be sharded (means we need the sharding out of the box) but we don't have any specific requirments. we just want to be future proof. 
```