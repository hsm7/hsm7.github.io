---
layout: post
title:  "Hands-On Kubernetes: Abstractions and Core Concepts"
date:   2023-10-16 08:00:18 +0200
description: An Introduction to kubernetes abstractions and core concepts. This is the first blog post in the hands-on kubernetes blog series.
categories: Kubernetes
tags: [kubernetes]
image: /assets/images/featured/venti-views-1cqIcrWFQBI-unsplash.jpg
---

Since I'm prepairing for the Linux Foundation's Certified Kubernetes Application Developer (CKAD) exam, I decided to start a blog series about Kubernetes.

In this first post we'll start with an introduction to Kubernetes, its main features, its architecture and components, and its core abstractions which we'll explore further in the next parts of this series.

## Introduction

Kubernetes, often abbreviated as K8s, was originally developed by Google and is now maintained by the Cloud Native Computing Foundation (CNCF). Kubernetes is a portable, extensible, open source container orchestration platform for managing containerized workloads and services, that facilitates both declarative configuration and automation. It has a large, rapidly growing ecosystem.

While containers provide a portable and isolated environment for applications, managing them at scale can be a complex task. This is where Kubernetes comes in. Kubernetes abstracts away the underlying infrastructure, allowing us to focus on defining how our applications should run. Kubernetes supports various container runtimes, including Docker and OCI-compliant runtimes, making it vendor-agnostic and adaptable to different container technologies.

### Kubernetes Core Features

Kubernetes provides a range of features to help manage and orchestrate containers effectively. Some of the core features of Kubernetes include:

- **Self-Healing** Kubernetes automatically replaces failed containers, nodes, or pods, which helps maintain the desired state of our applications without manual intervention.

- **Horizontal and Vertical Scaling** We can scale our applications horizontally by adding or removing replicas and vertically by adjusting the resource requests and limits of containers.

- **Service Discovery** Kubernetes provides a DNS service for dynamically discovering services and routing traffic to containers by their service name.

- **Automated Load Balancing** Kubernetes offers built-in load balancing to distribute incoming traffic across containers, ensuring even distribution and high availability.

- **Configuration Management** Kubernetes supports the declarative configuration of applications using YAML or JSON files, enabling us to define our application's desired state, which Kubernetes then strives to maintain.

- **Rolling Updates and Rollbacks** Kubernetes supports rolling updates, allowing us to update our applications without downtime by incrementally replacing old containers with new ones. In case of issues, we can easily roll back to a previous versions.

- **Role-Based Access Control (RBAC)** Kubernetes offers RBAC, allowing fine-grained control over who can access and perform actions within a cluster.

- **Monitoring and Logging** Kubernetes can integrate with various monitoring and logging tools to help us collect, analyze, and visualize the performance and health of our applications.

- **Declarative Configuration** Kubernetes uses declarative configuration files (usually written in YAML) to define the desired state of the application and its components. This approach abstracts the details of how to achieve that state, leaving the Kubernetes control plane responsible for making it happen.

- **Extensibility** Kubernetes is highly extensible and allows us to define custom resources and controllers to adapt it to our specific needs.

These core features make Kubernetes a powerful platform for container orchestration and application management, suitable for a wide range of deployment scenarios, from simple web applications to complex microservices architectures.

Now let's break down Kubernetes architecture to understand how it manages to achieve that.

## Kubernetes Architecture

A typical Kubernetes cluster consists of a set of nodes, that run containerized applications. The architecture of Kubernetes is designed to provide a highly available and scalable platform for deploying these containerized applications. It consists of various components that work together seamlessly. 

Kubernetes components can be further classified to control plane components and worker node components. The following figure demonstrates Kubernetes cluster architecture and its main components.

![Figure 1 - Kubernetes Cluster Architecture](/assets/images/posts/hands-on-kubernetes/kubernetes-cluster-architecture-dark.svg)
*Figure 1 - Kubernetes Cluster Architecture*

Here is a brief description of these components and their functions in a Kubernetes cluster:

### Control Plane Components

In Kubernetes, the control plane is responsible for managing the overall state of the cluster, making decisions about when and where to deploy applications, scaling them, and ensuring their high availability. The control plane components are critical for the proper functioning of a Kubernetes cluster. Here are the main control plane components:

