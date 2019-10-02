# AWS Deploys with Docker

This repo has the code to deploy a multi container app to [AWS Beanstalk](https://aws.amazon.com/elasticbeanstalk/) service, which in turn provides us with a few perks out of the box:

- Auto association with S3
- Auto Load Balancer
- Auto Scaling
- Use of ECS cluster

# App's purpose

The app is a Fibonacci calculator that takes an index from the user and then calculates the value of that index in the Fibonacci sequence.

The indexes that the user inserts as time passes are stored in a Postgres relational database and the results of the calculations are stored in a Redis server that acts as a cache.

The actual calculations of the Fibonacci index are done by a separate nodejs worker that in turn sets the results back into redis.

# Development

For development purposes, you can use docker-compose which will create the environment needed to test the app locally, with all services already configured.

# Production

## AWS Configuration

In the AWS part there are several configurations needed that this repo doesn't provide. Namely:

- Creation of a new Elastic Beanstalk app with Docker Multi-Container settings defined
- The S3 bucket is configured automatically when you create the EB app, and will host the project contents for AWS to deploy
- Creation of a Security Group under VPC AWS service
- Creation of a Postgres instance under RDS AWS service
- Associate the security group to Postgres instance 
- Creation of a Redis instance unser ElastiCache AWS service
- Associate the security group to Redis instance
- Define the necessary env variables in Elastic Beanstalk AWS service for Postgres and Redis instances
- Create new user under IAM AWS service to use in TravisCI to later deploy to AWS

These kinds of configurations can later on be set on something like [Terraform](https://www.terraform.io/).

## TravisCI

This repo uses Travis as a CI and the [.travis.yml](./.travis.yml) script is responsible for:

- Testing the app
- Building the docker images
- Pushing the docker images to [Docker Hub](https://hub.docker.com)
- Request deploy from AWS Elastic Beanstalk service

# License

[MIT](./LICENSE)