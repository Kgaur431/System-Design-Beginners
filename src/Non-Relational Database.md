### Rumor:-     engineers think that **All the Relational Databases are similar, that's TRUE. but it does not mean that All the Non-Relational Databases are similar**
``` 
    Ques:-  What is Non-Relational Database ?
    Ans  :-   it refer to classify all the databases that are "neither mysql, postgres, Oracle types etc. means that are not sql type of db" (img 98).
    
    Ques:-  what makes Non-relational databases interesting ?       img 98.
    Ans  :-   lots's of engineers think that 'Non-relational database are scale more that Relational db'.       ==>     not completely true.
                  In details explanation:-
                        Non-relational databases are coming into diff falvours (like casandra, mongodb, redis etc).
                            one thing about them is  that they shard things out of the box. 
                                like Non-relational databases they don't assume that 'everything would be there in one node / db'. & it gives us out of the box tooling  to split it horizontally. that's why it is very common misconceputation that 'Non-relational database are scale more that Relational db'. 
                                    In relational database they assume that 'everything would be there in one node / db' that's why the relational database provides the foreign key, cascade, trigger etc. 
                                    
    like:-  In relational db's, there is an way in which we can access things like sql queries, there is an way in which relational db store the data like rows & column (means table).
                but In non-relational db it's not completely true. becoz every non-relational db free to choose their own query || accessing layer.  their own storage layer, their own optimization, their own optimization, their own gurantees etc. 
                so it is very difficult to generalise the things in the non-relational db's. 
                
   it is not totally true that all Non-Relational Databases are similar.           
         
        we are going to discuss three no-relationsal db to understand the above line better.    we will see GraphDb, Key-value stores, DocumentDb (img 99).
        1.  DocumentDb:-
             -  it is closest to Relational db's. it is mostly json based.
                    eg:- MongoDb, ElasticSearch.  
            -   it supports complex queries. 
                      complex queries means aggregation. like In sql we can do aggreagtion (select * from table group by order by etc means we use multiple things in single query).
                      this is why it is closest to Relational db's.
           -    partial updates to document possible. 
                     let say we have a document (row) (img 100), whenever we want to increment the total_posts value for a particular user we just have to fire a query (like total_posts = total_posts+1) for the particular document & it will just update the total_posts field.
                        here we are updating the only one field for a document. this is know as partial updates. 
                        we can't do the partial update in all the db's.  
                    partial update is most common feature for most of the db's. but the lot of db's are doing like 'they read the entire document & update the field & then put the document back'.
                    documentDb is offering the partial update features. 
                        means we don't have to read the entire document to make a small update. we can issue a small update as part of the query & the documentDb will applied the changes.
                        it makes the db performent. it makes the network call very minimal
                            like assume this is an 1kb of document, first we have to read this 1kb document & update the particular field & then wirte back the 1kb document. so we are doing 2kb of data transfer un-necessarly just to update one field.  
                    real-world use case of documentDb:-
                        In app-notification service, like on linkedin when we get notification then we can see the list of notifications (not talking about the push notifications). those things are very well powerby documentDb becoz notification are unstructured like we can put any kind of data in it.
                             it still has a structure but we can put the extra data into each notification.
                            eg:- In linkedin there is an bell icon (img 101) we can click on it & we can see the notifications.  
                    img 102. 
        
        2.  Key Value Stores 
             -  it is very minimal in nature. means it is an simple databases.
             -  it restrict the access pattern to key value based access. 
                    means we can get a key, put a key, delete a key etc. but we can't do the operations on lot's of keys at one time.
                        which means it always a one single key based operation we are doing. 
                    eg:-    Redis, DynamoDb, Aerospike. 
            -   it have very limited functionalities.
            -   it meant to just search the key based access pattern.
            -   it does not support complex queries.
                    like we can not do aggregation. we can't fire complex queries on values like we can't go within the value & do something.  we can do only 'for a given key, do particular thing'.
            -   it gives us very high ability to shard & partition the data. this is the bigges feature of it.
                    becoz we know that the access of this db is very limited like key-value based access. so we can spin up 100 of nodes where we distributing the data & query it. means we can get very large scale in the data. this is why this db is widely adopted.
                    if we want to know more about Amazon DynomoDb we can watch video9 from 09:15 to 10:10. 
            real wold use case of Key Value Stores:- 
                    profile data :- if we provide the profile id then it will give the profile info. 
                    order data:-    if we provide the order id then it will give the order info. 
                    auth data:-      if we provide the auth token then it will give the auth obj. 
                    message:-       if we provide the message id then it will give the message info. 
                    etc. 
                    Key Value Stores are so much common.
            desiging Key Value Stores:-
                    we can use the relational db's & document db's as Key Value store,  In relational db's  primary key becomes the Key & any field aganist the key. that's all. 
            
            img 103.
        
        3.  Graph Databases
             -  whatever we are doing in the graph data structure like nodes, edges, vertices, graph algo's etc. so take all of these & bundle it in a database this is the exactly graph db is doing.
                    like  the graph algo we studied shortest path, least common ancenstor, traversal etc. graph database are specialized in the graph algo's.
                    wherever we see the graph usecase, we can model this data in graph formate (graph db).
                    eg:-
                            A follows B, Kartik Bought Ipad etc. we store it in a graph db.
                            anything we think that we can represented in graphdb, should not be represented in the graphdb. becoz relational db or key value store gives us the very good operation.    
            -   we should use graph db, when we want complex graph algorithms. becoz it will very difficult to build on relational db or key value store etc. 
                    becoz by looking anything that we can represent it in a graph. it does not means we should do it.
                    remember:- GraphDb are not optimal for Transaction, Iteration etc.   so when we need graph algo & we can model a data in a graph like way then we can use the graphdb.
            real wold use case of Graph Databases:-
                    Social media networks:-  like A follows B. so we can use it.
                    Recommendation system. 
                    Fraud Detection etc.
            
            img 104.
        
        
        exercise :- watch vide9 from 13:40 to end. 

```
### Relational Database gives us the strong gurantee of consistency, constraints, ACID transaction etc. but the Non-Relational Databases are not provides these things.  
``` 
    means if we don't use the foreign key checks, don't leverage the transactions etc. then we can scale Relational Databases horizontally. 
        with the help of sharding & partitioning.
```