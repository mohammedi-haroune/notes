# CloudFormation
- When being asked to automate the provisioning of resources think CloudFormation   
- When Infrastructure as Code (laC) is mentioned think CloudFormation   
- CloudFormation can be written in either JSON or YAML   
- When CloudFormation encounters an error it will rollback with  ROLLBACK_IN_PROGRESS
- CloudFormation templates larger than 51,200 bytes (0.05 MB) are too large to upload directly, and must be imported into CloudFormation via an S3 bucket.   
- NestedStacks helps you break up your CloudFormation template into smaller reusable templates that can be composed into larger templates 
- At least one resource under resources: must be defined for a CloudFormation template to be valid
- Format:
	- MetaData extra information about your template   
	- Description a description of what the template is suppose to do   
	- Parameters is how you get user inputs into templates   
	- Transforms Applies marcos (like applying a mod which change the anatomy to be custom)   
	- Outputs are values you can use to import into other stacks   
	- Mappings maps keys to values, just like a lookup table   
	- Resources defines the resources you want to provision, at least one resource is required 
	- Conditions are whether resources are created or properties are assigned
	
# CloudWatch
CloudWatch is a collection of monitoring services: Dashboards, Events, Alarms, Logs and Metrics   

- CloudWatch Logs: log data from AWS services. eg. CPU Utilization   
- CloudWatch Metrics: Represents a time-ordered set of data points, A variable to monitor eg. CPU Utilization over time   
- CloudWatch Events: trigger an event based on a condition eg. ever hour take snapshot of server   
- CloudWatch Alarms: triggers notifications based on metrics when a defined threshold is breached   
- CloudWatch Dashboards: create visualizations based on metrics
- EC2 monitors at 5 min intervals and at Detailed Monitoring 1 minute intervals
- Most other service monitor at 1 minute intervals, With intervals of 1 , 3 , 5 minutes.   
- Logs must belong to a Log Group   
- CloudWatch Agent needs to be installed on EC2 host to track Memory Usage and Disk Size 
- You can can stream custom log files eg. production.log   
- Custom Metrics allow you to track High Resolution Metrics a sub minute intervals all the way down to 1 second.

# CloudLogs
 
CloudTrail logs calls between AWS services   
- governance, compliance, operational auditing, and risk auditing are keywords relating to CloudTrail   
- When you need to know Who to blame think Cloud Trail   
- CloudTrail by default logs event data for the past 90s days via Event History   
- To track beyond 90 days you need to create Trail   
- To ensure logs have not been tampered With you need to turn on Log File Validation option
- CloudTrail logs can be encrypted using KMS (Key Management Service)   
- CloudTrail can be set to log across all AWS accounts in an Organization and all regions in an account.   
- CloudTrail logs can be streamed to CloudWatch logs   
- Trails are outputted to an S3 bucket that you specify   
- CloudTrail logs two kinds of events: Management Events and Data Events   
- Management events log management operations eg. AttachRolePolicy   
- Data Events log data operations for resources (S3, Lambda) eg. GetObject, DeleteObject, and PutObject   
- Data Events are disabled by default when creating a Trail.   
- Trail logs in S3 can be analyzed using Athena