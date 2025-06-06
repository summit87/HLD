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






