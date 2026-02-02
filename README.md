# BoardgameListingWebApp

## Description 

**Board Game Database Full-Stack Web Application.**
This web application displays lists of board games and their reviews. While anyone can view the board game lists and reviews, they are required to log in to add/ edit the board games and their reviews. The 'users' have the authority to add board games to the list and add reviews, and the 'managers' have the authority to edit/ delete the reviews on top of the authorities of users.  

## Technologies

- Java
- Spring Boot
- Amazon Web Services(AWS) EC2
- Thymeleaf
- Thymeleaf Fragments
- HTML5
- CSS
- JavaScript
- Spring MVC
- JDBC
- H2 Database Engine (In-memory)
- JUnit test framework
- Spring Security
- Twitter Bootstrap
- Maven

## Features

- Full-Stack Application
- UI components created with Thymeleaf and styled with Twitter Bootstrap
- Authentication and authorization using Spring Security
  - Authentication by allowing the users to authenticate with a username and password
  - Authorization by granting different permissions based on the roles (non-members, users, and managers)
- Different roles (non-members, users, and managers) with varying levels of permissions
  - Non-members only can see the boardgame lists and reviews
  - Users can add board games and write reviews
  - Managers can edit and delete the reviews
- Deployed the application on AWS EC2
- JUnit test framework for unit testing
- Spring MVC best practices to segregate views, controllers, and database packages
- JDBC for database connectivity and interaction
- CRUD (Create, Read, Update, Delete) operations for managing data in the database
- Schema.sql file to customize the schema and input initial data
- Thymeleaf Fragments to reduce redundancy of repeating HTML elements (head, footer, navigation)

## How to Run
1. Clone the repository
2. Open the project in your IDE of choice
3. Run the application
4. To use initial user data, use the following credentials.
  - username: bugs    |     password: bunny (user role)
  - username: daffy   |     password: duck  (manager role)
5. You can also sign-up as a new user and customize your role to play with the application! 😊




# project
End-to-End DevSecOps CI/CD Pipeline with Security & Monitoring

Project Overview: Designed and implemented an automated DevSecOps CI/CD pipeline using Jenkins and GitHub, integrating SonarQube (SAST), OWASP Dependency-Check, and Trivy to identify code quality issues, vulnerable dependencies, and container image vulnerabilities. Containerized the application using Docker and deployed using Kubernetes manifests, with continuous availability and performance monitoring via Prometheus, Blackbox Exporter, and Grafana to ensure reliable and secure deployments.

Project Objectives:
AWS EC2,
Jenkins,
Docker,
Kubernetes (kubeadm),
SonarQube,
Nexus Repository,
Maven,
Trivy,
Kubeaudit,
Prometheus,
Blackbox Exporter,
Grafana,
GitHub,

☁️ Stage 1: AWS Infrastructure Setup
EC2 Instances Created
Purpose	Instances
Kubernetes Cluster	1 Master + 2 Worker Nodes
CI/CD & Tools	Jenkins, SonarQube, Nexus
Monitoring	Prometheus, Grafana
Configuration

OS: Ubuntu

Security Group Ports:

22 (SSH)

8080 (Jenkins)

9000 (SonarQube & Nexus)

30000–32767 (Kubernetes NodePort)

☸️ Stage 2: Kubernetes Cluster Setup (All Nodes)

Run on Master, Slave1, Slave2:

sudo apt-get update
sudo apt install docker.io -y
sudo chmod 666 /var/run/docker.sock

sudo apt-get install -y apt-transport-https ca-certificates curl gnupg
sudo mkdir -p -m 755 /etc/apt/keyrings

curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /' | sudo tee /etc/apt/sources.list.d/kubernetes.list

sudo apt update
sudo apt install -y kubeadm=1.28.1-1.1 kubelet=1.28.1-1.1 kubectl=1.28.1-1.1

☸️ Stage 3: Initialize Kubernetes Master
sudo kubeadm init --pod-network-cidr=10.244.0.0/16


Configure kubectl:

mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config


Install networking & ingress:

kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/controller-v0.49.0/deploy/static/provider/baremetal/deploy.yaml


Verify:

kubectl get nodes

🔐 Stage 4: Kubernetes Security Audit (Kubeaudit)
wget https://github.com/Shopify/kubeaudit/releases/download/v0.22.2/kubeaudit_0.22.2_linux_amd64.tar.gz
tar -xvzf kubeaudit_0.22.2_linux_amd64.tar.gz
sudo mv kubeaudit /usr/local/bin
kubeaudit all

🔧 Stage 5: Install Jenkins, SonarQube & Nexus
Docker Installation (All Tool Nodes)
sudo apt-get update
sudo apt-get install -y ca-certificates curl
sudo install -m 0755 -d /etc/apt/keyrings
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg -o /etc/apt/keyrings/docker.asc
sudo chmod a+r /etc/apt/keyrings/docker.asc

echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.asc] \
https://download.docker.com/linux/ubuntu $(. /etc/os-release && echo "$VERSION_CODENAME") stable" \
| sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io
sudo chmod 666 /var/run/docker.sock

🧪 SonarQube Setup
docker run -d --name sonar -p 9000:9000 sonarqube:lts-community


Access:

http://<sonarqube-ip>:9000

📦 Nexus Repository Setup
docker run -d --name nexus -p 9000:9000 sonatype/nexus3:latest


Access:

http://<nexus-ip>:9000

🔄 Stage 6: Jenkins DevSecOps CI/CD Pipeline
Pipeline Features

Git Checkout

Maven Compile & Test

Trivy File System Scan

SonarQube Code Analysis

Quality Gate Enforcement

Artifact Publish to Nexus

Docker Image Build & Tag

Trivy Image Scan

Push to Docker Hub

Deploy to Kubernetes

Deployment Verification

Email Notification

✔ Pipeline Status: SUCCESS

