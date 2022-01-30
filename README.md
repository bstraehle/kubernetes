**Google Kubernetes Engine:**  

...  

---

**Elastic Kubernetes Service:**  

...  

---

**Azure Kubernetes Service:**  

- Log into Azure Portal  
- Create Resource Group  
- Create Kubernetes Service  
- Open Cloud Shell  

```
az account set --subscription <subscription>  
az aks get-credentials --resource-group <resource-group> --name <name>  
```

---

```
git clone https://github.com/bstraehle/kubernetes.git  
cd kubernetes  
kubectl create -f rest-api-deploy.yaml  
kubectl create -f rest-api-service.yaml  
kubectl create -f mvc-app-deploy.yaml  
kubectl create -f mvc-app-service.yaml  
kubectl get deployments,services  
```

**URL:** &lt;external-ip&gt;:&lt;port&gt;  

---

**Useful Links:**  

- Video (25 min): <a href="https://www.youtube.com/watch?v=BE77h7dmoQU">Kubernetes Documentary - Part 1/2</a>  
- Video (32 min): <a href="https://www.youtube.com/watch?v=318elIq37PE">Kubernetes Documentary - Part 2/2</a>  
- <a href="https://betterprogramming.pub/10-antipatterns-for-kubernetes-deployments-e97ce1199f2d">10 Anti-Patterns for Kubernetes Deployments</a>  
- <a href="https://codefresh.io/containers/docker-anti-patterns/">Docker Anti-Patterns</a>  
- Free O'Reilly eBook (email address required): <a href="https://tanzu.vmware.com/content/ebooks/production-kubernetes">Production Kubernetes</a>  
