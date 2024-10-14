# ECS Docker Web Application

AWS ECS to create a sample Docker image for running a web application

## Prerequisites

- AWS CLI configured
- Docker installed
- AWS ECR repository created
- ECS cluster created

## Project Structure:
```
ecs-docker-webapp/
├── app/
│   └── index.html
├── Dockerfile
├── ecs-task-definition.json
└── README.md
```

## Steps

1. **Clone the repository:**
```
git clone https://github.com/atulkamble/ecs-docker-webapp.git
cd ecs-docker-webapp
```
2. **Build Docker Image:**
```
docker build -t ecs-docker-webapp .
example: sudo docker build -t atuljkamble/ecs-docker-webapp .
```
3. **Push to ECR:**
```
aws ecr get-login-password --region <YOUR_REGION> | docker login --username AWS --password-stdin <YOUR_ECR_URL>
docker tag ecs-docker-webapp:latest <YOUR_ECR_URL>:latest
docker push <YOUR_ECR_URL>:latest
```
4. **Create ECS Cluster:**
Navigate to ECS in the AWS Management Console and create a new ECS cluster (Fargate).

5. **Register ECS Task Definition:**
Use the ecs-task-definition.json to register a new task definition in ECS.

6. **Deploy to ECS:**
Create a service in your ECS cluster, link it to the task definition, and deploy the web application.
Expose the service via a load balancer to access the web app

7. **Access the Web Application:**
After the service is running, access the application via the Load Balancer’s DNS.
The application is a simple HTML page served by an Nginx web server.
