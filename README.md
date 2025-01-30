# NBA-Schedule-API System
This project demonstrates building a containerized API management system for querying sports data. It uses Amazon ECS (Fargate) for running containers, Amazon API Gateway for exposing REST endpoints, and an external Sports API for real-time sports data. The project showcases advanced cloud computing practices, including API management, container orchestration, and secure AWS integrations.

## Prerequisites:
Sports API Key: Sign up for a free account and subscription & obtain your API Key at serpapi.com
AWS Account: Create an AWS Account & have basic understanding of ECS, API Gateway, Docker & Python
AWS CLI Installed and Configured: Install & configure AWS CLI to programatically interact with AWS
Serpapi Library: Install Serpapi library in local environment "pip install google-search-results"
Docker CLI and Desktop Installed: To build & push container images

## Archicture:
![Architecture](https://github.com/mbengiivy/NBA-GD-schedule/blob/main/images/Screenshot%202025-01-30%20213926.png)


## SETUP INSTRUCTIONS:
1. Clone this repository using the git clone command:
git clone https://github.com/mbengiivy/NBA-GD-schedule.git
2. Open the repository
cd NBA-GD-schedule
3. Using the terminal, create an ECR repo:
aws ecr create-repository --repository-name sports-api --region us-east-1
4. Authenticate,build and push the docker image:
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com

docker build --platform linux/amd64 -t sports-api .
docker tag sports-api:latest <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/sports-api:sports-api-latest
docker push <AWS_ACCOUNT_ID>.dkr.ecr.us-east-1.amazonaws.com/sports-api:sports-api-latest
5. Create an ECS cluster and task definition:
(Create manually through the console)
6. Create an API Gateway REST API:
(Create manually through the console)
7. Deploy the API Gateway REST API to the ECS cluster:
8. Test the system(using curl or using the browser)
9. Monitor the system using AWS CloudWatch and AWS X-Ray


## How to set up ECS:
1. Create an ECS cluster
2. Create a task definition(use any name for the container, use the Image URI from the ECR repo, Container Port-8080, protocol-TCP and define env variable-SPORTS-API-KEY)

## Create a service
1. Go to Clusters → Select Cluster → Service → Create.
2. Capacity provider: Fargate
3. Select Deployment configuration family (sports-api-task)
4. Name your service (sports-api-service)
5. Desired tasks: 2
6. Networking: Create new security group
7. Networking Configuration:
8. Type: All TCP
9. Source: Anywhere
10. Load Balancing: Select Application Load Balancer (ALB).
11. ALB Configuration:
12. Create a new ALB:
13. Name: sports-api-alb
14. Target Group health check path: "/sports"
15. Create service


## Configure API Gateway
1. Create a New REST API:
2. Go to API Gateway Console → Create API → REST API
3. Name the API (e.g., Sports API Gateway)
4. Set Up Integration:
5. Create a resource /sports
6. Create a GET method
7. Choose HTTP Proxy as the integration type
8. Enter the DNS name of the ALB that includes "/sports" (eg. http://sports-api-alb-<AWS_ACCOUNT_ID>.us-east-1.elb.amazonaws.com/sports
9. Deploy the API:
10. Deploy the API to a stage (e.g., prod)
11. Note the endpoint URL