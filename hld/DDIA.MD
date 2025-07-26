## Topic to cover in DDIA

#### Chapter 5 : Replication

* #### Leader based replication
        
    * Leader based replication have master - slave or leaser follower based architecture

        * All the write will go to master / leader and with some replication strategy data will copy to 
            to follower or slave

        * #### <b >Question</b> :  What hapen if followe crashes or down due to some reason (Power outage/ disk crashes etc...)
        
        * Replication strategy 

            * Synchronus : Client will get response only when leader will get acknowlade from all the follower 

                Pros : data will be consistence
                
                Cons : Latency will be sacrifice
            
            * Asynchronus : Leader will accept the write request from client and retun reponse post transaction commited in leader , then follower will request from leader for write request

                Pros : Latency will be more

                Cons: Data may be in inconsistence state b/w leader and follower

            * There are different way to handle data replication

                * Statement based replication
                * WAL  based replication
                * logical log based replication