- **API Server** The API server is the entry point for the Kubernetes control plane. It exposes the Kubernetes API, which is used by both users and other components to interact with the cluster. It validates and processes requests, and communicates with the etcd store to read and write cluster state information.

- **etcd** etcd is a distributed key-value store that acts as the cluster's database. It stores all configuration data and the desired state of the cluster. etcd is essential for maintaining consistency and fault tolerance in the cluster.

- **Scheduler** The scheduler is responsible for placing workloads onto nodes in the cluster. It takes into account factors like resource requirements, node capacity, and user-defined constraints when making placement decisions. The scheduler helps ensure that workloads are distributed effectively and that there's high availability.

- **Controller Manager** The controller manager runs a set of controllers that regulate the state of the system. Controllers are responsible for monitoring the desired state of resources and making changes to bring the actual state in line with the desired state. Examples of controllers include the Replication Controller, Deployment Controller, and Node Controller.

- **Cloud Controller Manager (optional)** In cloud-based Kubernetes deployments, the Cloud Controller Manager interacts with the cloud provider's API to manage resources like load balancers, storage volumes, and network routes. It externalizes cloud-specific functionality from the core Kubernetes components.

These control plane components work together to maintain the desired state of the Kubernetes cluster, ensure high availability, and make dynamic decisions about resource allocation and workload placement. Properly configuring and securing these components is essential for the reliable operation of a Kubernetes cluster.

### Worker Nodes Components

Worker nodes in a Kubernetes cluster are responsible for running containers and executing the tasks assigned to them by the control plane. These worker nodes host the actual workloads and applications. The components that make up a Kubernetes worker node include:

- **Kubelet** Kubelet is an agent that runs on each node and communicates with the control plane. It is responsible for ensuring that containers are running healthy in a Pod as specified in that Pod specifications.

- **Container Runtime** The container runtime is the software responsible for running containers. Docker, containerd, and cri-o are some of the commonly used container runtimes in Kubernetes.

- **Kube Proxy** Kube Proxy is a network proxy that maintains network rules on nodes. It is responsible for forwarding traffic to the appropriate Pod based on IP and port information. Kube Proxy helps provide a simple network abstraction to Pods running on a node.

- **Supplementary Services** These may include various services and agents necessary for specific functionality, like monitoring, logging, or other infrastructure-related tasks.

These components work together to ensure that containers are scheduled, deployed, and maintained on the worker nodes in a Kubernetes cluster. They are responsible for handling the low-level details of running containers.

## Kubernetes Abstractions

Kubernetes abstracts infrastructure-specific details by providing a high-level, declarative way to define and manage containerized applications. This abstraction allows developers and operators to focus on the application itself rather than the specific details of the underlying infrastructure.

Kubernetes also provides a common interface that works across various cloud providers and on-premises environments. This enables us to move our applications between different infrastructure providers without major code or configuration changes.

In the following sections we'll explore the core Kubernetes abstractions.

### Kubernetes Workloads

Containerization abstracts the application and its dependencies by encapsulating them within containers. Containers package the application code, libraries, and runtime environments, making them highly portable and independent of the host infrastructure.

Kubernetes abstracts containers through the concept of pods. A pod can contain one or more containers, and Kubernetes schedules pods, not individual containers. This abstraction allows Kubernetes to handle container lifecycle management and other orchestration tasks.

#### Pods 

Pods are the smallest deployable units in Kubernetes. They can contain one or more containers that share the same network and storage namespace. Pods are often used for running closely related processes or multiple containers that need to work together. Pods are generally not created directly and are created using workload resources.

Kubernetes manages the lifecycle of Pods, including scheduling, scaling, monitoring, restarting, and self-healing. If a Pod fails, Kubernetes can automatically replace it.

#### Workload Resources

Kubernetes workloads are a way to define, deploy, and manage these applications and services in a Kubernetes cluster. Workloads in Kubernetes represent different types of applications and the desired state of those applications within the cluster. There are several types of Kubernetes workloads, each designed for different use cases and requirements.

Here are the built-in Kubernetes workloads:

