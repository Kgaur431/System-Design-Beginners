``` 
        it is very similar to Message Streams & Message Brokers. 
        In case of both they pull the message out from the queue || streams. this is how 'they can process the message at their own pace' while the messages are being buffered. 
        the realtime pubsub is like, 'why should we pull if we can push'. 
            let say we have a realtime pubshub setup. if someone send an event to the pubsub then the even would be immidetaly broadcast to the everyone who has subscribe to it.
                 subscribers are not pulling from it instead pubsub are pushed the event to all of them who has subscribe to the pubsub.
            eg:-    Redis pubsub.
                        Redis is an db, typically known as for its caching capability but it also has pubsub. what it does?
                            on the redis we can configure 'like user can subscribe to a channel & whoever has subscribe to that channel, whenever a publisher publishes on that channel subscriber would get it immediately. 
                            so the subscriber are not pulling from the channel rather redis pushing the data to corresponding subscribers'.  
                            where it helps ? 
                                it helps in getting delivery faster to the subscribers .
                                like as soon as redis gets the message pubsub pushes that message to every subscribers.
                                becoz of that we can build very low latency applications.
        messaging apps uses the realtime pubsub very well.
            real world usecase:-
                  -     when we do Message Broadcast, lots of servers are connected to the same broker (redis pubsub). if api server pushes the message into the pubsub, redis pusub responsibility to sends the message to the every subsricbers who have subscribe to this channel (img 203).
                  -     let say in api server has change some configuration & now we want to deliver this changes to every api server. how we can do ?
                            all the api servers are connected to the redis pubsub, when one of the server changes the configuration then it pushes an event into redis pubsub which is sends to all the servers.   
        
        realtime pubsub do not buffer any message on it. as soon as api server push the event into the pubsub, it will broadcast that event immidiately to the subscribers.
        so the limitation of pubsub is that it would not buffer anything. 
                            
        exercise:-  watch video 16,  from 03:17 to end. 
``` 





