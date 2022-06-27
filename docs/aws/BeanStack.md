# BeanStack
Elastic Beanstalk handles the deployment, from capacity provisioning, load balancing, auto-scaling  to application health monitoring   
- When you want to run a web-application but you don't want to have think about the underlying  infrastructure.   
- It costs nothing to use Elastic Beanstalk (only the resources it provisions eg. RDS, ELB, EC2  
- Recommended for test or development apps. Not recommended for production use   
- You can choose from the following preconfigured platforms: Java, .NET, PHP, Node.js, Python, Ruby, Go, and Docker
- BeanStack needs some configuration files to know how to run your app, for nodejs `.ebextensions/nodecommand.config` and `.ebextensions/staticfiles.config`
- You can run dockerized environments on Elastic Beanstalk.