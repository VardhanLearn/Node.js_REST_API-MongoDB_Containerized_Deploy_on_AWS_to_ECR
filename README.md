# 🚀 Node.js Docker API Deployment on AWS ECR & EC2

This project demonstrates how to **containerize a Node.js REST API using Docker**, **push the image to AWS Elastic Container Registry (ECR)**, and **deploy it on an AWS EC2 instance**.

It is designed as a **real-world DevOps project** showcasing Docker, AWS, and deployment workflows.

---

## 📌 Project Overview

**What this project covers**
- Node.js REST API using Express
- Dockerfile for containerization
- AWS ECR for image storage
- AWS EC2 for container deployment
- End-to-end Docker → AWS deployment workflow

**Why this project?**
- Practical backend service (not a static website)
- Covers Docker build, tag, push, pull lifecycle
- Useful for DevOps / Cloud interviews
- Easily extendable to ECS, EKS, or CI/CD pipelines

---

## 🏗️ Architecture

Local Machine
│
├── Docker Build
│
├── Push Image → AWS ECR
│
└── EC2 Instance
└── Pull Image from ECR
└── Run Docker Container


---

## 📁 Project Structure

node-docker-api/
├── app.js
├── package.json
├── Dockerfile
└── .dockerignore


---

## 🧑‍💻 Application Details

### API Endpoints

| Endpoint | Method | Description |
|--------|--------|------------|
| `/` | GET | Returns app status message |
| `/health` | GET | Health check endpoint |

---

## 🐳 Docker Configuration

### Dockerfile
```dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package.json .
RUN npm install

COPY . .

EXPOSE 3000
CMD ["npm", "start"]

.dockerignore
node_modules
npm-debug.log


⚙️ Prerequisites

AWS Account

IAM user with:

AmazonEC2ContainerRegistryFullAccess

AmazonEC2FullAccess

AWS CLI installed and configured

Docker installed locally

EC2 key pair

🔧 Step-by-Step Deployment Guide
1️⃣ Build Docker Image Locally
docker build -t node-api:1.0 .

2️⃣ Create AWS ECR Repository
aws ecr create-repository \
  --repository-name node-api \
  --region ap-south-1

3️⃣ Authenticate Docker to ECR
aws ecr get-login-password --region ap-south-1 \
| docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.ap-south-1.amazonaws.com

4️⃣ Tag Docker Image
docker tag node-api:1.0 \
<ACCOUNT_ID>.dkr.ecr.ap-south-1.amazonaws.com/node-api:1.0

5️⃣ Push Image to ECR
docker push <ACCOUNT_ID>.dkr.ecr.ap-south-1.amazonaws.com/node-api:1.0

☁️ EC2 Deployment
6️⃣ Launch EC2 Instance

AMI: Amazon Linux 2

Instance Type: t2.micro

Security Group:

Port 22 (SSH)

Port 3000 (Application)

7️⃣ Install Docker on EC2
sudo yum update -y
sudo amazon-linux-extras install docker -y
sudo service docker start
sudo usermod -aG docker ec2-user


Reconnect to EC2 after this step.

8️⃣ Authenticate EC2 to ECR
aws ecr get-login-password --region ap-south-1 \
| docker login --username AWS --password-stdin <ACCOUNT_ID>.dkr.ecr.ap-south-1.amazonaws.com

9️⃣ Pull Image from ECR
docker pull <ACCOUNT_ID>.dkr.ecr.ap-south-1.amazonaws.com/node-api:1.0

🔟 Run Container
docker run -d -p 3000:3000 \
<ACCOUNT_ID>.dkr.ecr.ap-south-1.amazonaws.com/node-api:1.0

✅ Verification

Open in browser:

http://<EC2_PUBLIC_IP>:3000

