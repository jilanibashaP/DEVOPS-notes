# 🚀 Kubernetes Part 1 — Complete Revision Notes
> **Background assumed:** You know Docker & manual deployment on EC2/VMs

---

## 📌 Table of Contents
1. [The Problem with Manual Deployment](#1-the-problem-with-manual-deployment)
2. [What is Kubernetes?](#2-what-is-kubernetes)
3. [Core Architecture](#3-core-architecture)
4. [Key Vocabulary](#4-key-vocabulary)
5. [Setting Up a Cluster](#5-setting-up-a-cluster)
6. [Pods](#6-pods)
7. [ReplicaSet](#7-replicaset)
8. [Deployment](#8-deployment)
9. [Why Deployment over ReplicaSet?](#9-why-deployment-over-replicaset)
10. [Services — Exposing Your App](#10-services--exposing-your-app)
11. [Full End-to-End Example](#11-full-end-to-end-example)
12. [Quick Command Reference](#12-quick-command-reference)

---

## 1. The Problem with Manual Deployment

You already know this from experience:

```
WITHOUT KUBERNETES (what you know):
─────────────────────────────────────────────────────────

  User Request
      │
      ▼
  Load Balancer (you set this up manually on AWS)
      │
  ┌───┴───┐
  ▼       ▼
EC2-1   EC2-2      ← you SSH into each, run docker run, manage crashes manually
(BE)    (BE)

EC2-3              ← separate machine for frontend
(FE)
```

**The problems you face:**
- App crashes at 3am → you have to SSH and restart manually
- Traffic spikes → you manually spin up more EC2s
- Deploying new version → downtime or complex bash scripts
- Watching logs across 5 machines → painful

**Kubernetes solves ALL of this.**

---

## 2. What is Kubernetes?

> **Simple definition:** Kubernetes is a system that manages your Docker containers automatically — starting them, healing them, scaling them, and exposing them.

```
WITH KUBERNETES:
─────────────────────────────────────────────────────────

  User Request
      │
      ▼
┌─────────────────────────────────┐
│      Kubernetes Cluster         │
│                                 │
│  ┌──────────┐  ┌──────────┐    │
│  │  Worker  │  │  Worker  │    │
│  │  Node 1  │  │  Node 2  │    │
│  │ [BE Pod] │  │ [BE Pod] │    │
│  │ [BE Pod] │  │ [FE Pod] │    │
│  └──────────┘  └──────────┘    │
│                                 │
│  ┌──────────┐                   │
│  │  Master  │ ← YOU talk here  │
│  │  Node    │   via kubectl     │
│  └──────────┘                   │
└─────────────────────────────────┘
```

**You tell K8s:** "I want 3 nginx containers running always"  
**K8s handles:** where they run, restarting if they crash, updating them

---

## 3. Core Architecture

### The Big Picture

```
┌──────────────────────────────────────────────────────────────┐
│                    Kubernetes Cluster                        │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │              MASTER NODE (Control Plane)            │    │
│  │                                                     │    │
│  │  ┌──────────┐  ┌──────┐  ┌───────────┐  ┌──────┐  │    │
│  │  │API Server│  │ etcd │  │ Scheduler │  │Ctrl  │  │    │
│  │  │(gateway) │  │(DB)  │  │(placement)│  │Mgr   │  │    │
│  │  └──────────┘  └──────┘  └───────────┘  └──────┘  │    │
│  └─────────────────────────────────────────────────────┘    │
│           ▲                                                  │
│           │ kubectl sends commands here                      │
│           │                                                  │
│  ┌──────────────────┐    ┌──────────────────┐               │
│  │   WORKER NODE 1  │    │   WORKER NODE 2  │               │
│  │                  │    │                  │               │
│  │ ┌──────────────┐ │    │ ┌──────────────┐ │               │
│  │ │   kubelet    │ │    │ │   kubelet    │ │               │
│  │ │(node agent)  │ │    │ │(node agent)  │ │               │
│  │ └──────────────┘ │    │ └──────────────┘ │               │
│  │ ┌────┐ ┌────┐    │    │ ┌────┐           │               │
│  │ │Pod │ │Pod │    │    │ │Pod │           │               │
│  │ └────┘ └────┘    │    │ └────┘           │               │
│  └──────────────────┘    └──────────────────┘               │
└──────────────────────────────────────────────────────────────┘
```

### Master Node Components

| Component | What It Does | Analogy |
|-----------|-------------|---------|
| **API Server** | Entry point for all commands. Handles auth. | Reception desk |
| **etcd** | Key-value DB storing cluster state | The cluster's memory |
| **kube-scheduler** | Decides which worker node runs a new pod | HR assigning employees to offices |
| **kube-controller-manager** | Watches and maintains desired state | Manager who notices issues |

### Worker Node Components

| Component | What It Does | Analogy |
|-----------|-------------|---------|
| **kubelet** | Agent that runs on each node, manages pods | On-site supervisor |
| **kube-proxy** | Handles network routing to pods | Office receptionist/switchboard |
| **Container Runtime** | Actually runs containers (containerd/docker) | The actual worker |

---

## 4. Key Vocabulary

> Think of it as levels of abstraction, just like you have in Docker:

```
Docker World          Kubernetes World
─────────────────     ──────────────────────────
Dockerfile        →   (same)
Docker Image      →   Image (pulled from DockerHub)
docker run nginx  →   Pod (running instance of image)
docker-compose    →   Deployment (manages multiple pods)
Multiple VMs      →   Cluster (group of nodes/machines)
```

### Full Hierarchy

```
CLUSTER
└── NODES (machines)
    ├── Master Node (brain)
    └── Worker Nodes (muscles)
        └── PODS (smallest unit)
            └── CONTAINERS (your actual app)
                └── IMAGE (blueprint)
```

### Explained Simply

**Image** = your Docker image sitting on DockerHub. Blueprint. Not running.
```bash
# Example: the nginx image on DockerHub
https://hub.docker.com/_/nginx
```

**Container** = image that is running
```bash
docker run -p 3005:80 nginx
# ↑ this creates a container from the nginx image
```

**Pod** = Kubernetes wrapper around one or more containers
```bash
# In K8s, instead of docker run, you do:
kubectl run nginx --image=nginx --port=80
# This creates a POD containing a container running nginx
```

**Node** = A machine (EC2 or local VM) in your cluster

**Cluster** = All your nodes together

---

## 5. Setting Up a Cluster

### Option A: Local with `kind` (Kubernetes IN Docker)

> Kind runs Kubernetes nodes as Docker containers on your laptop. 

```bash
# Install kind first: https://kind.sigs.k8s.io/docs/user/quick-start/#installation

# Single node cluster (not ideal)
kind create cluster --name local
docker ps   # you'll see 1 container: control-plane

# Delete it
kind delete cluster -n local
```

**Multi-node cluster (recommended):**

Create `clusters.yml`:
```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane    # the master/brain
  - role: worker           # worker 1
  - role: worker           # worker 2
```

```bash
kind create cluster --config clusters.yml --name local
docker ps
# You'll now see 3 containers: 1 control-plane + 2 workers
```

```
After running above:
─────────────────────────────────────────────────────
$ docker ps

CONTAINER ID   IMAGE                  COMMAND   NAMES
a1b2c3d4e5f6   kindest/node:v1.29.0   ...       local-control-plane
b2c3d4e5f6a1   kindest/node:v1.29.0   ...       local-worker
c3d4e5f6a1b2   kindest/node:v1.29.0   ...       local-worker2
─────────────────────────────────────────────────────
Each Docker container = one K8s node
```

### Option B: Local with `minikube`

```bash
minikube start        # starts a single-node cluster
docker ps             # one container running (single-node)
```

> ⚠️ Single node = control-plane also acts as worker. Not ideal for production but fine for learning.

### Install kubectl (the CLI tool)

```bash
# Install: https://kubernetes.io/docs/tasks/tools/#kubectl

kubectl get nodes     # list all nodes in your cluster
kubectl get pods      # list all running pods
```

---

## 6. Pods

### What is a Pod?

> A Pod is the **smallest deployable unit** in Kubernetes. It wraps one or more containers.

```
Analogy: 
  Docker  → you run containers directly
  K8s     → you never create containers directly
              you create PODS, and pods run containers

Pod
└── Container 1 (nginx)     ← usually just one container per pod
└── Container 2 (optional)  ← sometimes a "sidecar" container
```

### Creating a Pod

**Method 1: Command (quick & dirty)**
```bash
kubectl run nginx --image=nginx --port=80
```

**Method 2: Manifest YAML (correct way)**

Create `pod.yml`:
```yaml
apiVersion: v1          # K8s API version
kind: Pod               # type of resource
metadata:
  name: nginx           # name of this pod
spec:
  containers:
    - name: nginx       # name of container inside pod
      image: nginx      # Docker image to use
      ports:
        - containerPort: 80   # port the container listens on
```

```bash
kubectl apply -f pod.yml       # create/update
kubectl get pods               # check status
kubectl logs nginx             # see logs
kubectl describe pod nginx     # detailed info
kubectl delete pod nginx       # delete it
```

### What happens when you create a pod?

```
YOU
 │
 │  kubectl apply -f pod.yml
 ▼
API Server (validates your request)
 │
 ▼
etcd (stores desired state: "I want 1 nginx pod")
 │
 ▼
Scheduler (picks a worker node for this pod)
 │
 ▼
kubelet on Worker Node (pulls nginx image, starts container)
 │
 ▼
🟢 Pod is Running
```

### Checking Pod Status

```bash
kubectl get pods
# NAME    READY   STATUS    RESTARTS   AGE
# nginx   1/1     Running   0          30s
#         ↑ 1 container ready out of 1 total

kubectl get pods -owide
# Shows which NODE the pod is running on + its private IP
# NAME    READY   STATUS    IP           NODE
# nginx   1/1     Running   10.244.2.3   local-worker
```

> ⚠️ The IP shown is a **private IP**. You can't access it from your browser directly. That's what Services are for (covered later).

---

## 7. ReplicaSet

### The Problem with Single Pods

```
Without ReplicaSet:
─────────────────────────────────────────────
  Pod crashes
      │
      ▼
  App is DOWN ❌
  You get paged at 3am 😭
  You SSH in, restart manually
─────────────────────────────────────────────
```

### What is a ReplicaSet?

> A ReplicaSet ensures that a **specified number of pod replicas are always running**. If one dies, it creates a new one automatically.

```
With ReplicaSet (replicas: 3):
─────────────────────────────────────────────
  Pod crashes
      │
      ▼
  ReplicaSet Controller notices: "I have 2 pods, I need 3"
      │
      ▼
  Creates new pod automatically ✅
  You sleep peacefully 😴
─────────────────────────────────────────────
```

### Creating a ReplicaSet

Create `rs.yml`:
```yaml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-replicaset
spec:
  replicas: 3                    # I want 3 pods always running
  selector:
    matchLabels:
      app: nginx                 # manage pods with this label
  template:                      # template for creating pods
    metadata:
      labels:
        app: nginx               # label applied to each pod
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80
```

```bash
kubectl apply -f rs.yml
kubectl get rs
# NAME               DESIRED   CURRENT   READY   AGE
# nginx-replicaset   3         3         3       23s

kubectl get pods
# NAME                       READY   STATUS    RESTARTS   AGE
# nginx-replicaset-7zp2v     1/1     Running   0          35s
# nginx-replicaset-q264f     1/1     Running   0          35s
# nginx-replicaset-vj42z     1/1     Running   0          35s
#        ↑ named as: replicaset-name + random-id
```

### Self-Healing Demo

```bash
# Delete one pod manually
kubectl delete pod nginx-replicaset-7zp2v

# Check immediately
kubectl get pods
# nginx-replicaset-q264f     1/1     Running   0    2m
# nginx-replicaset-vj42z     1/1     Running   0    2m
# nginx-replicaset-NEW123    1/1     Running   0    5s   ← NEW pod auto-created!
```

### Labels — How ReplicaSet Identifies "Its" Pods

```
ReplicaSet has: selector → matchLabels → app: nginx
                                               ↑
Pods have:      labels   → app: nginx   ←──────┘
                
If labels match → ReplicaSet manages that pod
```

**Interesting experiment:**
```bash
# Try adding a pod with app=nginx label manually
kubectl run extra-pod --image=nginx --labels="app=nginx"

# RS already has 3 pods → it will IMMEDIATELY TERMINATE your extra pod!
kubectl get pods   # you'll see extra-pod briefly then it's gone
```

---

## 8. Deployment

### What is a Deployment?

> A Deployment is a **higher-level abstraction that manages ReplicaSets**. When you update your app, Deployment handles the transition safely.

```
Hierarchy:
────────────────────────────────────────────
  YOU
   │  apply deployment.yml
   ▼
  DEPLOYMENT         ← manages updates, rollbacks
   │  creates
   ▼
  REPLICASET         ← manages number of pods
   │  creates
   ▼
  PODS               ← actual running containers
   │  runs
   ▼
  CONTAINERS         ← your app
────────────────────────────────────────────
```

### Creating a Deployment

Create `deployment.yml`:
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80
```

```bash
kubectl apply -f deployment.yml

kubectl get deployment
# NAME               READY   UP-TO-DATE   AVAILABLE   AGE
# nginx-deployment   3/3     3            3           18s

kubectl get rs
# NAME                          DESIRED   CURRENT   READY   AGE
# nginx-deployment-576c6b7b6    3         3         3       34s
#        ↑ deployment-name + hash (hash represents current config)

kubectl get pods
# NAME                                READY   STATUS    RESTARTS   AGE
# nginx-deployment-576c6b7b6-b6kgk    1/1     Running   0          46s
# nginx-deployment-576c6b7b6-m8ttl    1/1     Running   0          46s
# nginx-deployment-576c6b7b6-n9cx4    1/1     Running   0          46s
#        ↑ deployment-rs-hash + pod-random-id
```

### Passing Environment Variables

```yaml
spec:
  containers:
    - name: postgres
      image: postgres:latest
      ports:
        - containerPort: 5432
      env:
        - name: POSTGRES_PASSWORD    # env variable name
          value: "mysecretpassword"  # env variable value
```

This is equivalent to:
```bash
# Docker world:
docker run -e POSTGRES_PASSWORD=mysecretpassword postgres

# K8s world: put it in the YAML above
```

---

## 9. Why Deployment over ReplicaSet?

### The Key Reason: Safe Rollouts

**Experiment — bad image deployment:**

Update your `deployment.yml` to use a fake image:
```yaml
image: nginx2:latest   # this image doesn't exist!
```

```bash
kubectl apply -f deployment.yml
kubectl get rs
```

```
WHAT YOU'LL SEE:
────────────────────────────────────────────────────────────────
NAME                          DESIRED   CURRENT   READY   AGE
nginx-deployment-576c6b7b6    3         3         3       14m   ← OLD (still running! ✅)
nginx-deployment-5fbd4799cb   1         1         0       2m    ← NEW (failing ❌)

kubectl get pods:
nginx-deployment-576c6b7b6-9nlnq    1/1   Running          0    15m  ✅
nginx-deployment-576c6b7b6-m8ttl    1/1   Running          0    15m  ✅
nginx-deployment-576c6b7b6-n9cx4    1/1   Running          0    15m  ✅
nginx-deployment-5fbd4799cb-fmt4f   0/1   ImagePullBackOff 0    2m   ❌
────────────────────────────────────────────────────────────────
Your app is STILL RUNNING because Deployment kept the old RS alive!
With just a ReplicaSet, you'd have killed all 3 pods first. App = DOWN.
```

```
Rolling Update Strategy (Deployment handles this):
───────────────────────────────────────────────────────
  OLD VERSION: Pod1✅  Pod2✅  Pod3✅

  Step 1: Start new Pod → Pod1✅  Pod2✅  Pod3✅  NewPod🔄
  Step 2: New pod healthy? → Kill Old Pod1
          Result:           Pod2✅  Pod3✅  NewPod✅
  Step 3: Repeat until all pods updated
  
  If new pod FAILS: stop rollout, keep old pods running
───────────────────────────────────────────────────────
```

### Rollback

```bash
# Check rollout history
kubectl rollout history deployment/nginx-deployment
# REVISION  CHANGE-CAUSE
# 1         <none>     ← original nginx:latest
# 2         <none>     ← bad nginx2:latest

# Undo last deployment (go back to revision 1)
kubectl rollout undo deployment/nginx-deployment

# Your old 3 pods come back ✅
kubectl get pods
```

---

## 10. Services — Exposing Your App

### The Problem

```
After creating a deployment:

kubectl get pods -owide
NAME                                READY   STATUS    IP            NODE
nginx-deployment-576c6b7b6-7jrn5    1/1     Running   10.244.2.3    worker1
nginx-deployment-576c6b7b6-88fkh    1/1     Running   10.244.1.4    worker2
nginx-deployment-576c6b7b6-zf8ff    1/1     Running   10.244.2.5    worker1

The IPs (10.244.x.x) are PRIVATE → you can't hit them from your browser
```

### What is a Service?

> A Service is a **stable endpoint** that exposes your pods to the world. Pods can die and be recreated with new IPs — the Service IP stays the same.

```
Without Service:           With Service:
─────────────────          ──────────────────────────────
  Your Browser               Your Browser
       │                           │
       │ ??? which IP ???          │ hits Service (stable IP)
       ▼                           ▼
  Pod (10.244.2.3)          Service (fixed address)
                                   │
                              ┌────┴────┐
                              ▼         ▼
                         Pod 1      Pod 2      ← load balanced!
```

### 3 Types of Services

```
1. ClusterIP (default)
   ─────────────────────────────────────────────
   Only accessible INSIDE the cluster
   Use case: Pod-to-pod communication
   (e.g., your backend talking to postgres)

   Internet  ✗→  [Service: ClusterIP]  →  Pods
   
2. NodePort
   ─────────────────────────────────────────────
   Accessible from OUTSIDE via NodeIP:NodePort
   Port range: 30000-32767
   Use case: Development/testing

   Internet  →  Node:30007  →  [Service: NodePort]  →  Pods

3. LoadBalancer
   ─────────────────────────────────────────────
   Cloud provider creates a public load balancer
   Use case: Production on cloud (AWS, GCP, Vultr)

   Internet  →  [Cloud LB: 52.x.x.x]  →  [Service: LoadBalancer]  →  Pods
```

### NodePort Service (Local Development)

**Step 1:** Create `kind.yml` with port forwarding:
```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane
    extraPortMappings:
      - containerPort: 30007   # port inside cluster
        hostPort: 30007        # port on your laptop
  - role: worker
  - role: worker
```

```bash
kind create cluster --config kind.yml --name local
```

**Step 2:** Create `deployment.yml` (nginx with 3 replicas — same as before)

**Step 3:** Create `service.yml`:
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx          # targets pods with this label
  ports:
    - protocol: TCP
      port: 80          # service port (inside cluster)
      targetPort: 80    # pod's port
      nodePort: 30007   # external port (30000-32767)
  type: NodePort
```

```bash
kubectl apply -f deployment.yml
kubectl apply -f service.yml

# Now visit: http://localhost:30007 → you'll see nginx! 🎉
```

```
What happens when you hit localhost:30007:
──────────────────────────────────────────────────────────────
  Your Browser
      │  http://localhost:30007
      ▼
  Kind's port mapping (hostPort:30007 → containerPort:30007)
      │
      ▼
  Kubernetes Node
      │
      ▼
  NodePort Service (port 30007)
      │  picks one of the pods (round-robin)
  ┌───┴───┐
  ▼       ▼
Pod 1   Pod 2   Pod 3
──────────────────────────────────────────────────────────────
```

### LoadBalancer Service (Production on Cloud)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: LoadBalancer    # no nodePort needed — cloud handles it
```

```bash
kubectl apply -f service-lb.yml
kubectl get service
# NAME            TYPE           CLUSTER-IP    EXTERNAL-IP     PORT(S)
# nginx-service   LoadBalancer   10.96.1.100   52.14.xxx.xxx   80:31456/TCP
#                                              ↑ public IP assigned by cloud!
```

---

## 11. Full End-to-End Example

Here's everything together — deploy nginx and expose it:

### File Structure
```
k8s-demo/
├── kind.yml           # cluster config
├── deployment.yml     # your app pods
└── service.yml        # expose the app
```

### `kind.yml`
```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane
    extraPortMappings:
      - containerPort: 30007
        hostPort: 30007
  - role: worker
  - role: worker
```

### `deployment.yml`
```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
        - name: nginx
          image: nginx:latest
          ports:
            - containerPort: 80
```

### `service.yml`
```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
      nodePort: 30007
  type: NodePort
```

### Commands in Order
```bash
# 1. Create cluster
kind create cluster --config kind.yml --name local

# 2. Verify cluster
kubectl get nodes

# 3. Deploy app
kubectl apply -f deployment.yml

# 4. Check pods
kubectl get pods

# 5. Expose app
kubectl apply -f service.yml

# 6. Visit http://localhost:30007 in browser 🎉

# --- If you update the image ---
# 7. Edit deployment.yml (change image version)
kubectl apply -f deployment.yml

# 8. Watch rolling update
kubectl rollout status deployment/nginx-deployment

# --- If something breaks ---
# 9. Roll back
kubectl rollout undo deployment/nginx-deployment

# --- Cleanup ---
kubectl delete deployment nginx-deployment
kubectl delete service nginx-service
kind delete cluster -n local
```

---

## 12. Quick Command Reference

### Cluster
```bash
kind create cluster --name local                          # single node
kind create cluster --config clusters.yml --name local   # multi node
kind delete cluster -n local                             # delete cluster
kubectl get nodes                                        # list nodes
```

### Pods
```bash
kubectl run nginx --image=nginx --port=80   # create pod (imperative)
kubectl apply -f pod.yml                    # create pod (declarative)
kubectl get pods                            # list pods
kubectl get pods -owide                     # list pods with IPs and nodes
kubectl logs nginx                          # see logs
kubectl logs -f nginx                       # follow logs (live)
kubectl describe pod nginx                  # detailed info
kubectl delete pod nginx                    # delete pod
```

### ReplicaSet
```bash
kubectl apply -f rs.yml          # create replicaset
kubectl get rs                   # list replicasets
kubectl delete rs nginx-rs       # delete rs (also deletes pods)
```

### Deployment
```bash
kubectl apply -f deployment.yml                       # create/update deployment
kubectl get deployment                                # list deployments
kubectl rollout status deployment/nginx-deployment    # watch rollout progress
kubectl rollout history deployment/nginx-deployment   # see revision history
kubectl rollout undo deployment/nginx-deployment      # rollback
kubectl delete deployment nginx-deployment            # delete deployment
```

### Service
```bash
kubectl apply -f service.yml     # create service
kubectl get service              # list services (shows EXTERNAL-IP for LB)
kubectl delete service nginx-svc # delete service
```

### Debugging
```bash
kubectl describe pod <pod-name>     # events, errors, config
kubectl logs <pod-name>             # stdout logs
kubectl logs -f <pod-name>          # live logs
kubectl get events                  # cluster-wide events
kubectl get nodes --v=8             # see raw HTTP requests to API
```

---

## 🧠 Mental Model Summary

```
WHAT YOU WRITE        WHAT K8S CREATES           WHAT RUNS
────────────────      ─────────────────────      ──────────────
deployment.yml   →    Deployment                 
                       └── ReplicaSet            
                            ├── Pod 1      →     Container (nginx)
                            ├── Pod 2      →     Container (nginx)
                            └── Pod 3      →     Container (nginx)

service.yml      →    Service (NodePort/LB)
                       └── Routes traffic to Pods via labels
```

```
KEY RULES TO REMEMBER:
──────────────────────────────────────────────────────────────
✅ Always use Deployments (not raw Pods or ReplicaSets directly)
✅ Labels are how K8s connects things (Service → Pods, RS → Pods)
✅ Pods have private IPs — always use Services to expose them
✅ Deployment = rolling updates + rollback for free
✅ kubectl apply -f is idempotent — safe to run multiple times
✅ etcd = source of truth. K8s always tries to match actual state to desired state
──────────────────────────────────────────────────────────────
```

---

*Notes based on 100xDevs Kubernetes Part 1 — Page 2 of 20*
