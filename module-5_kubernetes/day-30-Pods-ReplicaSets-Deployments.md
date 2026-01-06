# Day 30	Pods, ReplicaSets, Deployments

Pods, ReplicaSets, Deployments" is one of the most important topic in Kubernetes. I’ll explain it step by step with a hands-on demo.

### 📌 Explanation with Hands-On Example
### 1. Pods
 - A Pod is the smallest deployable unit in Kubernetes.
 - It can run one or more containers.
 - Think of it as a wrapper around containers.
👉 Example Pod YAML (pod.yaml):
```sh
apiVersion: v1
kind: Pod
metadata:
  name: nginx
  labels:
    app: proxy
spec:
  containers:
  - name: nginx
    image: nginx:stable
    ports:
      - containerPort: 80
        name: http-web-svc

```
Run:
```sh
kubectl apply -f pod.yaml
kubectl get pods
kubectl describe pod mypod
```

### 2. Service

A Kubernetes Service is an abstraction that defines a stable network endpoint (a permanent IP address and DNS name) and a policy to access a logical set of Pods. It provides a consistent way for applications to communicate, abstracting away the ephemeral nature of individual Pods (which can be created, destroyed, or rescheduled at any time, changing their IP addresses).
Services use labels and selectors to automatically route traffic to the appropriate Pods, acting as an internal load balancer across the healthy Pods that match the defined criteria.

Key Functions:
- Service Discovery: Services are assigned a stable DNS name and IP address. Other Pods or external clients can use this stable name to discover and communicate with the application without needing to track individual Pod IPs.
- Load Balancing: When multiple Pods are associated with a Service, the Service automatically distributes incoming requests among them, ensuring even load distribution and high availability.
- Decoupling: Services decouple the application logic (how Pods are accessed) from the underlying infrastructure (where Pods are running), making the system more resilient to changes.

👉 Example Service YAML (service.yaml):
```sh
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: proxy
  ports:
  - name: name-of-service-port
    protocol: TCP
    port: 80
    nodePort: 30091
  type: NodePort
```

### 3. ReplicaSets
 - Ensures a specified number of Pods are always running.
 - If a Pod crashes, ReplicaSet creates a new one automatically.
👉 Example ReplicaSet YAML (replicaset.yaml):

```sh
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx
  labels:
    app: proxy
spec:
  replicas: 2
  selector:
    matchLabels:
      app: proxy
  template:
    metadata:
      labels:
        app: proxy
    spec:
      containers:
      - name: nginx
        image: nginx:stable
        ports:
          - containerPort: 80
            name: http-web-svc
```
Run:
```sh
kubectl apply -f replicaset.yaml
kubectl get rs
kubectl get pods
```
👉 You’ll see 2 Pods running.

### 4. Deployments
 - A Deployment manages ReplicaSets.
 - Provides rolling updates, rollbacks, and scaling.
 - This is what we usually use in production.
👉 Example Deployment YAML (deployment.yaml)
```sh
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx
  labels:
    app: proxy
spec:
  replicas: 2
  selector:
    matchLabels:
      app: proxy
  template:
    metadata:
      labels:
        app: proxy
    spec:
      containers:
      - name: nginx
        image: nginx:stable
        ports:
          - containerPort: 80
            name: http-web-svc
```
Run:
```sh
kubectl apply -f deployment.yaml
kubectl get deployments
kubectl get pods
```
👉 Update Deployment:
```sh
kubectl set image deployment/mydeployment nginx-container=nginx:1.23
kubectl rollout status deployment/mydeployment
```
👉 Rollback:
```sh
kubectl rollout undo deployment/mydeployment
```
