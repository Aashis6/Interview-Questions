# Question 1

How to handle kubernetes security?

1. By default pods can communicate with each other. We can define network policies to avoid this.
2. Using RBAC role( Role based access Controls)
3. USe namespaces for multi tenancy.
4. Set admission control policies to avoid running the privileged containers.
5. Turn on audit logging.

# Questions 2

How two containers running on single node can have single IP ?

Pause containers for sharing network. --> Read it.

# Question 3

What is service mesh?

Istio

# Question 4

What is init container? Why do we use it?

These are container which should run before running the actual container. For example, creating a volumen 

# Question 5

What is pod disruption budget?

A Pod Disruption Budget (PDB) is a resource provided by Kubernetes to help ensure the high availability of applications running within a Kubernetes cluster during disruptive events such as node maintenance, software upgrades, or hardware failures.

Here's an overview of how Pod Disruption Budgets work:

**Definition**: A Pod Disruption Budget is a Kubernetes object that specifies the minimum number of pods of a certain type that must remain available during disruptions. It allows you to define constraints on how many pods of a particular application can be unavailable simultaneously.

**Usage**: You can define a Pod Disruption Budget for a specific deployment, stateful set, or replica set using a Kubernetes manifest file. Within the PDB, you specify the maximum number or percentage of pods that can be unavailable at any given time.

**Disruption budget enforcement**: Kubernetes uses the Pod Disruption Budget to control the eviction of pods during disruptive events. When a node needs to be drained for maintenance or other reasons, Kubernetes considers the PDB constraints before evicting pods from the node. It ensures that the desired number of pods specified in the PDB remain available.

**Pod disruption budget types**: There are two types of Pod Disruption Budgets:

Minimum availability: Specifies the minimum number of pods that must remain available.
Maximum disruption: Specifies the maximum number or percentage of pods that can be unavailable.
Examples of use cases: Pod Disruption Budgets are useful for ensuring that critical applications maintain a certain level of availability during node maintenance, rolling updates, or other cluster disruptions. They are commonly used in stateful applications where maintaining data consistency and availability is crucial.

Overall, Pod Disruption Budgets help operators manage the availability of applications running in Kubernetes clusters by defining and enforcing availability constraints, thereby minimizing the impact of disruptive events on application availability and user experience.





# Question 5

What is voluntarily and involuntarily disruption?

In the context of Kubernetes and Pod Disruption Budgets (PDBs), the terms "voluntary" and "involuntary" disruptions refer to different types of events or actions that can affect the availability of pods within a cluster:

1. **Voluntary Disruptions**:
   - These are planned disruptions initiated by the cluster administrator or operator.
   - Examples include node maintenance, software upgrades, scaling down resources, or draining a node for maintenance or decommissioning.
   - Voluntary disruptions are typically performed with advance notice and coordination to minimize the impact on applications and users.
   - Kubernetes allows administrators to control how pods are evicted during voluntary disruptions using Pod Disruption Budgets (PDBs).

2. **Involuntary Disruptions**:
   - These are unplanned disruptions that occur due to unforeseen events such as hardware failures, network issues, or sudden crashes.
   - Involuntary disruptions can result in the loss of pod availability without advance warning.
   - Kubernetes attempts to handle involuntary disruptions gracefully by rescheduling affected pods onto healthy nodes in the cluster.
   - Pod Disruption Budgets (PDBs) can also help mitigate the impact of involuntary disruptions by enforcing availability constraints and preventing too many pods from being evicted simultaneously.

In summary, voluntary disruptions are planned and initiated by administrators for maintenance or scaling purposes, while involuntary disruptions are unplanned events that can impact pod availability. Kubernetes provides mechanisms such as Pod Disruption Budgets to manage and mitigate the impact of both types of disruptions on application availability and cluster stability.



# Question 6

What is custom controller?


A custom controller in Kubernetes is a specialized control loop that extends the functionality of the Kubernetes API server to manage custom resources or perform custom operations beyond what is provided by the built-in controllers.

