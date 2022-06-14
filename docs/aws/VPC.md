## Questions
**Q:** What is Network Address Translation, it's used both in NAT gateways for private insntaces and it's one of the features of Internet Gateways for public instances ?
**A:**

**Q:** Does star configuration: 1 Central - 4 other VPCs a must ? or just an example ?
**A:** 

**Q:** We have to create an entry in the route table with 0.0.0.0/0, but isn't that provided already ?
**A:** If I was talking about Interget Gateways, Yes, we have to route table entries are not created automatically.

**Q:** What does this means: "Security groups are STATEFUL i.e if traffic is allowed inbound it is aloso allowed outbound" ?

- Number of security groups per instance is determined by how many ENI it's connected to (not sure about it ?)
- What is an Availability Zone ? Physically ?
- What is the meaning of "IPv4 CIDR" ? What does the values mean like "172.31.48.0/20" ?
- Can we write a Security Group rule affecting outbound network
- What is the "Tenancy" that creates a dedicated host for us and expensive ?

**Q:** What the heck is an [AWS Transit Gateway](https://aws.amazon.com/transit-gateway/) ?



## Core components
- Provision a logically isolated section of the AWS Cloud where you can launch AWS resources in a virtual network that you define.
- Think of it as your own personal data center.
- Gives you complete control over your virtual networking environment
- It has a lot of components and services  like Internet Gateways, Routing tables, Public/Private subnets ...

![[vpc.png]]

##  Key features
- It has 4 fields: Name, IPv4 CIDR block, IPv6 block, Tenacy
- Region specifc, every region come with one, you can create up to 5 VPC per region
- 200 subnets
- **Cost nothing:** VPCs, route tables, tabls, Nacls, internet, gateways, securtiy groups and subnets, VPC Peering
- **Cost money:** NAT Gateway, VPC Endpoint, VPN gateway, Customer Gateway
- DNS hostnames to allow accesss to EC2 machines via sub domains not enabled by default

## Default VPC
It allows us to be able to immediately deploy instances, it creates:
- size of /16 IPv4 CIDR block (172.31.0.0/16)
- a size /20 default subnet in each availability zone
- an internet gateway and connect it to your default VPC for machine to be able to access internet
- Default security group and associate it with your default VPC
- default network access contorl list (NACL)
- default DHCP options of your account
- When you create a VPC, it has automatically a main route table

## Default everywhere IP 0.0.0.0/0
- it represents all possible IP addresses
- In our route table for IGW we are allowing internet access
- In our security groups inbound rules, we are allowing all trafic from the internet access our public resources

## VPC peering
Allows to connect multiple VPCs over a direct network using private IP addresses
- Intances on peerred VPCs behaves just like they are on the same network
- Cross account and regions
- Star configuration: 1 Central - 4 other VPCs
- No transitive peering, perring must take place directly between VPCs
- No overlapping CIDR Blocks (Network addresses)

## Route tables 
Determines where network trafic is directed
- Each subnet in your VPC must be associated with a route table
- A subnet can only be associated with one route table at a time but we can associate multiple subnets with the same route table

## Internet gateways (IGW)
Allows access to the internet
- provides a target in route table for internet-routable traffic
- perform network address translation (NAT) for intances taht have been assigned public IPv4 addresses

## Basition / Jumpbox
EC2 instances which are security harden, they help you gain access to your private EC2 instances via SSH or RCP

- Nat Gateways / Instaces are only intended for EC2 instances to gain outbound access to the inernet for things like security updates
- Nats cannot/should not be used as Bastions
- there is a service called system manager's session manager and it replaces the need for bastions so that you don't have to 

##  Direct Connect
Establishes dedicated network connection from on-premises locations to AWS with a very fast network 50M-500M for lower bandwidth and 1GB-10GB for higher bandwidth
- Helps reduce network costs and increase bandwidth throughput (great for high trafic networks)
- Provides more consitent network experience that a typical internet-based connection (reliable and secure)

##  VPC Endpoits
- Helps keep trafic between AWS services within the AWS network
- 2 kinds: Interface endpoints and Gateway endpoints
- Interface endpoint costs arround 7.5$/month. Gateway are free
- Interface endpoints uses an Elastic Network Interface (ENI) with Private (IP) (powered by AWS PrivateLink)
- Interface endpoints is a target for a specific route in your route table
- Interface endpoints support many AWS service 
- Gateway endpoints only support DynamoDB and S3

## VPC Flow logs
- Capture IP traffic information in-and-out of network interfaces whithin VPC
- Can be turned on at the VPC, Subnet or Network interface level
- Cannot be tagged like other resources
- Cannot be edited the configuration after created
- Cannot enable flow logs for VPCs which are peered with your VPC unless it is in the same account
- Can be exported to s3 or CloudWatch Logs
- VPC logs contains the source and destination IP addresses (not hostnames)
- Some instance trafic is not monitored:
	- contacting AWS DNS servers
	- Windows licence activation
	- Traffic to and from the instance metadata address (169.254.169.254)
	- DHCP traffic
	- Any traffic to the reserved IP addresss of the default VPC router
	
Question could be asked during the exam: Does VPC flow logs contain IP addresses or domain(hostname), IP is the answer


## NACLs
NACL acts as a virtual firewall at the subnet level
- VPCs automatically give a default NACL
- each NACL contain a set of rules that can allow or deny traffic into and out of subnets
- Subnets are associated with NACLs. Subnets can only belong to a single NACL. When associating with a new NACL the subnet will be removed from the previous one.
- NACL will deny all traffic by default
- Rule # determines the order of evaluation. From lowest to highest, the highest can be 32766 and its recommended to work in 10 or 100 increments to be able to create rules in between when needed
- You can allow or deny traffic. You could block a single IP address which we cannot do with Security Groups

Usecases:
- Block a single IP address
- Deny SSH (port 22)

## Security Groups
A virtual firewall that controls the traffic to and from EC2 instances (instace level)
- All inbound traffic is blocked by default. All outbound traffic is allowed by default. 
- A rule has a particular protocol and port access level
- There are no deny rules.
- Source could be IP range or specific IP addresse or another security group (but what does that mean ?)
- Instance can belong to multiple security groups. Security groups can contain multiple EC2 instances
- Security groups are STATEFUL i.e if traffic is allowed inbound it is aloso allowed outbound
- Changes to security groups take effect immediately
- Limits: 
	- Up to 10k securtiy groups in a region (default is 2500), make a service limit increase request.
	- 60 inbound rules and 60 outbound rules per security group
	- 16 security groups per Elastic Network Interface (default is 5)
	- Number of security groups per instance is determined by how many ENI it's connected to (not sure about it ?)


- One-to-one relationship between VPC and IGW
- We can create a custom route table other than the default created with the IGW and set it to the main one for subsequent IGWs.
- One subnet for each AZ ?? What does that mean ? We can create multiple subnets in the same AZ. Maybe it means there should at least one subnet in each availability zone ?
- Subnet have a property for "Auto assinging public IPv4 address to EC2 instances" which we call "Public subnets"
- We have to create at least 3 subnets for availabilty.

## Network Address Translation (NAT)
Remap on e address space to another

- Connect private network to the internet needs a NAT Gateway to remap the private IPs
- If we have two networks with conflicing IP ranges we can use NAT to make the addresses more aggreable
- NAT instances are EC2 instances that do the remaping, there are a lot of community AMIs to use.
- NAT gateways is a managed service that launches redundant instances whithin the select AZ
- They have to be in a public subnet
- when creating a NAT instances you must disable source and destination checks
we must have a route out of the private subnet to the NAT instance 
- NAT gateways starts at 5GBps and scale up to 45GBps

## Getting hands dirty

- Public subnets are those who are configured to automatically assing public IPv4 addresses to EC2 machines
- Bastion (Gua) provides Two factor authentication, session recording ... there is also AWS's Session manager which provide most of the features except session recording
- To allow an instance running in a private subnet to access the internet, we create a NAT gateway in a public subnet and add it to the route table of the private subnet
- We cannot change the subnet for an ec2 instance
- We have to create a CloudWatch log group to be able to collect VPC Flow logs there. 
- We can't delete an Attached Internet Gateway, we havet to first detach it from the VPC.
- Egress-only Internet gateways allow outbound only access to private instances via IPv6, for IPv4 use NAT gateways.
- [NAT Instances](https://docs.aws.amazon.com/vpc/latest/userguide/VPC_NAT_Instance.html) are EC2 instances tha provide Network Addresse Translation, NAT Gateways are hightly recommended because they are managed by AWS and they are high available in each AZ
- A VPC automatically comes with a default network ACL which allows all inbound/outbound traffic. **A custom NACL denies all traffic, both inbound and outbound by default.**
