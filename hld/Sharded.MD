## Sharded counter or Distributed counter

Below mentioned point is the use case of sharded counter <br>
    

 * Get total number of count distributed accross different shard's
 * Number of likes
 * number of re-tweet
 * number of re-post ,
 * .....


 There are different design approach for implementing the sharded counter / distributed counter


 * Single DB approach (RDBMS)
    
    * ##### <b>PROS : </b> 
    
        * We can achive the consistency.
    * ##### <b>CONS : </b>

        * Latency will be highe
        * Single point of failure
        * If request get bombarded then , we need to put extra effort to handle this situation ,</br> e.g handling backpressure , we need put circut breaker and then we may be dropping very crucial event which can affect customer / user .

* 2-Phase commit 

    * The pros and cons will be same , its just that we are removing single point of failure


* Kafka + DB

    * #### <b>PROS : </b>
    
        * We can handle sudden burst in the traffic
        * In case of failure we can do retry of the failed message without loosing the message
    
    * ##### <b>CONS : </b>

        * Need to aggregate the count from all the DB due to this delay, we may be giving in-correct count to user </br> event if we putting cache to store aggregated count , then we need to handle cache invlidation
        * there is extra effort to manage this kafka cluster , redis cluster etc..

* Sharding approach

    * #### <b>In this approach , we distribute the counter count among the shard's . </b>

    * PROS:  
    
      * That will improve the latency in writing 
      * For reading , we will read all the counts from shard's and merge the count and update the cache
      * This approach will give eventual consistency b/w DB and cache




## Master -  Master replication in distributed system
#### Why we need this
---
    
*   For highe availbility of application

* When one of the region is down then in case of master to master replication <br> we can redirect user to availble region

* We can achive the data durability

* Every user can connect with there own master region, in that case we are <br> also reducting the latency of the system
---
#### Biggest challenge with Master to master replication is handling the wright conflicts. <br> There may be certain question as below

* <b>How to avoid the write conflicts 
* How to fix this </b>
---

* >How to avoid the write conflicts 

    * Remove the master-master replication stratgey 
    * Let's keep master-slave architecture and let master handle all the write and slave will handle all the read.<br> In case of master fail, then one of the slave becomes master and serve the master responsibility.
    * To achive the highe availbility of the application , there should DR region of the primary region

* > How to fix (If we are going with master to master architecture)

    * We can add the timestamp in the transaction , when any write happening in already written data for the same key, then we will check which timestamp is greater. <br>
    The timestamp which have greater value, will owner and update the 
    transaction. <br>
    
         * <b>Couple of point on timestamp : </b> Timestamp should always come from source of origin of transaction. Timestamp should be independent of network drift.<br> If we will considering the timestamp which depends on the network, then in that case we may lossing the correct data.

    * While writing , either DB should have write conflicts resolution strategy or we can implement in application (Not recomended)

    * While reading , when application see conflict in the data , application should resolve and update the db and return to caller .
    This strategy we call read repair
