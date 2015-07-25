# docker-hellomongo
The hellomongo webapp dockerized
--------------------------------
boot2docker
-----------
To run locally use **boot2docker**: http://boot2docker.io/

If you're feeling lucky run 

  `./boot2docker.sh`
  
The main command to note is, 

  `docker-compose up -d`
  
which creates the containers and daemonizes them. 

Connect to Tomcat on:

  `curl http://$(boot2docker ip)`
  
AWS Elastic Container Service (ECS)
--------------------------------------
This requires the aws-cli: http://aws.amazon.com/cli/, and leverages ECS via CloudFormation.

Run aws.sh:

  `./aws.sh MyAWSSshKey fivesofwarmers.com`
  
# How does it work?
Containers (heavily namespaced processes) are connected via iptables (ports) and /etc/hosts entries (links).
## docker-compose.yml
This file instructs Docker on the containers (images) to create and how to connect them (*mongo* and **dcrbsltd/hellomongo_tomcat** images in this example).
### Containers/images
The **dcrbsltd/hellomongo_tomcat** container is a **custom image** built from a base image.
#### Building an image
`cd` into the "tomcat" directory - the **Dockerfile** instructs Docker on how to install apps and files.

  `docker build -t yourdockername/hellomongo_tomcat .`
#### Upload (push) the image
Push the image to Docker hub.

  `docker push yourdockername/hellomongo_tomcat`
  
## AWS
Now the **Docker** container is in the Cloud, it is available to Amazon and can be used by its **ECS** service.

The automated configuration of Amazon is controlled by the **CloudFormation** service and **JSON template**. 

### CloudFormation
The CloudFormation template: `aws/cf/template.json` orchestrates a virtualized environment by configuring services in AWS, http://aws.amazon.com/cloudformation/,

Included in this template are:

  * **DNS records**, http://aws.amazon.com/route53/
  * **load-balancers**, http://aws.amazon.com/elasticloadbalancing/
  * **Docker containers**, http://aws.amazon.com/ecs/
  * **Availability, scaling, compute and security**, http://aws.amazon.com/ec2/

Available for hire at home or abroad. Love and kisses David. x
