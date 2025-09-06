### Session Timeout
---
Till this time group coordinator will wait for consumer for heartbeat
___

#### Heartbeat timeout (Let's say X)
---
Every x ms broker will send heartbeat signal to group coordinator service about health

#### max poll interval (Le's say Y)
---
B/w Every batch consumer will wait for next batch



Let say session time out is 3000 and hearbeat is 100 so total retry count is 3 (3000/100) , i.e <br>
so if group coordinator service will not listen the heartbeat within defined session time , then group coordinator service will think consumer is dead and rebalancing will trigger.


Heartbeat timeout should always less than the session timeout

max poll interval :- we shuld always configure the right value b/w two poll , if one batch is taking more time to process than defined max poll interval , group coordinator service will trigger the reblancing