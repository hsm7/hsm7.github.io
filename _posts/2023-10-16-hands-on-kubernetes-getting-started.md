---
layout: post
title:  "Hands-On Kubernetes: Getting Started with Kubernetes Core Concepts"
date:   2023-10-16 08:00:18 +0200
description: Hands-on introduction to kubernetes core concepts. This is the first blog post in the hands-on kubernetes blog series.
categories: Kubernetes
tags: [kubernetes]
image: /assets/img/featured/venti-views-1cqIcrWFQBI-unsplash.jpg
caption: Photo by <a href="https://unsplash.com/@ventiviews?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Venti Views</a> on <a href="https://unsplash.com/photos/1cqIcrWFQBI?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash">Unsplash</a> 
---

Since I'm prepairing for the Linux Foundation's Certified Kubernetes Application Developer (CKAD) exam, I've decided to start a blog series about Kubernetes.

This series is a practical guide to Kubernetes. It mainly targets application developers so we won't be looking at Kubernetes administration tasks like setting up a cluster with kubeadm and we won't be using a production-grade Kubernetes distribution. Instead we'll setup a single node development cluster and proceed our practical exploration of Kubernetes from core concepts to more advanced Kubernetes features.

In this first post we'll start with an introduction to Kubernetes, its main features, and its architecture, after that we'll set up a single node Kubernetes cluster, then we'll dive into Kubernetes objects and interact with the Kubernetes API using kubectl.

## Introduction

Kubernetes, often abbreviated as K8s, was originally developed by Google and is now maintained by the Cloud Native Computing Foundation (CNCF). Kubernetes is a portable, extensible, open source container orchestration platform for managing containerized workloads and services, that facilitates both declarative configuration and automation. It has a large, rapidly growing ecosystem.

While containers provide a portable and isolated environment for applications, managing them at scale can be a complex task. This is where Kubernetes comes in. Kubernetes abstracts away the underlying infrastructure, allowing us to focus on defining how our applications should run. Kubernetes supports various container runtimes, including Docker and OCI-compliant runtimes, making it vendor-agnostic and adaptable to different container technologies.

### Kubernetes Core Features

Kubernetes provides a range of features to help manage and orchestrate containers effectively. Some of the core features of Kubernetes include:

1. **Self-Healing**: Kubernetes automatically replaces failed containers, nodes, or pods, which helps maintain the desired state of our applications without manual intervention.

2. **Horizontal and Vertical Scaling**: We can scale our applications horizontally by adding or removing replicas and vertically by adjusting the resource requests and limits of containers.

3. **Service Discovery**: Kubernetes provides a DNS service for dynamically discovering services and routing traffic to containers by their service name.

4. **Automated Load Balancing**: Kubernetes offers built-in load balancing to distribute incoming traffic across containers, ensuring even distribution and high availability.

5. **Configuration Management**: Kubernetes supports the declarative configuration of applications using YAML or JSON files, enabling us to define our application's desired state, which Kubernetes then strives to maintain.

6. **Rolling Updates and Rollbacks**: Kubernetes supports rolling updates, allowing us to update our applications without downtime by incrementally replacing old containers with new ones. In case of issues, we can easily roll back to a previous versions.

7. **Role-Based Access Control (RBAC)**: Kubernetes offers RBAC, allowing fine-grained control over who can access and perform actions within a cluster.

8. **Monitoring and Logging**: Kubernetes can integrate with various monitoring and logging tools to help us collect, analyze, and visualize the performance and health of our applications.

9. **Extensibility**: Kubernetes is highly extensible and allows us to define custom resources and controllers to adapt it to our specific needs.

These core features make Kubernetes a powerful platform for container orchestration and application management, suitable for a wide range of deployment scenarios, from simple web applications to complex microservices architectures.

Now let's break down Kubernetes architecture to understand how it manages to achieve that.

## Kubernetes Architecture

A typical Kubernetes cluster consists of a set of nodes, that run containerized applications. The architecture of Kubernetes is designed to provide a highly available and scalable platform for deploying these containerized applications. It consists of various components that work together seamlessly. 

Kubernetes components can be further classified to control plane components and worker node components. The following figure demonstrate Kubernetes cluster architecture and its main components.