Custom controllers can be used for various purposes, such as managing complex applications, implementing advanced scheduling policies, enforcing custom security policies, or integrating with external systems. Examples include the Horizontal Pod Autoscaler (HPA) controller for autoscaling applications based on CPU utilization, the Custom Resource Definition (CRD) controller for managing custom resources, and controllers for managing stateful applications or performing application-specific operations.

Overall, custom controllers provide a powerful mechanism for extending Kubernetes to meet diverse requirements and enable users to build and manage complex, custom workflows within their Kubernetes clusters.



# Question 7

What is a side car container?

A sidecar container is a design pattern in Kubernetes where an additional container is deployed alongside a primary application container within the same pod. The sidecar container works in conjunction with the main application container, providing supporting functionalities such as logging, monitoring, or proxying.

Example use cases for sidecar containers include:

**Logging**: A sidecar container may be responsible for collecting logs generated by the main application container and forwarding them to a centralized logging system.
**Monitoring**: A sidecar container could monitor the resource usage and performance metrics of the main application container, reporting them to a monitoring system.
**Security**: Sidecar containers can handle tasks such as encryption/decryption, authentication, or traffic filtering to enhance the security posture of the application.

Overall, sidecar containers provide a flexible and scalable approach to extending the capabilities of Kubernetes pods, enabling the deployment of modular and composable application architectures.


# Question 8

What is pod security policy?


# Question 9

What is daemonsets in kubernetes?

A DaemonSet in Kubernetes ensures that a copy of a pod runs on each node in a cluster. Here's what it does:

1. **One Pod per Node**: It ensures that exactly one instance of a specified pod runs on each node in the cluster.
    If a new node is added to the cluster, the DaemonSet automatically schedules a new pod on that node.

3. **Node-Level Operations**: DaemonSets are useful for node-level operations such as monitoring agents,
   log collection, storage daemons, or any other system-level services that should run on every node.

5. **Automatic Pod Creation**: When a DaemonSet is created, Kubernetes automatically creates pods on
   every node that matches the DaemonSet's node selector or affinity rules.

7. **Self-Healing**: DaemonSets are self-healing, meaning if a pod in the DaemonSet goes down or gets deleted,
   Kubernetes automatically creates a new pod to maintain the desired state.

9. **Node Lifecycle Management**: DaemonSets ensure that the specified pods are always running and available,
    even as nodes are added or removed from the cluster.

Overall, DaemonSets are a powerful tool for deploying system-level services across all nodes in a Kubernetes cluster, 
ensuring uniformity and consistency in the environment.

**Use case** Monitoring Solution

```apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: fluentd
  namespace: kube-system
  labels:
    k8s-app: fluentd-logging
spec:
  selector:
    matchLabels:
      name: fluentd
  template:
    metadata:
      labels:
        name: fluentd
    spec:
      containers:
      - name: fluentd
        image: fluent/fluentd:v1.11.2-debian-1
        env:
        - name: FLUENT_UID
          value: "0"  # Run Fluentd as root
        resources:
          limits:
            memory: 200Mi
            cpu: 100m
          requests:
            memory: 100Mi
            cpu: 50m
        volumeMounts:
        - name: varlog
          mountPath: /var/log
        - name: varlibdockercontainers
          mountPath: /var/lib/docker/containers
          readOnly: true
      volumes:
      - name: varlog
        hostPath:
          path: /var/log
      - name: varlibdockercontainers
        hostPath:
          path: /var/lib/docker/containers
```

# Question 10

What is headless service?

Headless services in Kubernetes are services that do not have a cluster IP assigned to them.
Instead of load balancing traffic across a set of pods, headless services allow direct communication with individual pods.
 They are commonly used for scenarios such as:

1. **StatefulSets**: When deploying stateful applications like databases, each pod typically requires a s
table and unique network identity. Headless services ensure that each pod in a StatefulSet can be uniquely addressed by DNS.

2. **Service Discovery**: Headless services provide a way to discover and connect to individual pods directly by their DNS name.
This is useful when applications need to communicate directly with specific instances of a service.

3. **Custom Load Balancing**: Applications that require custom load balancing or routing logic can use headless services
to manage network traffic according to their specific requirements.

