``` 
    In Most cases, api servers are "stateless" which means that let say lots of api server are put behind the load balancers, request can go to any one of the api server. if any one of the api server go down then other api server can handle the req only the reqest that are in transit get affected. 
    this is very simple when it comes to stateless api server. 
    what about db's, db's are statefull becoz they hold the data. so if the db goes down, in most of the cases it is a complete outage.
        worst thing that would happened is the db crashes such that the disk crashed & we loss the entire data (img 225). 
         
    -   A good system always takes care of this above sitution(img 226).
    -   whenever we want to make our data side of things resilient like db, first thing that come to our mind is 'can I make our data redudant means creating multiple copies of the data'.
            so if we created the multiple copies of the data then it can help us to recovery the data (img 227). 
    -   Data Redundancy can be implemented at diff level. 
            Row level. Document level.
                like we can dump the db tables & create another table || dump the db & create another db.
   -    Backup & Restore:-          img 228.
            =  it is an simplest way to prepare yourself for an outage.    
                1.  Daily backup:-
                        we can do daily incremental backup of the data. 
                        let say we can do a complete backup at once & whatever changes have been made in one day, that we have to dump it. & repeat this process.       this is the way of taking daily backup of the data.
                2.  Weekly complete backup:-
                        let say we fixed day in a week & we take complete backup of the db. 
                3.  Storing one copy across region:-
                        we store one copy of the db across the region for disaster recovery.
                
                these all of them are kind of one time backup.
   -    Continuous Redundancy:-         img 229.
            =   here we just don't do the complete backup || daily backup of the db.
            =   here we are "actively maintaining the two copies of data".  How we can do it?
                    the way to do it by 'setup replica of our db'.
                    eg:- let say we have a master node and we have a replica node.  assume the master node goes down (disk crashed), In that case we have replica there. 
                                like we can spin up the data from the replica to the Master node and start serving the data from master.
            =    we have to "ensure that our data is continuously getting duplicate in another db". 
                    How does the data goes to the replica ?
                        way1:-  either we can do asynchronously copied (img 231). 
                        way2:-  or all the writes requests go to both of these db (img 230). 
                    "replica is an stand by machine means replica may not be serving to the any user, all it is doing is 'it is just one stand alone copy of the data which is continuously sync with the master'.
   
  Exercise:-  video19, from 07:00 to end.          
```