## DNS
Convert domains names into routable IP addresses

- **Domain Registra** 3rd party copany who you register domains though
- **Name Server** is the server which contains the DNS records for a domain
- **Start of Authority (SOA)** contains information about the DNS zone and associated DNS records ????
- **A record** convert a domain directly to IP record
- **CNAME record** convert domain name to another
- **Time to Live (TTL)** the time that DNS record will be cached for, the lower means changes propagates faster


## Route53
Route53 is a DNS provider, register and mange domains, create record sets. Think Godaddy or NameCheap

- **Traffic Flow:** visual editor for changing routing policies, can version policy records for easy rollback
- **AWS Alias Record - AWS' smart DNS records:** detects changed IPs for AWS resources and adjusts automatically
- **Route53 Resolver:** lets you regionally route DNS queries between your VPCs and your network Hybrid Environments
- **Health checks** can be created to monitor and automatically over endpoints, You can have health checks monitor other health checks


### Routing policies
- **Simple:** Default, multiple addresses result in a random endpoint selection
- **Weighted:** split based on assigned weights
- **Latency-based:** Based on region, direct to the lowest latency for users, not neccessarly the closest region
- **Failover:** direct trafic to a secondary site if the health of the primary is not good
- **Geolocoation:** based on geographic location of request origin
- **Geo-proxmity:** based on geographic location using Bias values (needs Route53 Traffic Flow)
- **Multi-Value Answer:** just like Simple but uses health check for selection