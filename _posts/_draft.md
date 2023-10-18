This series is a practical guide to Kubernetes. It mainly targets application developers so we won't be looking at Kubernetes administration tasks like setting up a cluster with kubeadm and we won't be using a production-grade Kubernetes distribution. Instead we'll setup a single node development cluster and proceed our practical exploration of Kubernetes from core concepts to more advanced Kubernetes features.

### Kubernetes Namespaces

In Kubernetes, namespaces are a way to create multiple virtual clusters within a physical cluster. They provide a way to isolate resources, such as pods, services, and replication controllers, into separate groups. Namespaces are a fundamental feature for multi-tenancy and resource isolation in a Kubernetes cluster. Here are some key points about namespaces:

- **Default Namespace** When you create resources in a Kubernetes cluster without specifying a namespace, they are typically created in the default namespace. The default namespace is created by default in every cluster.

- **Access Control** Kubernetes allows you to apply Role-Based Access Control (RBAC) at the namespace level. This means you can define which users or service accounts have access to resources within a particular namespace, enhancing security and access control.

- **Resource Quotas and Limits** You can set resource quotas and limits on namespaces. This ensures that one namespace doesn't consume all the cluster's resources, helping to maintain cluster stability and fairness.

- **Organization and Management** Namespaces can be used for organizing and managing resources based on teams, projects, or environments. This can make it easier to understand and navigate the cluster's resources.

### Custom Resource Definitions

Kubernetes Custom Resource Definitions (CRDs) are an extension mechanism that enable us to define and create our custom resources, which can be treated and managed like built-in resources in your Kubernetes cluster. These custom resources are an essential part of the Kubernetes ecosystem and are commonly used to extend Kubernetes with domain-specific or application-specific resources.

Here are some key points to understand about Kubernetes Custom Resource Definitions:

- **Definition**: A Custom Resource Definition is an object that allows you to define the structure and behavior of custom resources. It specifies the kind of resource, the attributes (spec and status), and validation rules.

- **Custom Resources**: Once a CRD is defined, you can create instances of custom resources based on that CRD. These custom resources can be customized to match your specific use cases. For example, you could create a CRD for a "Database" and then create custom resources for various types of databases like MySQL, PostgreSQL, or MongoDB.

- **API Extension**: Custom resources, once created, become part of the Kubernetes API and can be managed using standard Kubernetes API operations, such as `kubectl apply`, `kubectl get`, `kubectl delete`, and others.

- **Controller Logic**: To make custom resources useful, you often need to create custom controllers that watch for changes in custom resources and take actions based on those changes. These controllers are responsible for reconciling the desired state (defined in the CR) with the actual state (in the cluster).

- **Operators**: In Kubernetes, you might hear the term "Operator." An Operator is essentially a design pattern for using custom resources and controllers to automate the management of applications and services. Operators are commonly used in stateful applications like databases to handle tasks like provisioning, scaling, and backup.

CRDs are incredibly flexible and can be used to extend Kubernetes for a wide range of use cases, from managing applications to creating custom networking resources and more. They are a powerful tool for making Kubernetes more adaptable to specific application requirements and domain-specific needs.