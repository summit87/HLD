### Design Distributed Task scheduler
---

* #### Why we need destributed task scheduler
```html
There are multiple use case when we need distributed task scheduler as mentioned below
    1. In socials network , For friends suggestion
    2. OTT platform will use this job to generate the transalation 
    3. OTT platform can use this job to generate different different
       HD's Viedo ,  2k,4k,8k,etc..
    4. We can also provide as Sceduler as a service to customer to do 
       Generate tumbnail , format file, extract text from image , etc..
```

* #### Clarification question
```text
    1. How many user can use this service (assume : 300 milion user)
    2. What should be DAU (assume 10 % of total user , i.e 30 milion)
    3. How many times/request each user can use this service 
       (For assumption , we can take 4 times or 4 request/day by each user)
    4. How many time user can see the job status , assume 1 read 
```

* #### Requirement
```
    1. User can submit task
    2. User can delete the task
    3. User can cancel the task
    4. User can view the progeress of the job
    5. User can give option , when job should get executed
    6. Job should support premium vs non premimum customer
```


* #### Backof the envelop calculation
```
Assuming 24*60*6 ~= 10^5
Since we have 30M DAU , and each user can use this systme every day 4 times,
Request per day : 30*4 => 120M => 120*10^6 => 1200 Req/sec

Assuming over all user job request size  100KB
Daily we are getting 120M req => 120*100*10^6 => 12*10^9 => 12GB daya every day
for one year 12*400 => 4800GB => ~= 5TB
for 5 year ~= 5*5 => 25TB 

Sice we have read:write ratio is 1:3 , we can use RDBMS

```

* #### API contract

```HTTP
POST /submitJob
{
    jobType,customerType,nextExecutionTime,numberOfRetry,retryInterval
}

GET / getJobStatus/{jobId}

DELETE /deleteJob/{jobId}

GET /getJobByStatus /{jobStatus}

GET /jobsHistory /{userId}

```

* #### DB Schema

```
user_job_details (t1)
---
1. User_id
2. Job_id
3. retry_count
4. retry_interval
5. created_at
6. retryble_flag
7. year
8. month
9. date
10. user_type

cluster index : job_id
non-cluster-index : user_id
partition_key (year,month,date) , this will help us to go specific partition and search data

job_details (t2)
1. job_id
2. next_execution_time
3. job_status
4. retry_count
5. last_updated_ts

cluster_index job_status
non_cluster_index last_updated_ts

```

* #### DB query plan
```SQL format

select t2.job_id,t1.user_type from t2 left join t1 on t1.job_id = t2.job_id 
and t1.retry_falg=true where t2.retry_count < t1.retry_count and DIFF(next_execution_time-now()) < t1.retry_interval  and t2.job_status != COMPLETED

```

* #### Building block

    * Load balancer
    * Rate limitter
    * Database
    * Messaging system
    * UI Dashboard