4. **Multicast or Broadcast Communication**: Some applications require multicast or broadcast communication patterns,
 which are not supported by regular Kubernetes services. Headless services can be used to enable such communication patterns.

In summary, headless services offer a way to bypass the default load balancing behavior of Kubernetes services
and enable direct communication with individual pods, making them particularly useful for stateful applications and
custom networking requirements.

Stateful creates pod in sequential order whereas deployment creates all in one go.

# Question 11

What is helm charts?

Helm is a package manager for Kubernetes that helps you manage Kubernetes applications. 
Helm Charts are packages of pre-configured Kubernetes resources. 
They can include things like deployments, services, ingress rules, and more, all defined in a structured format.

Using Helm Charts, you can easily deploy complex applications onto Kubernetes clusters without having 
to manually configure each resource. Helm Charts can be shared and reused, making it easier to manage
and distribute Kubernetes applications. They also support versioning and dependency management,
allowing you to easily upgrade or rollback releases.

In summary, Helm Charts are a convenient way to package, deploy, and manage applications on Kubernetes clusters.
They streamline the deployment process and promote best practices for managing Kubernetes applications.

# Question 12

What is HPA in kubernetes?

Horizontal Pod Autoscaling (HPA) in Kubernetes automatically scales the number of pods in a deployment, replication controller, or replica set based on observed CPU utilization or custom metrics. Here's the syntax for defining HPA in YAML:

```yaml
apiVersion: autoscaling/v2beta2
kind: HorizontalPodAutoscaler
metadata:
  name: example-hpa
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment   # Kind of the resource to scale
    name: example-deployment  # Name of the resource to scale
  minReplicas: 2   # Minimum number of pods
  maxReplicas: 10  # Maximum number of pods
  metrics:
  - type: Resource   # Type of metric (Resource or External)
    resource:
      name: cpu      # Name of the resource to scale on (cpu or memory)
      targetAverageUtilization: 50   # Target average CPU utilization percentage
```

In this YAML:

- `apiVersion` specifies the Kubernetes API version.
- `kind` specifies the kind of resource, which is HorizontalPodAutoscaler in this case.
- `metadata` includes the name of the HorizontalPodAutoscaler.
- `spec` defines the scaling behavior.
  - `scaleTargetRef` specifies the deployment, replication controller, or replica set to scale.
  - `minReplicas` specifies the minimum number of pods.
  - `maxReplicas` specifies the maximum number of pods.
  - `metrics` defines the metrics to use for scaling.
    - `type` specifies the type of metric, which can be `Resource` (CPU or memory) or `External`.
    - For `Resource` type metrics, `resource` specifies the name of the resource (`cpu` or `memory`) and the target average utilization percentage.

This YAML file can be applied to the Kubernetes cluster using the `kubectl apply -f filename.yaml` command to create the HorizontalPodAutoscaler resource.

# Question 13

What is RBAC and how to setup?

Create a namespace.

```kubectl create ns test ```

Create a service account of name foo

```
apiVersion: v1
kind: ServiceAccount
metadata:
  name: foo
  namespace: test
```

```kubectl apply -f serviceaccount.yml```

Create a role.
```
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: testadmin
  namespace: test
rules:
- apiGroups: ["*"]
  resources: ["pods"]
  verbs: ["get", "list", "watch"]
```

  ```Kubectl apply -f role.yml```

  Do the role binding.
  
```
  apiVersion: rbac.authorization.k8s.io/v1
kind: RoleBinding
metadata:
  name: testadminbinding
  namespace: test
subjects:
- kind: ServiceAccount
  name: foo
  apiGroup: ""
roleRef:
  kind: Role
  name: testadmin
  apiGroup: ""
```
```kubectl apply -f rolebinding.yml```

```auth can-i --as system:serviceaccount:test:foo list pods -n test```

Ouput should be yes


# Question 14

Define components of kubernetes.

Kubernetes is a powerful container orchestration platform that automates the deployment, scaling, and management of containerized applications. It comprises several key components that work together to provide a robust and scalable platform for running containerized workloads. Here are the core components of Kubernetes:

