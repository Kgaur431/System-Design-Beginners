``` 
    we know that RDBMS provides the ACID gurantees.
    what do we mean, when we say 'database gives us the isolation' ?
        when database provides isolation, it means it is talking about the isolation b/w the transactions. 
        like when there are two transaction running parallely on my database, what kind of transperancy one has over other. this is know as isolation.
        
        the way database gives you tools to tuned that, it is called isolation level.       means the way which db provides to tunned the transperancy b/w the trnasactions that is done through isolation levels.   
```
``` 
    -   Isolation level dictate how much one transaction knows about the other, both of the txn's are executed parallel way.
    -   there are 4 standard isolation level possible.
    
                about the db:-          we are using mysql db.
                        there has one sql db, 
                        where we have create the users table(img 9). there are two fields (id, name).  
                            we have inserted the one row (img 10). 
                            we can see the user details (img11).
                        we have started the two sessions in the terminal(img 12), we can consider the left terminal as txn1 & right terminal as txn2   
               now we try to understand all of the isolation level with the help of the db.  
                       
        1.  Repetable Reads
                Ques:- what it does ?
                                it talks about " it giving us the consistent reads within the same txn".
                                
                    understand with the eg:-
                    =   In both of the txn's, we have the current isolation level (img 13).        ==>     REPEATABLE-READ.
                    =   In both of the txn's, we have set the autocommit as 0, so that there is no commit happen after each transcation. if we manually do the commit then only txn's get commited. if we not set to 0 then after executing the each statement mysql will automatically commit the changes (img14).
                    =   Now I have initiated both the transactions (img 15).    
                    =   Now we read the row(id=1) in txn1.  after that we have read same row(id=1) in txn2 (img 16). both of the txn's has get the result as 'a'. 
                    =   Now In the txn1 we have updated the value of row(id=1) (img 17).  but it has not commited yet. after that In txn1 we have read the same row(id=1) & we get the update the value (img 18).  
                         Ques:- what happens if we read the same row(id=1) in txn2 ?  remember the txn1 has neither commited nor aborted. it still running.
                         Ans  :-    we get old data (img 19). 
                    
                    conclusion:- "It does not matter that, if txn1 has made any changes unless the changes are actually get committed the txn2 will not able to read".   
                        
                    =   Now we have do the commit in txn1 (img 20). after that if we read the row(id=1) in txn1 then we get the updated the value (img 21).         
                    
                    now the things go interesting.
                        we have commited the txn1. so the actual value of  row(id=1) has been updated.
                   
                   =    In txn2 previously we read the value (line 32) then we got the old value. now the txn1 has committed. so if we again read the value of same row(id=1) in txn2 then we again got the old value (img 22).      (txn1 has committed but still we are getting the old value). 
                   this is the beauty of repeatable reads that "we can repeat our read within the txn & it would gives us the consisted view of it" (img 23).  
                        assume if we get the updated value then what happen ?
                            then it seems inconsistent. becoz "repeatable reads gives the gurantee that the first read that we do (line 32), whatever value we got. we will continue to see the same value  if we read agian (line 41) within the txn" no matter how many other txns make any changes to that value. 
                            like if we read the value of row(id=1) multiple times in the txn2 , we would always see the same value. means the data is consistent   (no matter if the data has updated). 
                   
                   =    Now In the txn2 we have do the commit (img 24). {In txn2 we not perform any update operation in row(id=1), we have just read the value, now we have committed}.
                   =    Now we have start the new txn & we have read the row(id=1),  we get the updated value (img 25).
                   
            Conclusion:- we can see that "how the reads are repeatable & consistent within the txn".
                                 
        2.  Read Committed
                    img 26.
                    In read commtted, "when a transaction is reading a value, it could always read the latest committed value". 
                        means when a transaciton is running, it is possible for that transaction, it could read two diff values. eg when the txn1 was running, it read the particular value. while the txn1 read the same value again, b/w this txn, there might be some other txn committed with the updated value. 
                        then the value read by txn1 would be updated value.
                    
                    understand with the eg:-
                    =   First we set the isolation level (img 27) in both the txns. & we confirmed that the correct isolation level has to be set (img 28).
                    =   Now we start both the tnx's (img 29). 
                    =   Now In the txn1 we read the value of row(id=1), we got value as 'a' (img 30).
                    =   Now In the txn2, we have update the value of row(id=1) (img 31). after that In txn1, we have read the value of row(id=1) & we got the updated value (img 32). 
                    =   Now In the txn2, we have do the commit. (img 33).    {remember that txn1 is still running means we have not commit the txn1 yet}     
                         we have not do the commit in txn1 yet. means the txn1 is still running. we have not the terminated the txns1 .
                    =   Now In the txn1 we have read the value of row(id=1) & we got the updated value (img 34).         { still we have not commited the txns, & we get the updated value} 
                            means In the first read operation we get the old value & In the second read operation we get the updated value within the same txns, it means In read committed we get the inconsistent view if we execute the multiple read operations within the same txn.   (img 36).
                            
              Conclusion:-   In Read Committed, whenever any txns read the value then it always read the committed value. means assume txn1 read the row(id=1) value & txn2 update the value of row(id=1) then commit it. & now the txn1 again read the row(id=1) value then it will get the updated value 
                                        within the same txn.
                            
                       
        3.  Read Uncommitted
                    img 37.
                    "In the txn2 we have updated the value of row(id=1) & we have not do the commit yet. In the txn1 we read the value of row(id=1) & we got the uncommited value of txn2".  this is called as Dirty read.
                    
                    understand with the eg:-
                    =   First we set the isolation level (img 38) in both the txns. & we confirmed that the correct isolation level has to be set (img 39).
                    =   Now we start both the tnx's (img 40). 
                    =   Now In the txn1 we read the value of row(id=1), we got value as 'a' (img 41).
                    =   Now In the txn2, we have update the value of row(id=1) (img 42).   {remember both the txns are running, we have not commited any of the txn yet}. 
                    =   Now In the txn1 we have read the value of row(id=1) & we got the updated value (img 43).        {here we are reading the uncommited value (img 44)}.
                            assume In txn1, instead of reading the value, we are doing some other processing on the updated value. let say In txn2 we updated the value with wrong input , so In txn2 we might do the ROLLBACK.  
                                &  In txn1 we have done the processing on the data which is rolledback by the txn2 (img 45).
                    =   At the end, we do commit in the txn2. 
                                
                                it will give a higher through put  so In some cases if we want the higher throughput & we are agree to ignore the correctness of the data then we can use  Read Uncommitted. 
                                
              Conclusion:-  In txn1 we are reading the uncommited data of other txn like txn2.
              
        4.  Serializable 
                (img 46)    
                Ques:-  What it does ?
                Ans  :-   it is an strict form of isolation. typically the slowest one as well. 
                              it enforces the serializability means "every read operation we do is a locking read operation. 
                              means unless one transaction has done their processing till the it's blocking all the other transaction.
                
                understand with the eg:-
                =   First we set the isolation level (img 47) in both the txns. & we confirmed that the correct isolation level has to be set (img 48).
                =   Now we start both the tnx's (img 49).
                        initally the id=1 has a  value as 'a'. 
                =   Now In the txn1 we update the value of row(id=1) (img50) & we have not do the commit in txn1, it still running. 
                =   Now In the txn2 we are trying to read the value of row(id=1), then it waits for txn1 to commit || rollback its changes(img 51).
                =   as soon as we do commit || rollback in the txn1, then In txn2 the read operation will perform (img 52).
                        means if we see the images properly, as per img 51 we have do the update operation in txn1 & then  we do the read operation in txn2 but txn2 are waiting for the completion of txn1. so we not get the o/p instead the cursor was blinking in the next line. 
                        but when we do commit in txn1 then after we get the o/p at the same line where the cursor was blinking.  

          Conclusion:-  here we can see the very strict serilization in place. when we are updating / reading etc. something then everyone else can not allow to process or move forward along it.  
          
    Warnings:-    it depends on the storage engine of the database we are using some minor behaviour we can oberseve but these are big picture on how the 4  isolation level determines how read & writes are happening in the system.

```