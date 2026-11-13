Automated Docker Image Build and Deployment using Jenkins

Project Overview:-

This project demonstrates a basic CI/CD pipeline using Jenkins, Docker, Docker Hub, GitHub, and AWS EC2.

The pipeline automates Docker image building, uploading the image to Docker Hub, and deploying a website on an AWS EC2 instance.

The pipeline is manually triggered through Jenkins using Build Now.

Technologies Used:-

. AWS EC2 – Cloud server
 
. Ubuntu Linux – Operating system

. Jenkins – CI/CD automation

. Docker – Containerization

. Docker Hub – Docker image storage

. Git & GitHub – Source code management

. Nginx – Web server

Project Workflow:-

1. Store the website source code in GitHub.

2. Jenkins retrieves the source code.

3. Jenkins builds a Docker image using the Dockerfile.

4. The image is tagged with the Jenkins build number and latest.

5. Jenkins pushes the image to Docker Hub.

6. Jenkins pulls the image onto the EC2 instance.

7. The website is deployed inside a Docker container.

8. Jenkins verifies the website using curl.

Project Structure:-

docker-hub-deploymnent/

├── index.html
├── Dockerfile
├── Jenkinsfile
└── README.md

Dockerfile:-

FROM nginx:alpine

COPY index.html /usr/share/nginx/html/index.html

EXPOSE 80

This Dockerfile uses Nginx Alpine to serve the static website.

Docker Hub Repository:-

https://hub.docker.com/r/vivekkumarjha/devops-website

Pull the latest image:-

docker pull vivekkumarjha/devops-website:latest

Deployment Details:-

Container Name: devops-test

Host Port: 8081

Container Port: 80

Platform: AWS EC2

Access the deployed website:-

http://EC2_PUBLIC_IP:8081

Replace EC2_PUBLIC_IP with your EC2 instance's public IP address.

Verification Commands:-

Check running containers:
docker ps

Check Docker images:
docker images

Test website availability:
curl http://localhost:8081

Key Learnings:-

. Working with Linux commands and AWS EC2.

. Managing source code using Git and GitHub.

. Creating Docker images using Dockerfiles.

. Running Docker containers.

. Pushing images to Docker Hub.

. Creating Jenkins pipelines using Jenkinsfile.

. Managing Jenkins credentials.

. Automating application deployment.
