
## EC2 Pricing
https://plazagonzalo.medium.com/ec2-exam-questions-aws-solutions-architect-8c0f0e643038

- **Solution: 4.** A cluster placement group provides low latency and high throughput for instances deployed in a single AZ. Load Balancer Placement group doesn’t exist.
- **Solution: 3.** A spread placement group is a group of instances placed on distinct underlying hardware, reducing the risk of simultaneous failures.
### On-dmenad
By default, by the hour or by the minute, no up-front payment, no long-term commitment. Suitable for *short-term*, *spikey* or *unpredictable*

### Reserved Instances
Best long-term savings with commitment. Best for *steady-state*, *predictable usage* or require *reserved capacity*. Reduced pricing is based on **Term . Class Offering . Payment Option**

**Terms**: 1 Year or 3 Year contract

**Payment Options**: All Upfront, Partial Upfront, No Upfront

**Standard** Up to **75%** reduced pricing. Cannot change attributes

**Convertible** Up to **54%** reduced pricing. Alows you to change attributes if greater or equal value

**Scheduled** You reserve instances for specific time periods eg. once a week for a few hours. Savings vary

**RIs** can shared between multiple accounts within an org

**Unused RIs** can be sold in the **Reserved Instance Marketplace**

### Spot
Provide a discount of **90%** compared to On-Demand pricing

If **you** terminate an instance you will stil be charged for any hour that it run ???

### Dedicated Host
Physical Isolation, dedicated hardware, most expensive

Entreprises and Large Organizations may have security concerns or obligations

## AMI
Amazon Machine Images provides the information required to launch an instance which holds:
	- Template for the root volume for the intance (EBS instance or Instance Store Snapshot) eg. OS, applications ...
	- Launch permissions that controls which AWS can use the AMI to launch instnaces
	- Block device mapping that specifies the volumes to attach to the instance when it's launched.
- Region specific, we have to copy an AMI to other regions

## Auto scaling groups
- An ASG is a collection of EC2 instances grouped for scaling management
- Scaling Out is when you add servers. Scaling In when you remove servers. Scaling Up is when you inscrease the size of instance eg. updating the launch configuration with larger size
- Size of ASG is based on **Min**, **Max** and **Desired** capacity
- **Target Scaling Policy** scales based on when a target value for a metric is breached eg. Average CPU utilization exceed 75%
- **Simple Scaling** triggers scaling when an alarm is breached
- **Scaling Policy with Steps** is the new version of Simple Scaling and allows you to create steps based on intervals of alarm values
- Health checks can be run againts either an ELB or the EC2 instances
- ASG uses a Launch Configuration which holds the AMI, InstanceType, Role ...
- Launch configuration cannot be edited and must be cloned or a new one created. Auto scaling settings should be manually updated when new launch configurations are created

## ELB
Distributes incoming application traffic across multiple targets, such as Amazon EC2 instances, containers, IP addresses and Lambda functions

Load Balancers can be physical hardware or virtual software, there are three types:
	- ALB (Application, HTTP/HTTPS): Web Apps
	- NLB (Network, TCP/UDP): high network thoughtput eg. Video games
	- CLB (Classic, Legacy): not recommended
- ALB has Listeners, Rules and Target Groups to route traffic
- NLB uses Listenres and target groups, there are no rules
- CLB uses listeners and EC2 instances are directrly registered as targets to CLB

- An ELB must have at least **two** Availability Zones and cannot go cross-region, we must create one per region

- **Exam question:** Classic Load balancer respond with 504 error if the application is not responding
- Use `X-Forwared-For` to get original IP of incoming traffic
- We can attach Web Application Firewall (WAF) to ALB but not NLB or CLB
- We can attach Amazon Certification Manager SSL to any of the ELB for SSL termination
- ALB has advanced Request Routing rules where you can route based on subdomain header, path and other HTTP(S) information
- Sticky Sessions can be enabled for CLB or ALB and sessions are remembered via Cookies


## ECE Follow along
- In order to be able to use Session Manager to login to an EC2 instance, its IAM role should have `AmazonEC2RoleforSSM` policy or better the recommended one `AmazonSSMManagedInstanceCore`

```
# Get the user-data, the script that was used after luanching the instace
curl http://169.254.169.254/latest/user-data

# Get the instance meta-data like ami-id, security groups ...
curl http://169.254.169.254/latest/meta-data/ami-id
```

- Encrypt a running EBS
	- Create a snapshot, check the "Encryption" checkbox and select a "Master key"
	- Create an Image (AMI) from the EBS Snapshot
- Launch Configuration vs Launch Template
- Launch Configuration:
	- AMI
	- Machine Type
	- Volumes
	- IAM profile
	- Security group
	- User data
	- Meta data
- Auto Scaling Group
	- VPC
	- Subnets
- Auto scaling group ensures there is enough number of EC2 machines (in the same AZ ?) but If the Whole AZ goes out, the Auto Scaling group won't help, we need to create high availability using  load balancers so we can run instances in more that one AZ
- I couldn't change the security group inbound rule for HTTP to just accept "Source" from another security but I was able to create another security group with that same rule