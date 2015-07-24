# docker-hellomongo
The hellomongo webapp dockerized.

To run this docker-compose project simply checkout this repo (presuming you have installed docker on your server), and run 

  docker-compose up -d
  

# boot2docker

To run this docker in a development environment install the boot2docker application, instructions on how to do this can be found here: http://boot2docker.io/

Then run:

  docker-compose up -d

and you should be able to connect to the app on:

  curl http://$(boot2docker ip)
  


# Amazon Elastic Container Service (ECS)

To deploy this via Amazon's Elastics Containter Service, install the aws-cli, instructions on how to do this and configure the application can be found here: http://aws.amazon.com/cli/

Configure the aws-cli tools 

  aws configure
  
Note: you will require an AWS access key ID and secret access key, http://docs.aws.amazon.com/cli/latest/userguide/cli-chap-getting-set-up.html

Register the task definition:

  aws ecs register-task-definition --cli-input-json file://aws/ecs/hellomongo.json
  
Create the cluster:

  aws ecs create-cluster --cluster-name hellomongo
  
Create a cloudformation stack with the Container autoscaling groups and elastic loadbalancer:

  aws cloudformation create-stack --stack-name hellomongo --template-body file://aws/cf/template.json --parameters ParameterKey=SSHLocation,ParameterValue=81.134.202.29/32 ParameterKey=KeyName,ParameterValue=HelloMongo --capabilities CAPABILITY_IAM

Run the task

  aws ecs run-task --cluster hellomongo --task-definition hellomongo:4 --count 1