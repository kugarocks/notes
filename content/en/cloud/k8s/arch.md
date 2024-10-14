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
* 3 processes must be installed on each Node:
  * Container Runtime (e.g. Docker/Podman)
  * kubelet
  * kube-proxy
