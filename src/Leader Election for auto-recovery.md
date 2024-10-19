```` 
    Re-watch the entire video20.    
    must watch Bully algo or any leader election algo. 
    
    Summary as per understanding:-  
            -   when the api server go down, there are worker-orchestrator's who will spin up new  server & replace it so that the work is going on. 
            -   what if the worker-orchestrator goes down then who will spin up new  server ?
                    so there will be a Leader-orchestrator who will spin up the new worker-orchestrator when any worker-orchestrator goes down. 
            -   what if the Leader-orchestrator goes down then who will spin up worker-orchestrator ?
                    In that case all of the worker-orchestrator's are doing a election and choose any one of the worker-orchestrator as Leader & then the work is continue. 
                    like:-  In the company there is an manager who has responsibible for organise the Standup call, what if the manager is on leave, that does not mean that standup will not happen. 
                                    In this case One of the team member will act as a manager & take the responsibility of the standup that's how the standup call happen, means work does not stop. 
            -   electing a Leader-orchestrator is depending on the algo which we are using it. 
                    like Bully algo, is the famous & easiest algo to all the worker-orchestrator's choose one of them as a Leader-orchestrator.
            -   this is how "the system get auto-recover if any api-server, worker-orchestrator's or Leader-orchestrator goes down" without any human intervention.
```` 