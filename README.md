**Google Kubernetes Engine:**  

...  

**Elastic Kubernetes Service:**  

...  

**Azure Kubernetes Service:**

- Log into portal.azure.com  
- Create Resource Group  
- Create Kubernetes Service  
- Open Cloud Shell  

az account set --subscription &lt;subscription&gt;  
az aks get-credentials --resource-group &lt;resource-group&gt; --name &lt;name&gt;  

---

git clone https://github.com/bstraehle/kubernetes.git  
cd kubernetes  
kubectl create -f rest-api-deploy.yaml  
kubectl create -f rest-api-service.yaml  
kubectl create -f mvc-app-deploy.yaml  
kubectl create -f mvc-app-service.yaml  
kubectl get deployments,services  

---

**URL:** &lt;external-ip&gt;:&lt;port&gt;  
