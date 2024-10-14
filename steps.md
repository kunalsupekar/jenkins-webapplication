Here’s a setup for your project involving AWS ECS to create a sample Docker image for running a web application:

Project Title:

ECS-Docker-WebApp

GitHub Repo Name:

ecs-docker-webapp

Project Structure:

ecs-docker-webapp/
├── app/
│   └── index.html
├── Dockerfile
├── ecs-task-definition.json
└── README.md

Code:

	1.	index.html (Web Application File):

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Sample ECS Docker WebApp</title>
</head>
<body>
    <h1>Welcome to the Sample ECS Docker Web Application</h1>
</body>
</html>


	2.	Dockerfile:

# Use the official Nginx image
FROM nginx:alpine

# Copy the HTML file to the Nginx default location
COPY app/index.html /usr/share/nginx/html/index.html

# Expose port 80
EXPOSE 80


	3.	ecs-task-definition.json (Task Definition for ECS):

{
  "family": "ecs-docker-webapp",
  "networkMode": "awsvpc",
  "containerDefinitions": [
    {
      "name": "webapp-container",
      "image": "<YOUR_ECR_REPO_URL>:latest",
      "essential": true,
      "portMappings": [
        {
          "containerPort": 80,
          "hostPort": 80,
          "protocol": "tcp"
        }
      ],
      "memory": 512,
      "cpu": 256
    }
  ],
  "requiresCompatibilities": [
    "FARGATE"
  ],
  "cpu": "256",
  "memory": "512",
  "executionRoleArn": "<YOUR_EXECUTION_ROLE_ARN>",
  "taskRoleArn": "<YOUR_TASK_ROLE_ARN>"
}



Steps:

	1.	Create GitHub Repository:
	•	Create a new GitHub repository named ecs-docker-webapp and clone it locally.
	2.	Build Docker Image:

docker build -t ecs-docker-webapp .


	3.	Push Docker Image to AWS ECR:
	•	Create an ECR repository in AWS.
	•	Authenticate Docker to your AWS account:

aws ecr get-login-password --region <YOUR_REGION> | docker login --username AWS --password-stdin <YOUR_ECR_URL>


	•	Tag and push the Docker image:

docker tag ecs-docker-webapp:latest <YOUR_ECR_URL>:latest
docker push <YOUR_ECR_URL>:latest


	4.	Create ECS Cluster:
	•	Navigate to ECS in the AWS Management Console and create a new ECS cluster (Fargate).
	5.	Register Task Definition:
	•	Use the ecs-task-definition.json file to register a new task definition in ECS.
	6.	Deploy Web Application:
	•	Create an ECS service linked to the task definition, select a VPC, subnets, and configure auto-scaling if needed.
	•	Expose the service via a load balancer to access the web app.
	7.	Test the Web Application:
	•	Access the web application using the Load Balancer DNS name in your browser.
