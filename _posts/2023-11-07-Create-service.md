---
category: K8s
tags: [CKA]
usemath: [latex, ascii] 
---



CKA Kodecloud Mocking exam 1

> Create a service <mark>messaging-service</mark> to expose the <mark>messaging</mark> application within the cluster on port 6379.



이미 messaging 파드가 존재할 때

```bash
  $ kubectl expose pod messaging --port 6379 --name messaging-service
```

<code>kubectl expose --help</code>를 커맨드에 입력하여 결과물 중에 찾아보기!

커맨드를 이용해 문제를 해결할 수 있다.



# Kubernetes Services: A Guide

Kubernetes Services play a crucial role in managing the networking of your containerized applications. They provide a way to access and discover your application's Pods in a flexible and abstract manner. Let's dive into the essentials of Kubernetes Services.

## What is a Kubernetes Service?

A Kubernetes Service is an abstraction that defines a logical set of Pods and a policy for accessing them. It allows you to establish a loose coupling between dependent Pods, making your application more robust. Each Service is defined using YAML or JSON, similar to other Kubernetes objects.

## How Services Work

Services use label selectors to identify and group a set of Pods. These labels are key-value pairs attached to objects and can be used for various purposes, such as categorizing objects or specifying versions.

## Service Types

Kubernetes Services can be exposed in different ways by specifying a "type" in the "spec" section of the Service manifest. Here are the common types:

1. **ClusterIP (default):** This type exposes the Service on an internal IP within the cluster, making it reachable only from within the cluster.
2. **NodePort:** NodePort exposes the Service on the same port of selected Nodes, allowing external access using `<NodeIP>:<NodePort>`. It's a superset of ClusterIP.
3. **LoadBalancer:** This type creates an external load balancer in the cloud (if supported) and assigns a fixed, external IP to the Service. It's a superset of NodePort.
4. **ExternalName:** ExternalName maps the Service to an external domain name using a "CNAME" record. This type requires specific versions of kube-dns or CoreDNS.

## Service Without Selectors

In some cases, you might create a Service without specifying a selector in the manifest. This means the Service won't create corresponding Endpoints automatically, allowing you to map the Service to specific endpoints manually. Another scenario is when you're strictly using "ExternalName" type.

Kubernetes Services and Labels are powerful tools for managing and routing traffic to your application's Pods. They enable a high degree of flexibility and abstraction, making your application more resilient and adaptable in a containerized environment.