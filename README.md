# Deployment-of-Reddit-Clone-Application
Deployed a Reddit Clone using Docker and Kubernetes, integrating Ingress for traffic management. This project demonstrates essential DevOps practices like CI/CD, containerization, scalability, and automation.
# Architecture Diagram:
![image](https://github.com/user-attachments/assets/a76563a0-f193-4004-b09d-dead8b8cd531)
# Prerequisite:
1.AWS EC2 instance — CI-Server (t2.medium) & Deployment-Server(t2.xlarge) with AMI -Ubuntu.

2.Minikube

3.Docker

4.Kubectl
 # Start with the CI-Server. Connect by using EC2 Connect.
# Update and install docker:
sudo apt-get update
sudo apt-get install docker.io -y
sudo usermod -aG docker $USER && newgrp docker
# Clone Source Code:
$ git clone https://github.com/LondheShubham153/reddit-clone-k8s-ingress.git
Then cd into that repo
# Dockerize the App:
Write a Dockerfile for this application.
vim Dockerfile.yml
# Build Docker Image:
$ docker build -t <DockerHub_Username>/<Imagename>
$ docker build . -t arthisuresh0210157/reddit-clone
# Push to Docker Hub:
$docker login
# Once login is successfull push the image .
$ docker push <DockerHub_Username>/<Imagename>
$ docker push arthisuresh0210157/reddit-clone
# Lets connect the Depolyment-Server using EC2 Connect.
# Again update and install docker in the Deployment-Server also.
sudo apt-get update
sudo apt-get install docker.io -y
sudo usermod -aG docker $USER && newgrp docker
# Now install Minikube and Kubectl :
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube 

sudo snap install kubectl --classic
minikube start --driver=docker
# Create Kubernetes Manifest file & Deploy to Kubernetes:
vim Deployment.yml
vim Service.yml
# Apply be using this command
kubectl apply -f Deployment.yml
kubectl apply -f Service.yml
# Check the deployment and service
kubectl get deployment
kubectl get svc
# Enable ingress on minikube:
$minikube addons enable ingress
# Write manifest file for ingress:
vim ingress.yml
# Deploy ingress file and check:
$kubectl apply -f ingress.yml
$kubectl get ingress ingress-reddit-app
# Expose and Test:
$kubectl expose deployment reddit-clone-deployment — type=NodePort
# Get the url :
$minikube service reddit-clone-service — url
# You can test your deployment using
curl -L http://192.168.49.2:31000.
192.168.49.2 is a minikube ip & Port 31000 is defined in Service.yml
# Next, port forward:
$kubectl port-forward svc/reddit-clone-service 3000:3000 — address 0.0.0.0 &
You can access the app in EC2_ip:3000
