# Lambda
Lambda's are serverless functions. You upload your code and it runs without you managing or provisioning any servers.   
- Lambda is serverless. You don't need to worry about underlying architecture   
- Lambda is a good fit for short running tasks where you don't need to customize the os environment. If you need long running tasks (> 15mins) and a custom OS environment than consider using Fargate   
- There are 7 runtime language environments officially supported by Lambda: Ruby, Python, Java, NodeJs, C#, Powershell and Go   
- You pay per invocation (The duration and the amount of memory used) rounded up to the nearest 100  milliseconds and you based on amount of requests. First IM requests per month are free   
- You can adjust the duration timeout for up to 15 mins and memory up to 3008 MB   
- You can trigger Lambdas from the SDK or multiple AWS services eg. S3, API Gateway, DynamoDB
- Lambdas by default run in No VPC. To interact With some services you need to have your Lambda in  the same VPC eg. RDS   
- Lambda can scale to 1000 of concurrent functions in seconds. (1000 is the default, you can increase With AWS Service Limit Increase)   
- Lambdas have Cold Starts. If a function has not been recently been execute there will be a delay