- **ReplicaSets:** ReplicaSets are used to ensure that a specified number of replica Pods are running at all times. They are a basic building block for achieving high availability and scaling applications.

- **Deployments:** Deployments are a higher-level abstraction over ReplicaSets, allowing us to declaratively manage applications while providing updates and rollbacks. Deployments enable us to easily scale and upgrade our applications.

- **StatefulSets:** StatefulSets are used for stateful applications that require stable network identifiers and persistent storage. Examples include databases and distributed systems.

- **DaemonSets:** DaemonSets ensure that a copy of a Pod runs on each node in the cluster. They are often used for cluster-level operations like log collection, monitoring, or networking tasks.

- **Jobs:** Jobs are used to run short-lived, non-replicated tasks to completion. They are often used for batch processing, data analysis, or one-time administrative tasks.

- **CronJobs:** CronJobs are a time-based scheduler for running Jobs at specified intervals. They are useful for automating periodic tasks.

Each of these workload types allows us to define the desired state of our application or service, and Kubernetes takes care of the details of managing, scaling, and maintaining the workload according to that desired state. Workloads are an essential concept in Kubernetes for ensuring the efficient operation of containerized applications in a dynamic and scalable environment.

### Networking and Services

In a typical Kubernetes cluster there might be handreds of nodes running thousands of pods. Some of those pods requires communication with other pods in the cluster, and some of them needs to be exposed to an external network. Kubernetes abstracts networking through a combination of network plugins, services, and other networking abstractions, allowing for flexible and efficient management of the underlying networking infrastructure, while providing simple and useful abstractions for applications developers. The following figure is a simpified view of services and networking in Kubernetes:

![Figure 2 - Simplified View of Services and Networking in Kubernetes](/assets/images/posts/hands-on-kubernetes/simplified-kubernetes-networking-model-dark.svg)
*Figure 2 - Simplified View of Services and Networking in Kubernetes*

Let's break down some of the essential networking concepts in Kubernetes:

#### Network Model

Kubernetes networking is a complex topic that encompasses how pods communicate with each other, external users, and the broader network. 

- **Pod Networking** In Kubernetes, pods are assigned unique IP addresses in a cluster-internal network. Allowing them to communicate with each other within the same cluster. This internal networking is crucial for applications that consist of multiple microservices running in different pods.

- **Cluster Networking** For pods to communicate outside their cluster, Kubernetes relies on cluster networking. Cluster networking plugins, like Calico, Flannel, and Cilium, are responsible for managing the routing and networking between different pods and external networks. These plugins ensure that pods can reach external services and resources while maintaining security and isolation.

- **Network Policies** While Pod Networking provides an internal network for pods to communicate, Kubernetes also offers network policies that allow us to control which pods can communicate with each other and define network segmentation. Network policies can enhance security by specifying ingress and egress rules for pods, ensuring that only allowed traffic flows between them.

#### Services

Kubernetes services are an abstraction that simplifies pod discovery and network access. They provide a consistent way to access and communicate with our application components, even as they scale up or down. Services abstract the network layer, allowing us to refer to a service by its name rather than specific IP addresses. They also act as load balancers, enabling external or internal traffic to be routed to the appropriate pods. There are three types of Kubernetes services:

- **ClusterIP** ClusterIP services provide a stable, internal IP address within the cluster. These services are accessible only from within the cluster, making them ideal for communication between different components of an application.

- **NodePort** NodePort services expose applications on a specific port across all nodes in a cluster. This allows external access to a service, even from outside the cluster, via the node's IP address.

- **LoadBalancer** LoadBalancer services provide an external IP address and distribute incoming traffic to the pods behind the service. They are commonly used for public-facing applications. LoadBalancer service type relies on an external load balancer provisioned by a Cloud provider.

- **Ingress** Ingress is not exactly a service but rather an API object that manages external access to services within the cluster. It offers powerful routing and host-based traffic management capabilities.It's important to note that Kubernetes itself does not provide a built-in Ingress controller. Instead, there are several implementations available, such as Nginx Ingress Controller, Traefik, HAProxy Ingress, and others. These controllers implement the Ingress resource and handle the actual traffic routing and load balancing based on the defined ingress rules.

