# Kubernetes

[![CI](https://github.com/bstraehle/kubernetes/actions/workflows/ci.yml/badge.svg)](https://github.com/bstraehle/kubernetes/actions/workflows/ci.yml)

Deploy the demo [MVC app](https://github.com/bstraehle/dotnet-mvc-app) and [REST API](https://github.com/bstraehle/dotnet-rest-api) to Kubernetes (EKS, AKS, GKE, or a local [kind](https://kind.sigs.k8s.io/) cluster) with [Kustomize](https://kubectl.docs.kubernetes.io/guides/introduction/kustomize/).

## Table of Contents

1. [What Gets Deployed](#1-what-gets-deployed)
2. [Create a Cluster](#2-create-a-cluster)
3. [Deploy](#3-deploy)
4. [Access the App](#4-access-the-app)
5. [Clean Up](#5-clean-up)
6. [CI](#6-ci)
7. [Resources](#7-resources)

## 1. What Gets Deployed

All resources are in the `demo` namespace ([`manifests/`](manifests)):

| Resource | MVC app | REST API |
| --- | --- | --- |
| Deployment (2 replicas) | `mvc-app` | `rest-api` |
| Service | `mvc-app-service` (LoadBalancer, port 8000) | `rest-api-service` (ClusterIP, port 8001) |
| ServiceAccount (no API token mounted) | `mvc-app` | `rest-api` |
| PodDisruptionBudget (min 1 available) | `mvc-app` | `rest-api` |

The MVC app calls the API at `http://rest-api-service:8001`.

**Defaults**

- The namespace enforces the [restricted Pod Security Standard](https://kubernetes.io/docs/concepts/security/pod-security-standards/#restricted). Pods run as non-root (UID 1654) with a read-only root filesystem, no privilege escalation, all capabilities dropped, and the `RuntimeDefault` seccomp profile.
- Readiness and liveness probes (`/` for the MVC app, `/Demo` for the API), plus CPU and memory requests and limits.

## 2. Create a Cluster

Amazon EKS (in [AWS CloudShell](https://docs.aws.amazon.com/cloudshell/latest/userguide/welcome.html) or with [eksctl](https://eksctl.io/installation/) and [kubectl](https://kubernetes.io/docs/tasks/tools/) installed)

```shell
eksctl create cluster --name <name> --region <region>
```

Or provision EKS with Terraform: see [bstraehle/terraform](https://github.com/bstraehle/terraform).

Azure AKS (in [Azure Cloud Shell](https://learn.microsoft.com/azure/cloud-shell/overview))

```shell
az group create --name <resource-group> --location <location>
az aks create --resource-group <resource-group> --name <name> --node-count 2 --generate-ssh-keys
az aks get-credentials --resource-group <resource-group> --name <name>
```

Google GKE (in [Cloud Shell](https://cloud.google.com/shell/docs), with the Kubernetes Engine API enabled)

```shell
gcloud container clusters create-auto <name> --region <region> --project <project>
gcloud container clusters get-credentials <name> --region <region> --project <project>
```

Local kind cluster (with [Docker](https://docs.docker.com/get-started/get-docker/) and [kind](https://kind.sigs.k8s.io/docs/user/quick-start/#installation))

```shell
kind create cluster --name demo
```

> Note: On hosts that still use cgroup v1 (e.g. some WSL 2 setups), recent kubelets refuse to start. Create the cluster with a [kind config](https://kind.sigs.k8s.io/docs/user/configuration/#kubeadm-config-patches) that patches `KubeletConfiguration` with `failCgroupV1: false`, or switch the host to cgroup v2.

## 3. Deploy

```shell
git clone https://github.com/bstraehle/kubernetes.git
cd kubernetes

kubectl apply -k manifests
kubectl -n demo rollout status deployment/rest-api
kubectl -n demo rollout status deployment/mvc-app
kubectl -n demo get deployments,pods,services
```

To deploy other image tags, e.g. a pinned version, change `newTag` under `images` in [`manifests/kustomization.yaml`](manifests/kustomization.yaml), or use the standalone [kustomize](https://kubectl.docs.kubernetes.io/installation/kustomize/) CLI:

```shell
cd manifests
kustomize edit set image bstraehle/mvc-app=bstraehle/mvc-app:<tag>
```

## 4. Access the App

On EKS, AKS, and GKE, `mvc-app-service` gets a cloud load balancer:

```shell
kubectl -n demo get service mvc-app-service
```

Open `http://<EXTERNAL-IP>:8000` and choose **Demo** to list data from the API. On EKS, `EXTERNAL-IP` is a hostname.

On kind or any cluster without a load balancer:

```shell
kubectl -n demo port-forward service/mvc-app-service 8000:8000
```

Then open http://localhost:8000.

## 5. Clean Up

```shell
kubectl delete -k manifests
```

Then delete the cluster so you stop paying for it, along with any load balancer it created:

```shell
eksctl delete cluster --name <name> --region <region>
az group delete --name <resource-group>
gcloud container clusters delete <name> --region <region> --project <project>
kind delete cluster --name demo
```

## 6. CI

[`.github/workflows/ci.yml`](.github/workflows/ci.yml) validates the rendered manifests with [kubeconform](https://github.com/yannh/kubeconform). It then deploys them to a kind cluster and checks the endpoints, including `/Demo/List`, which goes from the MVC app through to the API. It runs on every push and pull request, and weekly to catch breaking changes in the `:latest` images.

## 7. Resources

- Next step: [Istio service mesh](https://github.com/bstraehle/istio) on top of these manifests
- Website: [Microservices Architecture](https://microservices.io/)
- Book: [Microservices Patterns](https://microservices.io/book)
- Video (25 min): [Kubernetes Documentary - Part 1/2](https://www.youtube.com/watch?v=BE77h7dmoQU)
- Video (32 min): [Kubernetes Documentary - Part 2/2](https://www.youtube.com/watch?v=318elIq37PE)
- Blog Post: [Docker Anti-Patterns](https://codefresh.io/containers/docker-anti-patterns/)
- Blog Post: [10 Anti-Patterns for Kubernetes Deployments](https://betterprogramming.pub/10-antipatterns-for-kubernetes-deployments-e97ce1199f2d)
- Checklist: [Kubernetes Production Best Practices](https://learnk8s.io/production-best-practices)
- Free eBook (information required): [O'Reilly: Kubernetes Patterns for Designing Cloud-Native Apps](https://library.devops.com/oreilly-kubernetes-patterns-for-designing-cloud-native-apps)
- Free eBook (information required): [O'Reilly: Kubernetes Up & Running](https://tanzu.vmware.com/content/ebooks/kubernetes-up-running-dive-into-the-future-of-infrastructure)
- Free eBook (information required): [Zero Trust Container Security for Dummies](https://more.suse.com/zero-trust-security-for-dummies.html)
- Free eBook (information required): [NGINX Library (Multiple)](https://www.nginx.com/resources/library)
