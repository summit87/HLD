### HLD_1.txt : Notofication system design : 
---
#####  Design a notification system, any service can use this notification system to target the user </br> 1. Service can sent 10M push notification ,5M SMS notification and 2M email notification on daily </br> 2. Notification Service should also handle notification in batches </br> 3. Assume Service which uses notification , can sent 10K notification in batches
---

* Backof the envelope calculation

    * calculating the size of the user_profile table 

        ##### user profile table
        ---

        column name   |data type
        ------------- | -------------
        user_id       | varchar(10) 
        email_id      | varchar(100) 
        first_name    | varchar(10) 
        last_name     | varchar(10) 
        mobile        | varchar(10) 
        is_active     | tinyint 
        create_ts     | datetime 
        opt_for_push  | tinyint
        opt_for_email_notification | tinyint
        opt_for_sms_notification | tinyint
        

        ##### device_info table
        ---

        column name   |data type
        ------------- | -------------
        user_id       | varchar(10)
        device_id | varchar(10)
        last_login | datetime 


         ##### push_notification
        ---

        column name   |data type
        ------------- | -------------
        device_id     | varchar(10) 
        text          | varchar(40) 
        sent_time     | datetime 
        media         | varchar(50) 


        ##### sms_notification
        ---

        column name   |data type
        ------------- | -------------
        mobile        | varchar(10) 
        text          | varchar(40) 
        sent_time     | datetime 
        media         | varchar(50)


        ##### email_notification
        ---

        column name   |data type
        ------------- | -------------
        emai_id       | varchar(100) 
        text          | varchar(100) 
        sent_time     | datetime 
        media         | varchar(50)






    * Assuming each user have 1 device , currently notification system have <b>50M</b> user  only  

      * user table

        * Assuming  record will take 1KB of data , 50M*1KB ~- 50GB
      * similary for device table and ither table we can consider 0.25KB of data i.e 25GB of data 

      * Sice we know every day we are sending the 10 milion push notification, 5 miniloin SMS and 2 milion email notofication

         * push_notification : 

            * TPS (Transaction per second) : 10M daily => 10M/ (24*60*60) => 1200 TPS 

            * Storage : 
              total size of message is : 500 byte => so 10M*500 => 5GB data every day

              for 5 year's => 5*365 ~=2TB*5 ~= 10TB of storage for push nottification

          * Similary we cancalculate for email and SMS notification

      * Assume in peeak , which last for 10 minute and we need to send 10M/minute push notification <b>Then how we calculate partition , required worker node and network bandwidth</b>

        * total messge in 10 minute : 10*10M => 100M message </br> per sec messge => 10M/(60) ~= <b>167k message per sec</b></br> 
        
        * Assuming one partition handle 50k/sec , then 167k/50k ~= <b>4 partition </b>
        * Assuming one worker node are processing 1000/sec , then total worker node required is ~167
        * Calculate network bandwidth

          * for email notification<br>

             * Assume each mail is 10KB of size , and we are sending 2M message every day (from problem statement given above) , <b>~=24 message/sec</b>

               * Total message size per sec = <b>24*10 => 240KB message/sec</b></br>
             
             * Assume in peek time we sent 10M/sec , then how we will calculate network bandwidth









        
           




  
