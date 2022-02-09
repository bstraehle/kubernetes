Create Google Kubernetes Engine (GKE) cluster:  
- Log into Google Cloud Console  
- Create Project  
- Enable Kubernetes Engine API  
- Create Kubernetes Cluster  
- Connect / Open Cloud Shell  
- (When done: Shut down Project)  
```
gcloud container clusters get-credentials <cluster> --zone <zone> --project <project>  
```
Create Elastic Kubernetes Service (EKS) cluster:  
- Log into AWS CloudShell  
- Install kubectl  
- Install eksctl  
- (When done: Delete EKS cluster and ELB load balancer)  
```
eksctl create cluster --name <name> --region <region>  
#eksctl delete cluster --name <name> --region <region>  
```
Create Azure Kubernetes Service (AKS) cluster:  
- Log into Azure Portal  
- Create Resource Group  
- Create Kubernetes Service  
- Connect / Open Cloud Shell  
- (When done: Delete Resource Group)  
```
az account set --subscription <subscription>  
az aks get-credentials --resource-group <resource-group> --name <name>  
```
Create Kubernetes deployments and services:  
```
git clone https://github.com/bstraehle/kubernetes.git  
cd kubernetes  
kubectl create -f rest-api-deploy.yaml  
kubectl create -f rest-api-service.yaml  
kubectl create -f mvc-app-deploy.yaml  
kubectl create -f mvc-app-service.yaml  
kubectl get deployments,services  
```
URL:  
```
<external-ip>:<port>  
```
Useful Links:  
- Website: <a href="https://microservices.io/">Microservices Architecture</a>  
- Book: <a href="https://microservices.io/book">Microservices Patterns</a>  
- Video (25 min): <a href="https://www.youtube.com/watch?v=BE77h7dmoQU">Kubernetes Documentary - Part 1/2</a>  
- Video (32 min): <a href="https://www.youtube.com/watch?v=318elIq37PE">Kubernetes Documentary - Part 2/2</a>  
- <a href="https://betterprogramming.pub/10-antipatterns-for-kubernetes-deployments-e97ce1199f2d">10 Anti-Patterns for Kubernetes Deployments</a>  
- <a href="https://codefresh.io/containers/docker-anti-patterns/">Docker Anti-Patterns</a>  
- Free eBook (email address required): <a href="https://tanzu.vmware.com/content/ebooks/production-kubernetes">Production Kubernetes</a>  
