#Technologies 

[Video](https://www.youtube.com/watch?v=s_o8dwzRlu4)

---
## # Kubernates ?

Open-source container orchestration tool.
Developed by Google.
Kubernetes helps in managing containers.

### # Need for a Tool like this

Trend from Monolith to Microservice architecture.
- Monolith = a big, closed application
- Microservices = multiple independand services, that communicate with each other

![[Pasted image 20241129141018.png]]

| + Feature         | Explanation                 |
| ----------------- | --------------------------- |
| High Availability | Application has no downtime |
| Scalability       | Users are balanced          |
| Recovery          | Backups & restors           |

---
## # Architecture / Cluster

1. Control Plane Node - processes to manage the cluster properly
	1. Process: API-Server - Entrypoint to K8s cluster
	2. Process: Controller-Manager - what's happening in cluster?
	3. Process: Scheduler - ensures Pods placement
	4. Process: etcd - holds state of K8s cluster = can back up & restore
2. Virtual Network
	1. allows a communication between Master-Node & Worker Nodes
3. Worker Node - applications are running
	1. Kubelet-Process - for communicating with other worker nodes

![[Kubernetes_Definition 2024-11-29 14.23.34.excalidraw]]

![[Pasted image 20241129151406.png]]

### # Load-difference

Worker Nodes
- higher workload
- much bigger & more resources

Control Plane Nodes
- higher importance = backup
- not big/many resources

---
## # Main Components

Example: Small database-application

### # Node

- Server - physical/virtual machine
- consists of pods

### # Pod

- the smallest unit in K8s
- managed by Control Plane
- in a pod are containers
- usually 1 container per pod
- each pod - has own IP-address
	- Note: Pods die easily - get assigned new IP-address

### # Service

- is a permanent IP-address for each Pod
- if a Pod dies - service + IP-address stay

**External Service** - open service from external sources
- Example: access through internet
- BUT: DataBase should relief as **Internal Service**

### # Ingress

- First step of request - Ingress, then Service

---
## # Other Components

### # ConfigMap

### # Secret

### # Volume

Storage on local machine.

---


---
## # Demo-Project

mongo-config.yaml:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mongo-config
data:
  mongo-url: mongo-service
```

Secret:

```yaml

```

Deployment - MongoDB:

```yaml

```

Deployment - WebApp:

```yaml

```


Terminal:

```bash
kubectl apply -f mongo-config.yaml 
kubectl apply -f mongo-secret.yaml
kubectl apply -f mongo.yaml 
kubectl apply -f webapp.yaml 

kubectl get all

kubectl --help
kubectl get --help
kubectl describe service ...

kubectl get svc
minikube ip
```