1. **Master Components**:

    - **kube-apiserver**: This component provides the Kubernetes API server, which is the front-end for the Kubernetes control plane. It serves the Kubernetes API, which allows users and other components to interact with the Kubernetes cluster.
    
    - **kube-controller-manager**: The controller manager runs various controllers that are responsible for managing the state of the cluster. These controllers include the Node Controller, Replication Controller, Endpoint Controller, and others. Each controller watches the state of the cluster through the API server and works to ensure that the desired state is achieved.
    
    - **kube-scheduler**: The scheduler is responsible for placing newly created pods onto nodes in the cluster. It takes into account factors such as resource requirements, node capacity, and user-specified constraints when making scheduling decisions.

    - **etcd**: etcd is a distributed key-value store used by Kubernetes to store cluster state. It is a consistent and highly available data store that provides the authoritative source of truth for the cluster configuration and state.

2. **Node Components**:

    - **kubelet**: The kubelet is an agent that runs on each node in the cluster and is responsible for managing the containers running on that node. It communicates with the API server to receive instructions about which containers to run and to report the status of containers back to the control plane.
    
    - **kube-proxy**: The kube-proxy is responsible for network proxying and load balancing within the cluster. It maintains network rules on each node to route traffic to the appropriate pods based on their IP addresses and port numbers.
    
    - **Container Runtime**: The container runtime is the software responsible for running containers. Kubernetes supports various container runtimes, including Docker, containerd, and CRI-O. The container runtime is responsible for pulling container images, creating containers, and managing their lifecycle.

3. **Add-Ons**:

    - **DNS**: Kubernetes provides a DNS service that allows pods to discover and communicate with each other using DNS names. This enables services to be accessed by their names within the cluster without needing to know the IP addresses of individual pods.
    
    - **Dashboard**: The Kubernetes dashboard is a web-based UI that provides a graphical interface for managing and monitoring the cluster. It allows users to view information about the state of the cluster, create and manage resources, and troubleshoot issues.
    
    - **Ingress Controller**: An Ingress controller manages external access to services in the cluster. It typically provides features such as load balancing, SSL termination, and path-based routing to allow external clients to access services running in the cluster.
    
    - **Monitoring and Logging**: Various monitoring and logging solutions can be integrated with Kubernetes to collect metrics and logs from the cluster components and applications running within the cluster. Examples include Prometheus for monitoring and Elasticsearch/Fluentd/Kibana (EFK stack) or Loki/Promtail/Grafana (PLG stack) for logging.
    
These components work together to provide the functionality required to deploy, manage, and scale containerized applications in Kubernetes clusters. Each component plays a specific role in the overall architecture of Kubernetes and contributes to the platform's scalability, reliability, and flexibility.

# Question 15

What is NetworkPolicy ?

By Creating a NetworkPolicy object , we specified which pod can access the service/pod based on there labels.
This can be useful in a production env. where security is a concern and access to certain service/pod need to
be restricted. NetworkPolicy in K8s to limit access to a services/pods.

Lets understand with one example , we have a frontend ,backend & database pod and we have created respective services fro each pod. All the pod and services are having there IP add. In K8s all the pods and services are in same network so by default any pod/service will conect easly to any pod/service using there IP address and name. It means any pod will connect to our database pod easly and this is a security concern and our database pod does not have any security because any pod will connect to database pod.

NP is applied on pod and we can control there traffic with the help of NP. Only one NP wiil apply on one Pod.

Yaml File:-

apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: db-network-policy
spec:
  podSelector:
    matchLabels:
      role: db-pod
  policyTypes:
  - Ingress
  - Egress
  ingress:
  - from:
    - podSelector:
        matchLabels:
          role: internal-db
      namespaceSelector:
        matchLabels:
          name: dev
    ports:
    - protocol: TCP
      port: 8080
  egress:
  - to:
    - ipBlock:
        cidr: 172.17.0.0/16
        except:
        - 172.17.1.0/24
    ports:
    - protocol: TCP
      port: 30000
      endPort: 32768