[//]: # (TODO: kubernetes components diagram)

Here is a brief description of these components and their functions in a Kubernetes cluster:

### Control Plane Components

In Kubernetes, the control plane is responsible for managing the overall state of the cluster, making decisions about when and where to deploy applications, scaling them, and ensuring their high availability. The control plane components are critical for the proper functioning of a Kubernetes cluster. Here are the main control plane components:

1. **API Server**: The API server is the entry point for the Kubernetes control plane. It exposes the Kubernetes API, which is used by both users and other components to interact with the cluster. It validates and processes requests, and communicates with the etcd store to read and write cluster state information.

2. **etcd**: etcd is a distributed key-value store that acts as the cluster's database. It stores all configuration data and the desired state of the cluster. etcd is essential for maintaining consistency and fault tolerance in the cluster.

3. **Scheduler**: The scheduler is responsible for placing workloads onto nodes in the cluster. It takes into account factors like resource requirements, node capacity, and user-defined constraints when making placement decisions. The scheduler helps ensure that workloads are distributed effectively and that there's high availability.

4. **Controller Manager**: The controller manager runs a set of controllers that regulate the state of the system. Controllers are responsible for monitoring the desired state of resources and making changes to bring the actual state in line with the desired state. Examples of controllers include the Replication Controller, Deployment Controller, and Node Controller.

5. **Cloud Controller Manager (optional)**: In cloud-based Kubernetes deployments, the Cloud Controller Manager interacts with the cloud provider's API to manage resources like load balancers, storage volumes, and network routes. It externalizes cloud-specific functionality from the core Kubernetes components.

These control plane components work together to maintain the desired state of the Kubernetes cluster, ensure high availability, and make dynamic decisions about resource allocation and workload placement. Properly configuring and securing these components is essential for the reliable operation of a Kubernetes cluster.

### Worker Nodes Components

Worker nodes in a Kubernetes cluster are responsible for running containers and executing the tasks assigned to them by the control plane. These worker nodes host the actual workloads and applications. The components that make up a Kubernetes worker node include:

1. **Kubelet**: Kubelet is an agent that runs on each node and communicates with the control plane. It is responsible for ensuring that containers are running healthy in a Pod as specified in that Pod specifications.

2. **Container Runtime**: The container runtime is the software responsible for running containers. Docker, containerd, and cri-o are some of the commonly used container runtimes in Kubernetes.

3. **Kube Proxy**: Kube Proxy is a network proxy that maintains network rules on nodes. It is responsible for forwarding traffic to the appropriate Pod based on IP and port information. Kube Proxy helps provide a simple network abstraction to Pods running on a node.

4. **Supplementary Services**: These may include various services and agents necessary for specific functionality, like monitoring, logging, or other infrastructure-related tasks.

These components work together to ensure that containers are scheduled, deployed, and maintained on the worker nodes in a Kubernetes cluster. They are responsible for handling the low-level details of running containers.

## Setting Up a Cluster

## Kubernetes Objects

Understanding Kubernetes architecture requires grasping some fundamental concepts:

1. **Nodes**: These are the worker machines that run containerized applications. Kubernetes can manage a cluster of nodes, making it easy to scale your applications.

2. **Pods**: Pods are the atomic unit in Kubernetes, representing a single instance of a running process. They can contain one or more containers that share the same network and storage. Pods are co-located on nodes and are managed by the Kubelet.

3. **Services**: Services enable network communication between different parts of your application, both within the cluster and with external services. They provide stable endpoints and load balancing for pods.

4. **Replica Sets and Deployments**: These concepts enable the scaling and management of application replicas, ensuring high availability and fault tolerance.

5. **ConfigMaps and Secrets**: These resources are used for storing configuration data and sensitive information, such as API keys, database passwords, and environment variables.

6. **Namespaces**: Namespaces are virtual clusters within a physical cluster, allowing you to segment resources logically, improving isolation, and management.

7. **Volumes**: Kubernetes provides different types of volumes for managing data, including local storage, network-attached storage, and cloud storage.

8. **Labels and Selectors**: Labels are key-value pairs that can be attached to objects (pods, services) to categorize them. Selectors are used to filter and query these objects based on labels.

## Conclusion

Kubernetes has revolutionized container orchestration, simplifying the deployment, scaling, and management of containerized applications. Understanding its architecture and core concepts is essential for anyone looking to harness the full power of this platform. By mastering these fundamentals, you'll be well-equipped to build and operate resilient and scalable containerized applications in a Kubernetes cluster. As Kubernetes continues to evolve, staying up-to-date with its concepts and best practices is crucial for success in the world of container orchestration.