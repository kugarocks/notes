---
title: "Architecture"
description: ""
summary: ""
date: 2024-10-13T22:00:00+08:00
lastmod: 2024-10-13T22:00:00+08:00
weight: 300
seo:
  title: "Kubernetes Architecture"
  description: ""
  canonical: ""
  noindex: false
---

## Node Processes

### Worker Nodes

* Each Node has multiple Pods on it.
* Worker Nodes do the actual work.
* Need more resources(CPU/Memory).
* 3 processes must be installed on each Node:
  * Container Runtime
    * e.g. Docker/Podman
  * kubelet
    * Communicate with Master Node.
    * Do the actual work
    * Scheduling Pods
  * kube-proxy
    * Network proxy/rules

### Master Node

* Cluster Interface.
* Less resources than Worker Nodes.
* 4 processes must be installed on each Node:
  * Api Server
    * Cluster gateway
    * Gatekeeper for authentication
    * Client: UI/API/CLI
  * Scheduler
    * Scheduling pods by resource availability
  * Controller Manager
    * Maintains the state of the cluster.
    * Detects and responds to node failure.
    * Request kubelet to restart failed Pods.
  * Etcd
    * Distributed key-value store.
    * Store the state of the cluster.

## Kubectl

* Command line tool for Kubernetes.

### Get Version

kubectl client version and k8s server version.

```bash {frame="none"}
kubectl version
```

### Get Nodes

```bash {frame="none"}
kubectl get nodes
```

```txt {frame="none"}
NAME      STATUS    ROLES   AGE   VERSION
node1     Ready     master   10d   v1.20.0
node2     Ready     worker   10d   v1.20.0
```

## Basic Kubectl Commands

Create and debug Pods in Kubernetes.

### Create Deployment

```bash {frame="none"}
kubectl create deployment my-nginx --image=nginx [--dry-run]
```

### Get Deployment

```bash {frame="none"}
kubectl get deployments
```

```txt {frame="none"}
NAME         READY   UP-TO-DATE   AVAILABLE   AGE
my-nginx     1/1     1            1           10s
```

### Get Pods

```bash {frame="none"}
kubectl get pods
```

```txt {frame="none"}
NAME                     READY   STATUS    RESTARTS   AGE
my-nginx-6d4b94b94-2s222   1/1   Running   0          10s
```

### Get ReplicaSet

Manage by Deployment.

```bash {frame="none"}
kubectl get replicasets
```

```txt {frame="none"}
NAME                   DESIRED   CURRENT   READY   AGE
my-nginx-6d4b94b94     1         1         1       10s
```

Pod name include 3 parts:

* `my-nginx` is Deployment name.
* `6d4b94b94` is ReplicaSet ID.
* `2s222` is Pod ID.

Layers of Abstraction:

* Deployment manages a ReplicaSet.
* ReplicaSet manages Pods.
* Pod is an abstraction of a container.
* Container is runtime of an image.

### Edit Deployment

Change the image version to `nginx:1.21`.

```bash {frame="none"}
kubectl edit deployment my-nginx
```

* The old pod will be terminated and a new pod will be created.
* The replicaset will have two versions.

### Pod Logs

```bash {frame="none"}
kubectl logs my-nginx-6d4b94b94-2s222
```

### Pod State

Get pod status/events.

```bash {frame="none"}
kubectl describe pod my-nginx-6d4b94b94-2s222
```

| Event     | Age  | From                | Message                |
|-----------|------|---------------------|------------------------|
| Scheduled | 10s  | default-scheduler   | Assigned to a node.    |
| Pulling   | 5s   | kubelet,minikube    | Pulling image.         |
| Pulled    | 5s   | kubelet,minikube    | Pulled image.          |
| Created   | 3s   | kubelet,minikube    | Created container.     |
| Started   | 2s   | kubelet,minikube    | Started container.     |

### Enter Pod

```bash {frame="none"}
kubectl exec -it my-nginx-6d4b94b94-2s222 -- /bin/bash
```

### Delete Deployment

```bash {frame="none"}
kubectl delete deployment my-nginx
```

### Apply Deployment

```bash {frame="none"}
kubectl apply -f nginx-deployment.yaml
```

```bash {frame="none"}
kubectl delete -f nginx-deployment.yaml
```

## Configuration File

* 3 Parts of configration file
* Connecting Deployments to Service to Pods
* Demo

```yaml {frame="none"}
apiVersion: apps/v1
kind: Deployment
metadata:
  name: my-nginx
  labels:
    app: nginx
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
          image: nginx:1.21
          ports:
            - containerPort: 80
```

### 3 Parts of configration file

* Metadata
* Specification
* Status

Status is automatically generated by Kubernetes.

* Compoare Desired State(Spec) and Actual State(Etcd)
* Rollout new changes

## Template(Blueprint for Pods)

* Template is used to create Pods
* Has own metadta and spec
  * label/name/image/port

## Connecting Components

Labels & Selectors & Ports

