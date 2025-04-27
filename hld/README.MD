### HLD_1.txt : Notofication system design
* Backof the envelope calculation

    * calculating the size of the user_profile table 

        ##### user profile table
        ---

        column name   |data type
        ------------- | -------------
        user_id       | varchar(10) (10 byte)
        email_id      | varchar(100) (100 byte)
        first_name    | varchar(10) (10 byte)
        last_name     | varchar(10) (10 byte)
        mobile        | varchar(10) (10 byte)
        is_active     | tinyint (1 byte)
        create_ts     | datetime (5 byte)
        opt_for_push  | tinyint
        opt_for_email_notification | tinyint
        opt_for_sms_notification | tinyint
        

        ##### device_info table
        ---

        column name   |data type
        ------------- | -------------
        user_id       | varchar(10) (10 byte)
        device_id | varchar(10) (10 byte)
        last_login | datetime (5 byte)


         ##### push_notification
        ---

        column name   |data type
        ------------- | -------------
        device_id     | varchar(10) (10 byte)
        text          | varchar(40) (10 byte)
        sent_time     | datetime (5 byte)
        media         | varchar(50) (50 byte, considering we will store only tiny url) 


        ##### sms_notification
        ---

        column name   |data type
        ------------- | -------------
        mobile        | varchar(10) (10 byte)
        text          | varchar(40) (10 byte)
        sent_time     | datetime (5 byte)
        media         | varchar(50) (50 byte, considering we will store only tiny url) 


        ##### email_notification
        ---

        column name   |data type
        ------------- | -------------
        emai_id       | varchar(100) (10 byte)
        text          | varchar(100) (10 byte)
        sent_time     | datetime (5 byte)
        media         | varchar(50) (50 byte, considering we will store only tiny url) 






    * Assuming each user have 1 device , currently notification system have <b>50M</b> user  only  

      * user table
        emailid :  100 byte *50M = 50*10^9 => 5 * 10^10 => 50GB
        total size of the table = sizeof(emailid)+sizeof(user_id)+sizeof(firstname)+sizeof(lastname) => 50+10+10+10 ~=100GB
      * device table : 10GB+10GB+5GB =25GB

      * Sice we know every day we are sending the 10 milion push notification, 5 miniloin SMS and 2 milion email notofication

         * push_notification : 

            * TPS (Transaction per second) : 10M daily => 10M/ (24*60*60) => 1200 TPS 

            * Storage : 
              total byte : 75 byte => for calculation we can assume total byte is 100 byte </br>, so 10M*100 => 10^10 => 10GB data every day

              for 5 year's => 10*365 ~=4TB*5 ~= 20TB of storage

          * Similary we cancalculate for email and SMS notification


        
           




  
