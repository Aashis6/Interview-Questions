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



# Question 5

What is voluntarily and involuntarily disruption?



# Question 6

What is custom controller?



# Question 7

What is a side car container?


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