Understanding Kubernetes networking and services is vital for managing containerized applications effectively. These components enable us to connect, secure, and expose our services to the world while abstracting much of the underlying complexity.

### Kubernetes Storage

Containers are designed to be lightweight and ephemeral, making them ideal for deploying and scaling applications quickly. However, this ephemeral nature also means that data stored within a container can be lost when the container terminates or moves to a different node. This limitation poses a challenge for stateful applications that rely on data persistence.

Kubernetes storage resources address this challenge by decoupling data from the container, ensuring data persistence and portability. The following figure illustrates how storage resources are managed in Kubernetes:

![Figure 3 - Stroage Resources in Kubernetes](/assets/images/posts/hands-on-kubernetes/storage-resources-in-kubernetes-dark.svg)
*Figure 3 - Stroage Resources in Kubernetes*

In the next sections we'll dive into these storage abstractions and their related concepts.

#### Volumes

Kubernetes Volumes provide a way to store and manage data in a containerized environment. Volumes are essentially directories that exist outside the container's file system but can be mounted into one or more containers in a Pod. Kubernetes provides various types of volumes, each with its own purpose and characteristics:

- **EmptyDir** An EmptyDir volume is created when a Pod is assigned to a node, and it shares the same lifetime as the Pod. It is primarily used for ephemeral storage or as a temporary scratch space.

- **HostPath** HostPath volumes are used to mount directories from the host node's file system into the Pod. While useful, they are not recommended in production environments, as they can lead to inconsistencies and issues with scalability and portability.

- **ConfigMap and Secret** ConfigMap and Secret volumes are used for reading configuration data or sensitive information from the Kubernetes API server. They are valuable for separating configuration data from the application code.

- **Persistent Volumes** Persistent Volumes (PVs) are a vital part of managing stateful applications in Kubernetes. PVs abstract the underlying storage, making it easier to manage, and can be shared across multiple Pods.

#### Persistent Volumes

A Persistent Volume (PV) in Kubernetes is a storage resource that abstracts and represents physical storage in a cluster. It provides a way to decouple the management of storage from the Pods that use it, offering a more scalable and flexible approach to data storage in containerized environments. Persistent Volumes are a key component of Kubernetes storage architecture and are particularly valuable for running stateful applications, such as databases, in a containerized infrastructure.

Persistent Volumes have a lifecycle independent of Pods. This means that data stored in a Persistent Volume can persist across the creation, deletion, or modification of Pods.

- **Persistent Volume Claims** To use a Persistent Volume, developers create a Persistent Volume Claim (PVC) with the desired specifications, such as storage class, access mode, and capacity. Kubernetes then dynamically provisions a Persistent Volume that matches these requirements. The PVC can be mounted into Pods, and the application can use the mounted storage as needed.

- **Storage Classes** Storage Classes define the characteristics of the Persistent Volumes. They encapsulate the storage-related configuration, such as the type of storage (e.g., SSD or HDD), access modes (ReadWriteOnce, ReadWriteMany, ReadOnlyMany), and reclaim policies. Storage Classes allow administrators to specify storage options and dynamically provision Persistent Volumes based on demand.

- **Dynamic Provisioning** Dynamic provisioning is a powerful feature in Kubernetes that simplifies the management of persistent storage. It enables automatic creation of PVs and PVCs as needed. With dynamic provisioning, the storage backend (e.g., a cloud provider's block storage) can automatically create PVs when a PVC is created, and delete them when no longer needed. Benefits of dynamic provisioning include simplifying storage management, reducing administrative overhead, and ensuring efficient utilization of resources. It also improves scalability as PVs are created dynamically based on demand.

Persistent Volumes in Kubernetes provide a standardized way to manage and allocate storage resources in a cluster, making it easier to handle data storage for stateful applications while ensuring data persistence and manageability in a containerized environment. Kubernetes abstracts storage dependencies by providing storage classes and persistent volume claims. This enables applications to request and use storage resources without being tightly coupled to specific storage solutions.

## Conclusion

Kubernetes has revolutionized container orchestration, simplifying the deployment, scaling, and management of containerized applications. Understanding its architecture and core abstractions is essential for anyone looking to harness the full power of cloud-native application development. 

In the next part we'll deploy a microservice application consisting of multiple Kubernetes resources.