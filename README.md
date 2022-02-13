Create Google Kubernetes Engine (GKE) cluster:  
- Login to Google Cloud Console  
- Create Project  
- Enable Kubernetes Engine API  
- Create Kubernetes Cluster  
- Connect / Open Cloud Shell  
- (When done: Shut down Project)  
```
gcloud container clusters get-credentials <cluster> --zone <zone> --project <project>  
```
Create Elastic Kubernetes Service (EKS) cluster:  
- Login to AWS CloudShell  
- Install kubectl  
- Install eksctl  
- (When done: Delete EKS cluster and ELB load balancer)  
```
eksctl create cluster --name <name> --region <region>  
#eksctl delete cluster --name <name> --region <region>  
```
Create Azure Kubernetes Service (AKS) cluster:  
- Login to Azure Portal  
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
- Website: <a href="https://microservices.io/" target="_blank">Microservices Architecture</a>  
- Book: <a href="https://microservices.io/book" target="_blank">Microservices Patterns</a>  
- Video (25 min): <a href="https://www.youtube.com/watch?v=BE77h7dmoQU" target="_blank">Kubernetes Documentary - Part 1/2</a>  
- Video (32 min): <a href="https://www.youtube.com/watch?v=318elIq37PE" target="_blank">Kubernetes Documentary - Part 2/2</a>  
- Blog Post: <a href="https://betterprogramming.pub/10-antipatterns-for-kubernetes-deployments-e97ce1199f2d" target="_blank">10 Anti-Patterns for Kubernetes Deployments</a>  
- Blog Post: <a href="https://codefresh.io/containers/docker-anti-patterns/" target="_blank">Docker Anti-Patterns</a>  
- Free eBook (information required): <a href="https://tanzu.vmware.com/content/ebooks/kubernetes-up-running-dive-into-the-future-of-infrastructure" target="_blank">O’Reilly: Kubernetes Up & Running - Dive into the Future of Infrastrucutre</a>  
- Free eBook (information required): <a href="https://library.devops.com/oreilly-kubernetes-patterns-for-designing-cloud-native-apps" target="_blank">O’Reilly: Kubernetes Patterns for Designing Cloud-Native Apps</a>  
- Free eBook (information required): <a href="https://tanzu.vmware.com/content/ebooks/production-kubernetes" target="_blank">O’Reilly: Production Kubernetes</a>  
