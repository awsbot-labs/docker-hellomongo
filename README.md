# docker-hellomongo
The hellomongo webapp dockerized.

To run this docker-compose project simply checkout this repo, and run 

  `docker-compose up -d`
  
(presuming you have installed docker on your server)

# boot2docker
To run this docker in a development environment install the boot2docker application, instructions on how to do this can be found here: http://boot2docker.io/

Then run:

  `docker-compose up -d`

and you should be able to connect to the app on:

  `curl http://$(boot2docker ip)`
  


# Amazon Elastic Container Service (ECS)
This is slightly more envolved and requires access to an Amazon Web Services account.

To deploy this via Amazon's Elastics Containter Service, install the aws-cli, instructions on how to do this and configure the application can be found here: http://aws.amazon.com/cli/

Configure the aws-cli tools:

  `aws configure`
  
Note: you will require an AWS access key ID and secret access key, http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html

Register the task definition:

  `aws ecs register-task-definition --cli-input-json file://aws/ecs/task_definition_.json`
  
Create the cluster:

  `aws ecs create-cluster --cluster-name hellomongo`
  
Create a cloudformation stack with the Container autoscaling groups and elastic loadbalancer, note that you will need to pass the name of an SSH Key and an IP address range (i.e. whatsmyip) to connect from:

  `aws cloudformation create-stack --stack-name hellomongo --template-body file://aws/cf/template.json --parameters ParameterKey=SSHLocation,ParameterValue=99.99.99.99/32 ParameterKey=KeyName,ParameterValue=HelloMongo --capabilities CAPABILITY_IAM`

Finally run the task definition in the cluster:

  `aws ecs run-task --cluster hellomongo --task-definition hellomongo:1 --count 1`
  
Ans monitor the status od the task definition in the console, once completed succesfully, the EC2 instance should connect to the loadbalancer, and you should then be able to connect to the Elastic Loadbalancer endpoint to view the webapp.