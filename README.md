# Final Assignment
This assignmnet demonstrates how to:

1. Set up AWS infrastructure using Terraform.
2. Create a simple python application and Dockerize it.
3. Set up a GitHub Actions pipeline for Docker build and push to Docker Hub.
4. Deploy the app to a Minikube local Kubernetes cluster.
5. Set up Prometheus and Grafana for monitoring.

## Prerequisites

1. Terraform
2. Docker
3. Kubernetes (Minikube)
4. Helm
5. GitHub account with Docker Hub credentials and aws account with required access

## Steps to Run

### 1. Set up AWS Infrastructure (Terraform)

Run the following commands to set up the EC2 instance using Terraform:

        terraform init
        terraform plan
        terraform apply

### 2. Dockerize python Application

- Create a simple puthon application.
- Write a Dockerfile to containerize the app.

- build the docker Image
  
        docker build -t dockerusername/my-app .

- Push Docker Image to Docker Hub
  
        docker push  yourdockerusername/my-app

 ### 3.Create GitHub Actions Pipeline
- Set up a .github/workflows/docker.yml pipeline for:

  -Building Docker image.
  
  -Pushing to Docker Hub.
  
  -Scanning the Docker image with Trivy.

### 4. Set up Minikube Locally

First you need to install minikube if it is not alreday installed in your system. After installing minikube in your system,Start the Minikube cluster with:

         minikube start

To Deploy Dockerized Application to Minikube : first configure Minikube to use the Docker daemon inside the Minikube cluste with mentioned commands:

       eval $(minikube -p minikube docker-env)
       
Then To deploy it in minikube , run following commands

       docker build -t <yourdockerusername/my-app .
       docker push yourdockerusername/myapp
       kubectl apply -f deploy.yaml
       kubectl apply -f service.yaml

### 5. Install Prometheus and Grafana in Minikube
I have used helm to install prometheus nad grafana:

       helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
       grafana https://grafana.github.io/helm-charts
       helm repo update

       helm install prometheus prometheus-community/kube-prometheus-stack
       helm install grafana grafana/grafana

To access prometheus dashboard run the below commad:

       kubectl port-forward --namespace default svc/prometheus-operated 9090:9090

Then, open your browser and go to: http://localhost:9090

To access grafana dashboard

       kubectl get secret --namespace default grafana -o jsonpath="{.data.admin-password}" | base64 --decode

This will take you to the browser where it ask you for the username and password , you will get your password while running the above commad.

## Configure Grafana to Use Prometheus as Data Source
-In Grafana, go to Configuration in the left sidebar.

-Click Data Sources > Add Data Source.

-Choose Prometheus from the list of available data sources.

-Set the URL field to: http://prometheus-operated:9090

-Click Save & Test to confirm the connection.

## Create Dashboard in grafana
    
-Go to the Dashboards section in Grafana.

-Click Import,
    
-Go to grafana.com where you can search for kuberenetes dashboard , copy their id

-Paste and import it.




    
