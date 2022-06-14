# VPS vs Dedicated server Hosting
- VPS Hosting offers is a VM running in a shared server with others, each VM has its own resource limits: CPU, RAM ... which usually can be changed on-demand

- Dedicated servers offers a physical server with a predefined set of resources dedicated exclusively to one user which has much more horse power and thus much more expensive

VPS are cheap and offer on-demand resources, you can start small and allocate resources whenever you need them.

Dedicated servers offer more resource, control and security since you're the only one using a _physical_ server, no one else is running anything but you.

Note: If you are just starting, you have a regular application and you don't know what you need, just use a VPS you won't usually see any difference at this stage.

# Cloud providers comparison for VPS offers

## Contabo
Prices are dead cheap, it's is too good to be true you can get a 6vCPU, 16 GB of RAM and 100 GB NVMe VPS for $12 which only gets you a 1vCPU, 2 GB of RAM and 50 GB NVMe in Digital Ocean

According to [this benchmark](https://gist.github.com/sutlxwhx/8f9370d5f0a946b2d43e397db4b1ae38#discussion), [this](https://medium.com/@bostjanpisler/contabo-vs-digitalocean-which-to-choose-ed9b5b8ef45c) [this one](https://www.vpsbenchmarks.com/compare/contabo_vs_docean) they have the lowest performance

Despite that, I'm still curious and seduced by the price and I'm wiling to give it a shot and see the results myself

## Digital Ocean
Moderate prices, simple to use, predictable and easy to understand pricing

https://www.digitalocean.com/pricing

$20: 2vCPU, 4 GB RAM, 80 GB SSD

## Google Cloud
High prices, a hell of a platform with tons and tons of services, complex and fine-grained pricing model: it charges for CPU, RAM, Disk, Network separately and has many many machine types and customizations

Prices for an E2 machine type, there are other expensive types like N1 and N2 but Google claim to offer similar performance at lower cost with E2 machines

$25: 2vCPU, 4 GB RAM
$13: 80 GB SSD or $3.2 for HDD disk or $8 for a balanced SSD disk

if we go for the balanced SSD, GCP is $13/month (or 50%) more expensive than DigitalOcean

## AWS
Pretty much the same as GCP with more services and more complex pricing models, also the learning curve is steeper an requires much more knowledge and time to get started.

Not used it so much, I can't estimate the prices right now

## AWS Lightsail
> Virtual private servers for a low, predictable price

Looks like a DigitalOcean offered by AWS, similar prices and offers https://aws.amazon.com/lightsail/pricing/?opdp1=pricing

Exact same price ($20) as Digital Ocean
