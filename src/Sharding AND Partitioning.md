``` 
    img 75.
    Sharding:-      it is all about databases.  like when we have multiple databases then we do sharding.
    Partitioning:-  it is all about data.   like when we spliting the data the we do partitioning.
```
### How a database is scaled ? 
``` 
    here we are try to understand that when to use Sharding or when to use Partitioning.
    
    eg:-    let say we have mysql db that is running within the EC2 machine, with an expose of the port 3306 (img 76).  so if any of the request comes & access the db on the given port.   
               every database has a limit that the no. of request that it can handle. 
                    assume we have a small database which handles 100wrtie per second (img 77).     now we get the more load so what we can do ?
                        let say now we want to handle 200wrtie per second, so we can do two things. 
                            1.  we could scale up the db. means we have a small database which handles 200wrtie per second, here we have not added the another db, we have just added more cpu, more ram to the existing db so now it handles the 200wrtie per second (img 78).
                                 assume Now we are unable to handle the load becoz we are getting 1000write per second (img 79).    we are using AWS EC2 so we can upgrade the db within 2 clicks. this is the final upgradation that AWS offered.
                                 now we get even more traffic. now the AWS not provide the upgradation any more, becoz we reach the certain limit so we can't beyond it (img 80). 
                       Now we do Horizontal scalling.
                                let say we have 1 database which is able to handle 1000write per second, what will happen if get the req more than that like 1500write per second (img 82). 
                                    so "we will distributed the data across two databases". 
                                        means we have put the 50% of the data into one db & remaining 50% of the data into another db (img 81).  we were getting the request as 1500write per second & our db has capacity to serving 1000write per second and 
                                            we have alocated the 750write per second into diff db's. 
                                            means our db has a capacity to server 1000write per second so we have shard the database into two database with the configuration of 1000write per second. & we was getting the request of 1500write per second, so we have partitioned the request (data) into 50% each.  
                                            (line 22 is my understanding). 
                                            
                      summary:-  In vertical scaling we are able to handle maximum 1000write per second. but In horizontal scaling we have split the data into two, now it able to handle 1500write per second & we can go beyond it like 2000write per second. becoz each of the db                    
                                            can handle 1000write per second. as of now we are handle 750write per second in each db.
                             
    this is what we called as Sharding & Partitioning. 
        we partitioned the data / split the data 50-50%, put both the into the diff diff shards (img 84).  each db server where partioned data has serverd is know as sharding.
        like:-  the data has partitioned, & we put the these data into multiple db servers this is know na sharding.
                            Database has sharded        &   Data has Partitioned.
             means we have multiple db servers &    we have split the data. 
 
    img 84.
    
    eg:-    we go deeper into partitioning.
            let say we have 100gb of data, which we are partitioning in 5 partition with the help of some strategy (img 85). 
            what we can do now ?
                we can choose to store these partitions across two shards. like partition A & C can live in the Shard1. while partition B, D & E can live in the Shard2 (img 86).
                this is our business logic so we can put the partition wherever we want. so long my api server knows that if I am getting the req for the particular key which is part of the particular partition & that partition is the part of the particular shard. means as long as the api server know how to route
                    the req to the corresponding shard. we good to go.
            
            assume the shard1 gets hot then we can just take the partition of that & put into the shard2. means we are doing the load balancing (img 87).
            
            img 88.
```
### How to partition the data ? 
``` 
        it can be done in two ways. 
            1.  Horizontal Partitioning:-
                    -   it is the most common way of doing partitioning.
                    -   it says that "within one table / collection we can pick some rows put into one db & pick other rows & put into another db"  this is horizontal partitioning.
                            means we are not 'spliting a row' instead we are 'taking bunch of rows' & put into the db.  
                            like we are doing slicing of rows horizontally.         ----------  
                            
            2.  Vertical Partitioning
                   -    it says that "we have 2 tables in one database & 3 tables are in another database". this is vertical partitioning.
                            means putting the tables into diff diff db's.
                            like we are doing slicing of tables  vertically.         
                              db1        db2
                                | |     &   | | |  
                   -    vertical partitioning is an example of Monlithic to Microservice arch. 
```

``` 
    demonstration:- 
           -    let say we have partitioning on the X asix (img 89). 
           -    let say we have sharding on the Y asix (img 90). 
           -    when we are not doing partitioning the data & not sharding the database, it means that we have one db in which we having one data (img 91). it is like a standard monolithic arch or we can say like everything is in one database so every request goes to only that db.
           -    when we do the partitioning of data & not sharding the database, it means that we have one db in which we have two sub-databases each of them having the partition of data (img 92).
                   eg:- let say we have running two microservices, first is Authentication & second is Profile. so we have create the database for Authentication & Profile both.
                            so the Authentication db that we have created is one partition & the Profile db that we have created is one partition within the same database.
                            here we are not sharding, we are partitioning. so both the subset of data are still lying within the same db.
           -    when we do the partitioning of data & sharding the database, it means that we have two db's (img 94) in which one db store the first partiton & second db store the second partiton(img 93).
                   eg:- we have talked about two partition, first is Authentication & second is Profile. imagine we have took the Authentication database & move it into diff db.  
                            for Profile database we have moved it into another db.  like we have partitioned the data & put these data into two diff db.                                        {previously we have put the both of the partition in single db} 
                            now each of the database can handle good amount of requests independentaly. 
           -    when we do not partitioning the data but we sharding the database, it means that we are not spliting the data neither horizontally nor vertically,  we are just using the multiple db's (img 94). 
                        like we are using the Read Replica, means we have same data reside across the two db's. 
                            this is how we scale the read request. 
                        so no partitioning but sharding is the good example of Read Replicas.
                
``` 
#### img 96.
### Advantages of Sharding                                                  
``` 
        1.  we handle large no. of Read & Write requests. 
                like vertical scaling has a limit, we was only handle the 1000write per second. but when we do the sharding we can able to handle the 2000write per second.
        2.  Increase the overall storage capacity.
        3.  Higher availability. 
                like if one of the db has go down then system has at least partially functional.
```
### Disadvantages of Sharding
``` 
        1.  operatinally complex.
                like we have multiple shards, we have to write the routing logic in web-server.     
        2.  Cross-shard queries expensive.
                    we can't make cross shards query, it is very expensive means for single request we have to communicate with two db's that would slow down the things, we can't acheive the consistency, for that we have to go with distributed transaction to achieve consistency.
                generally the engineers try to minimize doing cross shard queries.  they use shards but in most of the cases the way in which the data has sharded || partitioned is such that they would never have to make the cross shard query. 
                    this is how the senior engineers decide on which parameter we have to shard the database || partition the data.     this thing is the heart of any distributed database.  


```

