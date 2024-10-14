---
title: "Introduction"
description: ""
summary: ""
date: 2024-10-13T17:00:00+08:00
lastmod: 2024-10-13T17:00:00+08:00
weight: 200
seo:
  title: "Kubernetes Introduction"
  description: ""
  canonical: ""
  noindex: false
---

## WHY K8S

In micro services, there are tons of container need to be host.
We can't do this by home make script and tools.
So we need a container Orchestration tool.
That is Kubernetes which offers the following features.

* High Availability or no downtime.
* Scalability or high performance.
* Disaster recovery - backup and restore.

## Components

* Pod
* Service
* Ingress
* Volumes
* StatefulSet
* Deployment
* ConfigMap
* Secrets

## Concepts

### Node

* Physical or virtual machine

### Pod

* Smallest unit of K8S.
* A layer of abstraction on containers.
* Pod can have many container(e.g. sidecar).
* No need to interact with specific container tech. (e.g. docker)
* Only interact with kubernetes layer.
* Each pod gets its own IP address.
* New IP address on re-creation.

### Container

* Isolated runtime environment.
* Tech: Docker/Podman/CRI-O/LXC.

### Service

* Permanent IP address (Endpoint).
* Pods communicate with each other using a service.
* Lifecycle of Pod and Service Not connected.
* Pod die, Service stay.
* Pod load balancer.

### Ingress

* Route traffic into cluster(Internal Service).

### ConfigMap

* External config of your application.

### Secret

* Used to store secret data.
* Base64 encoded.

### Volumes

* Data persistence of pod.
* Mount a physical storage to the pod.

### Replica

* Exist on other node connect to the same service.

### Deployment

* A layer of abstraction on pods.
* Stateless applications.
* Blueprint for app pods.
* Specify the replica numbers.
* Can't replicate database because it's stateful.

### StatefulSet

* For stateful apps like MySQL, Elasticsearch.
