	 
     RDS Instance in AWS:----
	 
	Create the RDS instance in aws console as follows -
    MySQL, 8.0.LATEST, Free Tier, DBInstance=flipbasket, credentials=root/password
    Instance size=T2u, No Storage Autoscaling, Connectivity-DefVPC,
    Additional conn-VPC SG- create new "rds-sg"
    No backups, no monitoring, no minor version upgrade
    Create the DB
    Once DB is ready modify the rds-sg cidr=0.0.0.0/0



	LAMBDA in Eclipse

1.  Create this project in Eclipse using the plugin and selecting "New AWS Lambda Java Project"
	and in the event dropdown select "Custom"

	configure these keys from aws in eclipse
	*aws_access_key_id
	*aws_secret_access_key
  
2.  Layers can be setup using the instructions here
     https://docs.aws.amazon.com/lambda/latest/dg/configuration-layers.html
     See the appbase directory for the structure and contents
     
     Use appbase-java.zip to be used as sql connector layer with lambda function in aws console
 
3.   AWSLambdaVPCAccessExecutionRole policy must be attached to the lambda-multirole to access
  resources that are attached to the VPC such as an RDS instance
     https://docs.aws.amazon.com/lambda/latest/dg/configuration-vpc.html
  
4.   Memory should be 512MB and timeout should be 30s
  
5.   At the time of creating the function select the default VPC and all the subnets and 	
  select the default security group. Multiple ENI will be created (see EC2 management console),
  so do not hit the lambda immediately. Wait for about 2 minutes.
  
6.   Setup the following JSON as the test case
    {"table": "employees","order_by": "birth_date"}
  
 7.  Once the instance is created, connect to it from an EC2 instance after installing the MySQL client
  and create the schema using employees.sql
  
 8.   Once it all starts to work see the number of connections in RDS console as one in monitoring tab.
		Do not make any more invocations; wait for about 25mins and see the connections drop to 0
  
9.   Note-> Upload the function and then go to the console->"Export function"->"Download AWS SAM File"!
        Now use this SAM to run and test locally! You do not have to author the SAM from scratch.
        Just ensure that the test cases are updated otherwise the default test case will fail.
        Do not upload with the SAM file, otherwise the lambda may not fire and error out by saying
        that LambdaFunctionHandler is missing!
	
10. 	private static final String URL = System.getenv("CONNECT_IP_DNS");
	private static final String DB = System.getenv("DB_NAME");
	private static final String UID = System.getenv("USER_ID");
	private static final String PWD = System.getenv("PWD");
	
	Enter these values in environmen  variables sestion in lambda function in aws console
 
