### if we know the database well then assume 70% of the problem we solved.  
``` 
    -   Database are most crictial component of any system. becoz most of the response time, most of the latency etc. if db goes down then our system goes down.
    -   In relational Databases Data is stored & represented in "rows and column".
    -   when something revolutionary comes up, it always finds it's first application in financial domain.
                means when computer was invented then the first set of program was written are all accounting programs. earlier people was doing accounting on sheets of paper which was literaly arranged in rows & column (like microsoft excel, google sheet).
                when internet comes up then the banks were go online for payment purpose. 
                when blockchain comes up then the first application was crypto currency.
                so something revolutionay comes then it would always find it's first set of application in financial domain.
          means when db was created they have created to solve problems of financial system. 
          the first database was created for the financial system. becoz the database handle financial data, the kind of properties of database provide were related to ensure that financial system work fine.
          like financial system need the
                data consistency, data durability, high data integrity, constraints, everything to be in one place.     img1.  
                that is why these are the very common trades of the RDBMS.
   -    In RDBMS there is something called as "Transactions". transactions are something that help us to build systems which are correct.     
        the reason of RDBMS become famous, becoz it provides the ACID gurantees.        img2.
        
   -    Atomicity
            "it implies that all the statements within a transaction takes effect or none of them takes effect". 
                let say In Financial system if we want to insert a data in two diff tables. we can wrape it in a transaction such that either both of them happen or none of them happens. 
                    it would never happens that we made a entry in one table but before I was able to write in another table my system crashed. now having a inconsistency state.
           this is what atomicity all about.
           
           means no matter how many statements(insert, update) we execute in my transaction. either all of them would be executed or none of them would be executed.        img3. 
           atomicity is very important property of any RDBMS.
           
           eg of non-financial domain, where RDBMS used, 
                      assume we are build a social network, whenever a user publish a post then we want to create a entry in the post table & also we want to update the count of the total post in the post table for the specific user.
                        here we are doing two thing, one is inserting the data into post table, second is update the count of total posts.  
                        Ques:- what will happen if we don't put both of them into transactions ?
                        Ans  :-  assume we write an entry successfully in the post table then before updating the total_posts, our system has been crashed. means we inserted the post successfully but we unabel to update the post. 
                                        that means for a particular user there has a 5 posts but the total_posts count is still 4. so if user scroll the profile where he can see total posts is 4 but while scrolling he got to know that there was an 5 posts.      means there is an inconsistent data.
          
          we just have to wrap all the statments within single transaciton. 
            like 
                    start transaction
                        statement1
                        statement2 
                        .....     
                        and so on. 
                    commit 
                    
                    when the commit command execute then all the statements will be executed otherwise none of them will be executed.  
                
   -    Consistency:-
            In financial system, it is very important to have the user data in a consistent state. 
                assume user1 should not be allowed to transfer money from one account to another account in a situtation where 'let say user1 have only 100$ in their account & he should not allow to transfer 200$'. 
                this is consistency. 
                no matter what happens, my "db always move from one consistence state to consistence state" . 
            the way in which RDBMS provides the consistency is through **Constraints**. 
                means we can define constraints on RDBMS. like Check constraints, Foreign key constraints etc. depend on the Db provider (Oracle, MySql, Postgres etc), go through the documentation & see the diff kinds of constraints they provide. 
                & when we use them then no matter what happen our data will not go in inconsistent state.
           
            we can use Constraints, Cascades, Triggers. 
                Ques:-  what is Cascades ?
                Ans :-   it suggest "if we delete a particular row then we have to delete everything associated with it". 
                                eg:-    let say we have foreign key relationship b/w two tables (Users-child). whenever we create a post, the post is created for a particular user. Now if we delete that users entry then all of the posts that were written by the users are now orphaned.
                            so if the "User get deleted then all the Posts related with that user also get deleted, means we don't have to explicit write the delete operation for posts".
                                like:- if we define the cascade:- On Cascade Delete.  
                
                Triggers:-  when "we specify the triggers".
                                        like On Update Trigger this (Thing which we want to do when update happen like invoke a function).  
                                        eg:-    when a particular row is updated in the table then updates the particular block. 
                                        eg:-    whenever any entry insert in the post table then we want to update the total_posts col.     img3. 
                                                    we can do this using Trigger also or transaction.
                                                    we prefer using transaction. becoz this is our business logic (when insert happen then update the particular field) so we have an control therefore we have to use Transaction. 
                                                    we have to use Trigger, when the logic specific to the database. 

   -    Durability:-
                imagine in financial system, we can't say like our data has been vanished. becoz if the transaction is commited, no matter what happens (like machine crashes) as long as the hard disk is there, the data should be durable. 
                
                "when transaction commits, the changes should outlives the outage".
                 durability is one of the most important feature of almost all the database. becoz the databases (specially RDBMS) are gives us the gurantee that when we commit a transaction, that commit will actually being written on the disk. no matter what happens. data would still be there.
                 
  -     Isolation:-
                In transaction we can specify multiple statements.
                when the RDMS running, it is not like that one time only one transaction is happening. at one time multiple transaction can run. 
                "when multiple transaciton are executing at the same time, how much of the tranparency of one transaction should be visible to other" is defined by isolation.  
                    means isolation implies that how much of details of one transaction is available to other transaction.
                    assume In the worst case we can say like 'when one transaction is running then we should not be running any other transaction' this is know as Serializable isolation level.    there are three other isolation levels (We can go throught it from google).   
                    
                    eg:-    in the transaction1 5 statements has running, in the transaction2  3 statements has running. 
                                assume these two statements (white circle) are happens at exact same time. should the changes happening in these statements (img4) be visible in this transaction (img5) or not. this is all about Isolation.  
                
                eg:- 
                        let say in Txn1, the statement3 (img6) will update the value (assume there is a table, there is one col whoes values are updated to 10), but the transaction has not commited yet (img7). the value was updated  by statement3.  now when the txn2 ran & it read the value. 
                            ques:- should txn2 read the updated value, but the transaction1 has not commiteed yet.
                            ans  :- if the txn2 read the updated value then txn2 become inconsistent. we can configure this. becoz of these isolation levels.
                                        like how much of isolation we want transaction in txn1 to be from txn2.
                                        like should we allow the txn2 to read the updated value or not. this thing we can configure it by using isolation levels.
                                        this is how the db arranges the transactions such that it maintain the isolation b/w them.  
```
### we should pick relational databases when we want the ACID property  & relations in our database.