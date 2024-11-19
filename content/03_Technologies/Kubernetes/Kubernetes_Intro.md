#Technologies 

---
## Container challenges

Manual deployment of containers is hard to maintain, error-prone and annoying.

- Containers might crash / go down and need to be replaced. (health-cheks + redeployment)
- We might need more container instances upon traffic spikes. (auto-scaling)
- Incoming traffic should be distributed equally. (load-balancing)

## Cloud services can help

There are cloud services you can use to deploy your docker containers to that will take care of some or all of the challenges above without the need to use kubernetes.

**But that locks us in!**

- Using a specific cloud service locks us in that service. (If you only ever want to use that specific service, that might be fine.)
- You need to learn about the specifics, services and config options of another provider if you want to switch.
- Just knowing Docker isn’t enough.

## Solution: Kubernetes

Kubernetes (or short k8s) is an open-source system (and de-facto standard) for orchestrating container deployments.

- Automatic Deployment
- Scaling & Load Balancing
- Management

Kubernetes is like docker-compose for multiple machines.

## Why Kubernetes?

![WhyKubernetes.excalidraw.svg](https://deep-thought.norwin.at/_astro/whykubernetesexcalidraw.BMYrdfgP_Z12MrOB.svg)

## Extensible, Yet Stardardized Configuration

```
apiVersion: v1kind: Servicemetadata:  name: auth-service  annotations:    service.beta.kubernetes.io/aws-load-balancer-access-log-enabled: "true"spec:  selector:    app: auth-appports:  - protocol: TCP    port: 80    targetPort: 8080type: LoadBalancer...
```

- Standardized way of describing the to-be-created and to-be-managed resources of the k8s cluster.
- Cloud-provider-specific settings can be added (like aws-load-balancer-access-log)

## What Kubernetes IS and IS NOT

Kubernetes …

|IS NOT|INSTEAD|
|---|---|
|a cloud service provider|is an open-source project|
|a service by a cloud service provider|can be used with any provider|
|restricted to any specific (cloud) service provider|can be used with any provider|
|just a software you run on some machine|is a collection of concepts and tools|
|an alternative to Docker|works with (Docker) containers|
|a paid service|is a free open-source project|

## Kubernetes Architecture

![KubernetesArchitecture.excalidraw.svg](https://deep-thought.norwin.at/_astro/kubernetesarchitectureexcalidraw._RDR2tHL_Z49QAp.svg)

- The Master Node controls your deployment (i.e. all Worker Nodes)
- Worker Nodes run the containers of your application
- Nodes are your machines / virtual instances
- Multiple Pods can be created and removed to scale your app.

## Worker Nodes

![KubernetesWorkerNodes.excalidraw.svg](https://deep-thought.norwin.at/_astro/kubernetesworkernodesexcalidraw.B922o6G7_Z2aN9Hi.svg)

- Worker Node
    - managed by the Master Node
    - think of it as one computer / machine / virtual instance
- kubelet
    - Communication between Master and Worker Node
- kube-proxy
    - Managed Node and Pod network communication
- Pod
    - Hosts one or more application containers and their resources (volumes, IP, run config)
    - Are created and managed by Kubernetes

## Master Node

![KubernetesMasterNode.excalidraw.svg](https://deep-thought.norwin.at/_astro/kubernetesmasternodeexcalidraw.rM8UpR5c_Z8gx2L.svg)

- API Server
    - API for the kubelets to communicate
- Scheduler
    - Watches for new Pods, selects Worker Nodes to run them on
- Kube-Controller-Manager
    - Watches and controls Worker Nodes, correct number of Pods & more
- Cloud-Controller-Manager
    - Like Kube-Controller-Manager BUT for a specific Cloud Provider
    - Knows how to interact with Cloud Provider resources

## Your Work / Kubernetes’ Work

|What Kubernetes Will Do|What You Need To Do|
|---|---|
|Create your objects (e.g. Pods) and manage them|Create the Cluster and the Node Instances (Worker + Master Nodes)|
|Monitor Pods and re-create them, scale Pods etc.|Setup API Server, kubelet and other Kubernetes services / software on Nodes|
|Kubernetes utilizes the provided (cloud) resources to apply your configuration / goals|Create other (cloud) provider resources that might be needed (e.g. Load Balancer, Filesystems)|

## Core Components

- Cluster
    - A set of **Node** machines which are running the **Containerized** Application (**WorkerNodes**) or control other nodes (**Master Node**)
- Nodes
    - **Physical or virtual machine** with a certain hardware capacity which hosts **one or multiple Pods** and **communicatios** with the Cluster.
- Master Node
    - Cluster **Control Plane**, **managing the Pods** across Worker Nodes
- Worker Node
    - Hosts Pods, **running App Containers** (+ **resources**)
- Pods
    - Pods **hold the actual running App Containers** + their **required resources** (e.g. volumes).
- Containers
    - Normal (Docker) Containers
- Services
    - A **logical set (group) of Pods** with a unique, Pod- and Container-**independent IP address**

## Local Development Installation

Install minikube: [https://minikube.sigs.k8s.io/docs/start/](https://minikube.sigs.k8s.io/docs/start/)

After install:

Terminal window

```
>> kubectl version --clientClient Version: v1.29.3Kustomize Version: v5.0.4-0.20230601165947-6ce0bf390ce3
```

Terminal window

```
>> minikube start😄  minikube v1.32.0 on Darwin 14.4.1 (arm64)✨  Automatically selected the docker driver📌  Using Docker Desktop driver with root privileges👍  Starting control plane node minikube in cluster minikube🚜  Pulling base image ...    > gcr.io/k8s-minikube/kicbase...:  410.58 MiB / 410.58 MiB  100.00% 13.15 M🔥  Creating docker container (CPUs=2, Memory=4000MB) ...🐳  Preparing Kubernetes v1.28.3 on Docker 24.0.7 ...    ▪ Generating certificates and keys ...    ▪ Booting up control plane ...    ▪ Configuring RBAC rules ...🔗  Configuring bridge CNI (Container Networking Interface) ...🔎  Verifying Kubernetes components...    ▪ Using image gcr.io/k8s-minikube/storage-provisioner:v5🌟  Enabled addons: storage-provisioner, default-storageclass🏄  Done! kubectl is now configured to use "minikube" cluster and "default" namespace by default
```

You now have a local cluster which you can control using the kubectl command-line tool. You can apply configurations to your cluster to test things out locally. The same configuration will then also work for remote clusters (e.g. Azure, AWS, …).

See your status:

Terminal window

```
>> minikube statusminikubetype: Control Planehost: Runningkubelet: Runningapiserver: Runningkubeconfig: Configured
```

You can use a web-dashboard by running `minikube dashboard`. (hit Ctrl+C to quit again when finished).

Terminal window

```
>> minikube dashboard🔌  Enabling dashboard ...    ▪ Using image docker.io/kubernetesui/dashboard:v2.7.0    ▪ Using image docker.io/kubernetesui/metrics-scraper:v1.0.8💡  Some dashboard features require the metrics-server addon. To enable all features please run:
  minikube addons enable metrics-server

🤔  Verifying dashboard health ...🚀  Launching proxy ...🤔  Verifying proxy health ...🎉  Opening http://127.0.0.1:60394/api/v1/namespaces/kubernetes-dashboard/services/http:kubernetes-dashboard:/proxy/ in your default browser...
```

## Understanding Kubernetes Objects

Kubernetes works with …

- Objects
    - Pods
    - Deployments
    - Services
    - Volume
    - …

Objects can be created in two ways:

- Imperatively
- Declaratively

## The Pod Object

- The smallest unit Kubernetes interacts with
- Contains and runs one or multiple containers
    - Most common use-case is “one container per Pod”
- Pods contain shared resources (e.g. volumes) for all Pod containers
- Has a cluster-internal IP by default
    - Containers inside a Pod can communicate via localhost
- Pods are designed to be ephemeral: Kubernetes will start, stop and replace them as needed.
- For Pods to be managed for you, you need a “Controller” (e.g. a “Deployment”)

## The “Deployment” Object

- Controls (multiple) Pods
- You set a desired state, Kubernetes then changes the actual state
    - Define which Pods and containers to run and the number of instances
- Deployments can be paused, deleted and rolled back
- Deployments can be scaled dynamically (and automatically)
    - You can change the number of desired Pods as needed.
- Deployments manage a Pod for you, you can also create multiple Deployments
- You therefore typically don’t directly control Pods, instead you use Deployments to set up the desired end state.

## The Service Object

- Exposes Pods to the Cluster or Externally
- Pods have an internal IP by default - it changes when a Pod is replaced
    - Finding Pods is hard if the IP changes all the time
- Services group Pods with a shared IP
- Services can allow external access to Pods
    - The default (internal only) can be overwritten
- Without Services, Pods are very hard to reach and communication is difficult
- Reaching a Pod from outside the Cluster is not possible at all without Services

## Deployment flow

![KubernetesDeploymentFlow.excalidraw.svg](https://deep-thought.norwin.at/_astro/kubernetesdeploymentflowexcalidraw.BUmssQDT_ZJyhYV.svg)

## Imperative vs Declarative Usage

|Imperative|Declarative|
|---|---|
|`kubectl create deployment ...`|`kubectl apply -f config.yaml`|
|Individual commands are executed to trigger certain Kubernetes actions|A config file is defined and applied to change the desired state|
|Comparable to using `docker run` only|Comparable to using `docker-compose` with compose files|

## A Resource Definition

```
apiVersion: apps/v1kind: Deploymentmetadata:  name: first-appspec:  selector:    matchLabels:      app: first-dummy  replicas: 1  template:    metadata:      labels:        app: first-dummy    spec:      containers:        - name: first-node          image: "some-image"
```

## A first imperative deployment

app.js

```
const express = require('express');
const app = express();
app.get('/', (req, res) => {  res.send(`    <h1>Hello from this NodeJS app!</h1>    <p>Try sending a request to /error and see what happens</p>  `);});
app.get('/error', (req, res) => {  process.exit(1);});
app.listen(8080);
```

package.json

```
{  "name": "first-run",  "version": "1.0.0",  "description": "",  "main": "index.js",  "scripts": {    "test": "echo \"Error: no test specified\" && exit 1"  },  "license": "ISC",  "dependencies": {    "express": "^4.17.1",    "body-parser": "^1.19.0"  }}
```

Dockerfile

```
FROM node:14-alpine
WORKDIR /app
COPY package.json .
RUN npm install
COPY . .
EXPOSE 8080
CMD [ "node", "app.js" ]
```

- Build an image
- `docker build -t kub-first-app .`
- Make sure cluster is running
- `minikube status`
- See list of available kubectl commands
- `kubectl help`
- Create deployment object
- `kubectl create deployment first-app --image=kub-first-app`
- Get list of created deployments and pods
- `kubectl get deployments`
- `kubectl get pods`
- This should not have worked. Images need to be pulled from registry by Kubernetes. You need to push your image first to docker hub, or use the trueberryless/kub-first-app image.
- First delete the previous deployment
- `kubectl delete deployment first-app`
- then create the new deployment
- `kubectl create deployment first-app --image=trueberryless/kub-first-app`
- Expose a port
- `kubectl expose deployment first-app --type=LoadBalancer --port=8080`
- See services
- `kubectl get services`
- On a cloud infrastructure we would get an external IP. For minikube we need:
- `minikube service first-app`
- You can crash the app by going to the `/error` route. You will see the pod gets restarted automatically (taking longer and longer each time).
- You can also scale your deployment
- `kubectl scale deployment/first-app --replicas=3`
- Have a look at your pods again
- `kubectl get pods`
- You can update your deployment by running. Images will only be pulled if they have a different tag.
- `kubectl set image deployment/first-app kub-first-app=trueberryless/kub-first-app:v2`
- view the deployment status
- `kubectl rollout status deployment/first-app`
- Run a deployment with a non-existing image/tag.
- `kubectl set image deployment/first-app kub-first-app=trueberryless/kub-first-app:not-exist`
- Checking the status of your rollout you will see, the old pods won’t be terminated because the new ones can’t be started due to an image pulling error.
- Rollout to last deployment
- `kubectl rollout undo deployment/first-app`
- See deployment history
- `kubectl rollout history deployment/first-app`
- `kubectl rollout history deployment/first-app --revision=3`
- You can also rollback to a specific revision
- `kubectl rollout undo deployment/first-app --to-revision=1`