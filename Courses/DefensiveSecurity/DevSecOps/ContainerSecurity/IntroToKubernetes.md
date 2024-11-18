# Task 1 :  Introduction 

Kubernetes  is a highly popular technology in the **DevSecOps** space. It's an open-source system for automating the deployment, scaling, and management of containerized applications. In this room, we will break down the complexities of Kubernetes and explain its role in the modern container ecosystem.

Kubernetes is sometimes referred to as **K8s**, a numeronym, to save time and effort in the industry!

---

## Learning Prerequisites

Before diving into this room, it's recommended to have completed the following modules as part of your DevSecOps learning path:

- **Intro to Containerization**
- **Intro to Docker**

These modules will provide the foundational knowledge needed to understand Kubernetes effectively.

---

## Learning Objectives

In this room, we aim to:

1. **Understand why Kubernetes is needed**: Explore the challenges that Kubernetes solves in modern application deployment and management.
2. **Understand basic Kubernetes architecture**: Learn how Kubernetes is structured and why its architecture is key to efficient container orchestration.
3. **Understand key components of Kubernetes**: Familiarize yourself with essential Kubernetes components like Pods, Nodes, Services, etc.
4. **Traverse a Kubernetes cluster using `kubectl`**: Learn how to interact with and manage a Kubernetes cluster via the command-line tool `kubectl`.
5. **Kubernetes for DevSecOps and Best Practices**: Understand how Kubernetes is used by DevSecOps engineers and explore security best practices for managing Kubernetes clusters.

---

## Key Takeaways

- **Kubernetes** is at the heart of modern cloud-native application deployment and management.
- Mastering **K8s** concepts is crucial for anyone in the DevSecOps or cloud infrastructure space.
- The next steps will involve getting hands-on experience with Kubernetes, including best practices for securing K8s environments.

---

# Task 2 : Kubernetes 101

To better understand **Kubernetes**, we must first consider the context of its emergence. Why was it needed in the first place?

## The Rise of Microservices

In the past, many companies used **monolithic architecture** where applications were built as a single unit—one code base, one executable, deployed as one component. While this worked for many, the industry began shifting toward a **microservices architecture**. 

With **microservices**, an application is broken down into smaller, independent components, each responsible for a specific business function. This approach allows individual components to be scaled independently without affecting the entire application. A high-profile example of this shift was **Netflix** in 2009, which faced challenges scaling their monolithic system to meet increasing demands, leading them to adopt microservices.

## Why Microservices and Containers?

Microservices architecture often involves hundreds or thousands of containers, each running a different component of the application. This created the need for a technology to **manage and orchestrate** these containers efficiently. 

**Enter Kubernetes**.

---

## What is Kubernetes?

Kubernetes is a **container orchestration** system that helps manage containers at scale. Let's consider an example:

- Imagine a Docker container running an app that can be accessed externally. 
- If this application starts receiving more traffic, you need to spin up another container to distribute the load.
  
This is where **Kubernetes** comes in—**orchestrating** the deployment and management of these containers, ensuring resources are distributed effectively. Think of Kubernetes as a **conductor** in an orchestra, where the containers are the instruments and Kubernetes directs them, bringing in new containers when necessary and removing them when they are no longer needed.

---

## The Many Benefits of Kubernetes

Now that we know what Kubernetes is, let's explore its benefits, particularly in the **DevSecOps** space.

### 1. **High Availability and Scalability**
   - Kubernetes ensures that applications remain highly available by duplicating components (e.g., the app and its database) and distributing traffic to available resources. This removes **single points of failure** and **bottlenecks**, improving response times.
   - **Scalability**: Kubernetes allows workloads to be **scaled up or down** based on demand, helping companies adapt to fluctuating traffic.

### 2. **Portability**
   - Kubernetes is highly **portable**, meaning it can run on various infrastructures, including **single-cloud or multi-cloud** setups, making it an extremely flexible tool for developers.

### 3. **Compatibility and Ecosystem**
   - Kubernetes' popularity means that **many other technologies** are compatible with it, enhancing its capabilities and making it even more powerful when integrated with other tools.

---

## Why Kubernetes is So Popular in DevSecOps

Kubernetes' ability to make applications **scalable**, **available**, and **portable** is why it's so widely used and frequently discussed in the DevSecOps space. As a tool for managing and orchestrating containers, it has become indispensable for managing modern, distributed applications.

---

### Which benefit of Kubernetes means that it can run anywhere on any type of infrastructure?
highly portable

### Fill in the blank "Kubernetes is a _________ _____________ system"
container orchestration

# Task 3 : Kubernetes Architecture

Kubernetes is a powerful container orchestration platform, but to truly appreciate its capabilities, we need to explore its architecture. Here, we'll break down each key component and explain how they interact within the Kubernetes ecosystem.

## 1. **Kubernetes Pod**
- **Definition**: The smallest deployable unit in Kubernetes.
- **Composition**: A pod can contain one or more containers that share storage and network resources.
- **Communication**: Containers in a pod communicate as if they are on the same machine, maintaining isolation from other pods.
- **Role in Scalability**: Pods act as the unit of replication, so scaling a workload involves increasing the number of pods.

### Diagram: Pod Structure
(A diagram here would depict a pod containing one or more containers with shared storage and network.)
![Alt text](https://tryhackme-images.s3.amazonaws.com/user-uploads/6228f0d4ca8e57005149c3e3/room-content/98b54b6b8427fae556be0cdd516b9e36.svg)
## 2. **Kubernetes Nodes**
Nodes are the infrastructure on which Kubernetes runs workloads. They can be **virtual or physical machines** and house all the services needed to manage and run pods.

### Types of Nodes:
- **Control Plane (Master Node)**: Oversees the management of worker nodes and pods.
- **Worker Nodes**: Run the pods and maintain workloads.

## 3. **Kubernetes Cluster**
- **Definition**: A collection of nodes (both control plane and worker nodes) working together as a unified system.

## 4. **Kubernetes Control Plane**
The control plane is essential for managing the cluster. It ensures the cluster runs as expected by coordinating nodes and pods using several critical components:

### Key Components:
- **Kube-apiserver**: 
  - Acts as the front end of the control plane.
  - Exposes the Kubernetes API and handles API requests.
  - Scalable, allowing multiple instances for load balancing.
- **Etcd**:
  - A consistent, distributed key/value store holding the cluster's current state.
  - Updates reflect changes like new pod creation or resource allocation.
- **Kube-scheduler**:
  - Watches for unassigned pods and assigns them to appropriate nodes based on criteria like resource usage.
- **Kube-controller-manager**:
  - Runs various controller processes to manage the state of the cluster.
  - Example: Node controller monitors node health and coordinates with the scheduler to handle node failures.
- **Cloud-controller-manager**:
  - Manages cloud-specific control logic.
  - Separates internal cluster operations from those involving cloud provider APIs, allowing independent feature releases by cloud providers.

### Diagram: Control Plane Architecture
(A diagram illustrating these components interacting within the control plane.)

## 5. **Kubernetes Worker Node**
Worker nodes execute the workloads by running pods. Each worker node includes several critical components:

### Essential Worker Node Components:
- **Kubelet**:
  - An agent running on every node that ensures containers are running in their assigned pods.
  - Verifies that the container state matches the pod specifications.
- **Kube-proxy**:
  - Manages network communication rules.
  - Routes traffic within the cluster and directs traffic from services to corresponding pods.
- **Container Runtime**:
  - Software that runs containers (e.g., Docker, rkt, runC).
  - Must be installed on each node to support containerized applications.

### Diagram: Worker Node Structure
(A diagram showing Kubelet, Kube-proxy, and the container runtime on a worker node.)

## 6. **Communication Paths**
- **Control Plane to Nodes**:
  - Communication between the control plane and worker nodes occurs through well-defined paths to manage pod specifications, health checks, and workload distribution.
- **Hub and Spoke Model**:
  - The Kubernetes architecture follows a hub-and-spoke pattern where the **kube-apiserver** serves as the central hub for communication between components.

---

## 7. **Putting It All Together**
A **Kubernetes cluster** functions by orchestrating pods within nodes using the control plane's guidance. The control plane makes decisions and delegates tasks, while worker nodes execute these tasks to maintain the desired state of the cluster. This cohesive interaction enables Kubernetes to provide high availability, scalability, and flexibility in deploying and managing containerized applications.

**Visual Summary**: A comprehensive diagram here would show the complete Kubernetes architecture, highlighting the relationship between the control plane, worker nodes, and their components.

---

Understanding these elements gives insight into how Kubernetes ensures smooth, reliable, and scalable management of containerized applications in DevSecOps environments.


### What is the smallest deployable unit of computing you can create in Kubernetes?
pod

### Which control plane component is a key/value store which contains data pertaining to the cluster and its current state?
etcd

### Which worker node component is responsible for network communication within the cluster?
kube-proxy

# Task 4 : Kubernetes Landscape

## The Lay of the Land: Navigating Kubernetes as a DevSecOps Engineer

Having explored the architecture of Kubernetes, it’s time to dive into the key concepts DevSecOps engineers frequently interact with. Below, we break down essential Kubernetes components and their roles in daily operations.

## Namespaces
**Purpose**: Namespaces in Kubernetes help isolate and organize resources within a single cluster.
- **Use Case**: Grouping resources for a specific application component or multi-tenant environments.
- **Uniqueness**: Resource names must be unique within a namespace but can be duplicated across different namespaces.

## ReplicaSet
**Purpose**: Ensures the availability and scalability of a defined number of identical pods.
- **Functionality**: ReplicaSets manage the lifecycle of pods and maintain a specified number of running pod replicas.
- **Management**: Often managed by a deployment rather than defined directly.

## Deployments
**Purpose**: Define and manage the desired state of applications.
- **Declarative Updates**: Enable users to specify the desired state (e.g., a deployment with three nginx pods), and the deployment controller ensures the actual state matches it.
- **Automatic Rollouts and Rollbacks**: Simplify updating and maintaining application versions.

## StatefulSets
**Purpose**: Manage stateful applications that require consistent storage and network identities.
- **Stateful vs. Stateless**: Stateful apps retain data across sessions (e.g., an email client), whereas stateless apps do not (e.g., a search engine).
- **Unique Pod Identity**: Pods in a StatefulSet have persistent unique IDs, ensuring ordered creation and stable storage.
- **Data Synchronization**: Typically, a master pod handles write operations while replica pods synchronize and read data.

## Services
**Purpose**: Provide a stable networking endpoint for accessing pods.
- **Problem Solved**: Pods are ephemeral, with frequently changing IPs. Services assign a consistent IP for reliable access.
- **Types of Services**:
  - **ClusterIP**: Default type, accessible only within the cluster.
  - **NodePort**: Exposes the service on each node’s IP at a static port.
  - **LoadBalancer**: Integrates with cloud providers to distribute traffic externally.
  - **ExternalName**: Maps a service to an external DNS name.

## Ingress
**Purpose**: Act as an entry point to manage external access to services.
- **Routing**: Facilitates complex routing rules to direct traffic to different services within the cluster.
- **Single Access Point**: Centralizes routing configuration for better manageability.

## DevOps vs. DevSecOps in K8s
Understanding Kubernetes basics involves recognizing the division of responsibilities:
- **DevOps Tasks**: Primarily involve building and maintaining clusters.
- **DevSecOps Tasks**: Focus on securing the cluster, ensuring compliance, and integrating security throughout the development cycle.
- **Overlap**: While distinct, there can be shared responsibilities depending on organizational needs.

This foundational knowledge in Kubernetes will assist you as we shift to interacting with and securing these resources in future discussions.

### Which Kubernetes component exposes pods and serves as an access point?
service

### Which Kubernetes component can guarantee the availability of X number of pods? 
replicaset

### What Kubernetes component is used to define a desired state?
deployments

# Task 5 : Kubernetes Configuration

In this section, we will walk through how to configure a Kubernetes deployment and service. This setup includes a **Deployment** that controls a **ReplicaSet**, which manages **Pods** exposed by a **Service**.

## File Format

Kubernetes configuration files are typically written in **YAML** format (though **JSON** can also be used). YAML is preferred due to its readability, but careful attention should be paid to indentation.

## Required Fields in Kubernetes Configuration Files

Each YAML file must contain four key fields:

1. **apiVersion**: Specifies the version of the Kubernetes API used to create the object. The version depends on the object type being defined.
2. **kind**: Identifies the type of object being created (e.g., Deployment, Service, StatefulSet).
3. **metadata**: Contains data used to uniquely identify the object (e.g., name and optional namespace).
4. **spec**: Defines the desired state of the object (e.g., the number of replicas for a Deployment).

## Configuring the Service and Deployment

When defining a **Deployment** and **Service**, it is a best practice to define the **Service** first. This is because when Kubernetes starts a container, it automatically creates environment variables for each service running at that time.

### Example Setup
- **Deployment**: Controls the **ReplicaSet** and defines the pods (e.g., 3 nginx pods).
- **Service**: Exposes the pods, making them accessible from within the cluster or externally.

In the next section, we will dive into an example of the configuration files for both the service and deployment.

```yaml
apiVersion: v1
kind: Service
metadata:
  name: example-nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 8080
      targetPort: 80
  type: ClusterIP
  ```

  Let's break that down: apiVersion is set to v1 (the version of the Kubernetes API best used for this simple service example), and kind is set to service. For the metadata, we just called this service "example-nginx-service". The spec is where it gets more interesting, under 'selector', we have 'app: nginx'. This is going to be important going forward when we define our deployment configuration, as is the ports information, as we are essentially saying here: "This service will look for apps with the nginx label and will target port 80. An important distinction to make here is between the 'port' and 'targetPort' fields. The 'targetPort' is the port to which the service will send requests, i.e., the port the pods will be listening on. The 'port' is the port the service is exposed on. Finally, the 'type' is defined as ClusterIP (earlier, we discussed that there were multiple types of services, and this is one of them), which is the default service type. Now let's take a look at the Deployment YAML and define the back end which this service will point to:


```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: example-nginx-deployment
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
        image: nginx:latest
        ports:
        - containerPort: 80
```

The first thing you might notice is that inside the 'spec' field, there is a nested field called 'template', which itself contains a 'metadata' and 'spec' field. To understand this, remember the image at the start of this task. We are defining a deployment which controls a ReplicaSet; here, in the outer 'spec' field, we tell Kubernetes we want 3 replicas (identical pods) in this ReplicaSet. This template field is the template that Kubernetes will use to create those pods and so requires its own metadata field (so the pod can be identified) and spec field (so Kubernetes knows what image to run and which port to listen on). Note that the port defined here is the same as the one in the service YAML. This is because the service's target port is 80 and needs to match. As well as this, in the outer 'spec' field, you can see we have also set the 'selector' field to have a 'matchLabels', which matches what we defined in the 'selector' field for the service YAML. This is so the service is mapped to the correct pods. With these two config YAML files, we have defined a deployment that controls a ReplicaSet that manages 3 pods, all of which are exposed to a service. It's all coming together!

These config files are used to define the desired state of Kubernetes components; Kubernetes will constantly be checking this desired state against the current state of the cluster. Using the etcd (one of the control plane processes mentioned in an earlier task), Kubernetes populates these configuration files with the current state and does this comparison. For example, if we have told Kubernetes we want 3 nginx pods running, and it detects in the status that there are only 2 running pods, it will begin the actions to correct this.

### In a config file, you have just declared that you want 4 nginx pods. In which one of the 'required fields' has this been declared?
spec

### The configuration file is for a deployment. In which one of the 'required fields' is this declared?
kind

### The pods in this deployment will be exposed by a service. In the service configuration file, the target port was set to 80. What should you put as the 'containerPort'?
80

# Task 6 : Kubectl

# Summary of Kubectl Commands

This document provides a brief overview of key `kubectl` commands used to interact with and manage a Kubernetes cluster.

## 1. Applying Configurations with `kubectl apply`
- Use the `kubectl apply` command to apply YAML configuration files and create running processes in Kubernetes.
```bash
kubectl apply -f example-deployment.yaml
```
## 2. Checking the Status with `kubectl get`
- Use `kubectl get` to check the state of resources like pods, services, and deployments.

```bash
kubectl get pods -n example-namespace
```
## 3. Describing Resources with `kubectl describe`
- The `kubectl describe` command provides detailed information about a specific resource for troubleshooting.
```bash
kubectl describe pod example-pod -n example-namespace
```
## 4. Viewing Logs with `kubectl logs`
- Use `kubectl logs` to view the logs of a specific pod for debugging or security analysis.
```bash
kubectl logs example-pod -n example-namespace
```

## 5. Accessing Container Shell with `kubectl exec`
- The `kubectl exec` command lets you access a container's shell for direct interaction and troubleshooting.
```bash
kubectl exec -it example-pod -n example-namespace -- sh
```

## 6. Port Forwarding with `kubectl port-forward`
- The `kubectl port-forward` command forwards a port from a Kubernetes service to your local machine for testing or access.
```bash
kubectl port-forward service/example-service 8090:8080
```
## Conclusion
These commands are fundamental for working with a Kubernetes cluster and can be used for deployment, monitoring, troubleshooting, and testing.

## What command would follow 'kubectl' if you wanted to....

### ...troubleshoot a pod by gathering some details about it?
describe

### ...access the container's shell?
exec

### ...check the status of running pods?
get

### ...turn a defined configuration (YAML file) into a running process?
apply

# Task 7 : Kubernetes & DevSecOps

As a DevSecOps engineer, understanding how to secure Kubernetes is essential. With Kubernetes rapidly growing in popularity, especially in tech startups, it introduces potential security risks. Let's explore Kubernetes from a security perspective and outline key best practices for securing a Kubernetes cluster.

## Kubernetes Security Risks
Introducing Kubernetes into a system increases security risks as it opens new channels for potential breaches. Kubernetes allows pods to communicate with each other by default, which poses security considerations that need to be managed effectively by a DevSecOps engineer.

## Kubernetes Hardening
Kubernetes hardening is the process of fortifying your cluster using best practices to minimize vulnerabilities. Here are some areas to focus on for securing Kubernetes:

### 1. Securing Pods
- **No root privileges** for containers running applications.
- Containers should have **immutable filesystems**, where changes cannot be made (if applicable).
- Regular **container image scanning** for vulnerabilities or misconfigurations.
- **Privileged containers** should be prevented.
- Implement **Pod Security Standards (PSS)** and **Pod Security Admission (PSA)**.

### 2. Network Hardening
- **Control plane node access** should be restricted using firewalls and Role-Based Access Control (RBAC) in an isolated network.
- Control plane components should communicate over **TLS certificates**.
- Implement an **explicit deny policy**.
- **Credentials and sensitive data** should not be stored as plain text; instead, they should be encrypted and stored in **Kubernetes secrets**.

### 3. Authentication and Authorization
- Disable **anonymous access**.
- Use **strong user authentication**.
- Implement **RBAC policies** for teams and service accounts to limit access to resources.

### 4. Logging and Monitoring
- Enable **audit logging** to track activity.
- Set up a **log monitoring and alerting system** to detect threats early.

### 5. Ongoing Security Maintenance
- **Apply security patches** and updates promptly.
- Perform **regular vulnerability scans** and **penetration tests**.
- Remove **obsolete components** from the cluster to reduce potential attack surfaces.

## Key Kubernetes Security Best Practices

### RBAC (Role-Based Access Control)
RBAC regulates access to Kubernetes resources based on defined roles and permissions. Permissions are granted to users, groups, or service accounts. RBAC can be configured via YAML files, where you define the type of resource and actions (verbs) like "create" or "get".

### Secrets Management
Kubernetes secrets store sensitive information, such as credentials and tokens. By default, secrets are base64 encoded, but for better security:
- **Encrypt secrets at rest**.
- Use **RBAC** to ensure **least privilege access** to secrets.

### Pod Security Admission (PSA) and Pod Security Standards (PSS)
- **Pod Security Standards (PSS)** define three levels of security policies:
  - **Privileged**: Near-unrestricted access (for known privilege escalations).
  - **Baseline**: Minimally restricted, preventing known privilege escalations.
  - **Restricted**: Heavily restricted policies that follow pod hardening best practices.
- **Pod Security Admission (PSA)** enforces these standards by intercepting API server requests and applying policies. Note that **Pod Security Policies (PSP)** were removed as of Kubernetes v1.25, and PSA/PSS now fill their role.

## Conclusion
Securing a Kubernetes cluster involves multiple layers of best practices. By implementing these security measures and utilizing techniques like automated security checks, a DevSecOps engineer can ensure that the Kubernetes environment remains resilient against cyber threats. With continuous updates and vigilance, Kubernetes security becomes a proactive process, not just a one-time effort.


### Which best container security practice is used to regulate access to a Kubernetes cluster and its resources?
RBAC
### What is used to define security policies at 3 levels?
Pod Security Standards
### What enforces these policies?
Pod Security Admission
### What Kubernetes object can be used to store sensitive information and should, therefore, be managed securely?
Secret

# Task 8 : Hands-on with Kubernetes

# Phase One: Explore

Okay, let's get going, shall we? The first thing we want to do is start our Minikube cluster. Once the cluster has started up, you're ready to go! This guided walkthrough will take you through this cluster, reinforcing knowledge taught in the room. That being said, you have a Kubernetes cluster at your disposal, so feel free to explore the cluster and experiment with the different commands you have learned so far!

Let's see what's running already, shall we? We can check this using the following command:

```bash
kubectl get pods -A
```
After running this, you should see a few pods running. These are the default pods present when you first start a Kubernetes cluster (you may recognize some of these names as they represent some of the control plane and worker node processes). Exciting stuff! But how about we make it more exciting by adding a deployment and service into the mix? On the VM you will be able to find some config YAML files located here:

```bash
~/Desktop/configuration
```
There are two config YAMLs here of interest to us: `nginx-deployment.yaml` and `nginx-service.yaml`. Check these configuration files out using the `cat` command:
```bash
cat filename
```
### nginx-deployment.yaml

This is a relatively simple deployment; we can see the desired state has been defined as a single replica pod, inside of which will be a container running an nginx image. From the other lines, we can determine that this pod will be used to run some kind of web app.

### nginx-service.yaml

This, again, is a straightforward nginx NodePort service being defined here. The eagle-eyed among you may have noticed that the 'selector: -> app: ' field matches the app label defined in the deployment, as well as the 'targetPort' matching the 'containerPort' outlined in the deployment. This service exposes the web application running in the pod, which the deployment controls.

To apply the configuration outlined in these YAML files, we use the `kubectl apply` command. Remember to apply the service file first!

```bash
kubectl apply -f nginx-service.yaml
kubectl apply -f nginx-deployment.yaml
```
Verify the replica pod is running by using the following command (not providing any namespace flag will get all the pods in the default namespace, which is where our pod should be):
```bash
kubectl get pods -A
```
You should now see a pod with 'nginx-deployment' in its name! Our deployment has been started!

# Phase Two: Interact

Okay, now we have a web application running in a pod, which is being exposed by a service. If you recall the `nginx-service.yaml`, the service connects to the web application using the target port of port 80 (the port where the container is exposed). However, the service's port itself is set to 8080. We want to access this service. We will do this by using the `kubectl port-forward` command, which allows us to forward a port on our localhost to the Kubernetes service port (8080).

After running this command, open a web browser (Firefox) and access the web application at the following address:

 `http://localhost:8090/`

Looks like a simple login terminal, but it needs credentials. Why don't we see if there are any Kubernetes secrets on the cluster that can help us log in to the terminal? Open another terminal window (so the previous window continues to port-forward) and run the following command to see if there are any secrets (in the default namespace):

```bash
kubectl get secrets
```
Ahh, there is! "terminal-creds" sounds like we are onto a winner! Using the `kubectl describe` command, we can get more details on this secret to see what is being stored here:

```bash
kubectl describe secret terminal-creds
```
In the description, we can see that two pieces of "Data" are being stored: a username and a password. While Kubernetes secrets are stored in plaintext and not encrypted by default, they are base64 encoded, so we pipe this command and base64 decode the output to get it in plain text.

To access this data, we can use the following commands:

To get the username:
```bash
kubectl get secret terminal-creds -o jsonpath='{.data.username}' | base64 --decode
```
To get the password:

```bash
-kubectl get secret terminal-creds -o jsonpath='{.data.password}' | base64 --decode
```

Use these credentials to access the login terminal. That's a bingo! We're in, and you have retrieved the flag!

### Bonus Task: 

For those curious enough, you can use an alternate method to get this flag. It will require some Kubernetes investigation on your part, but the first breadcrumb lies in the `nginx-deployment.yaml`!

# Phase Three: Secure

It's great that we could access the terminal using the credentials stored in the Kubernetes secret, but as a DevSecOps engineer, this is where our alarm bells should be going off. Time for us to get to work with some Kubernetes secret management. With these credentials being sensitive information, we want to restrict access to the Kubernetes secret they are stored in. We can do this by configuring RBAC (Role-Based Access Control).

First things first, let's decide who it is we want to be able to access this secret. Your DevSecOps manager has suggested that you restrict access to a service account, which is essentially an identity a pod can assume to interact with the Kubernetes API/cluster. By doing this, we can maybe even set it up so that in the future, our daily terminal tasks can be run by an application in a pod.

Let's use the `kubectl create serviceaccount` (can be abbreviated to 'sa') command to make two service accounts, the 'terminal-user' for non-admin terminal activities (should not have access to secret) and the 'terminal-admin' for admin terminal activities (should have access to secret).

Run these two commands to make those service accounts:

```bash
kubectl create sa terminal-user
kubectl create sa terminal-admin
```
With those service accounts created, it's time to restrict access to the 'terminal-creds' secret so that only the 'terminal-admin' service account can access it. We are going to do this by defining and applying two configurations.

### Role YAML (`role.yaml`)

Here, you can see we define a role named "secret-admin". In the rules section, we define what it is this role can do. We define the resource (secrets), what verbs are being restricted (we are restricting the 'get' verb, but you could restrict others), and finally, the name of our secret (terminal-creds).

### Role Binding YAML (`role-binding.yaml`)

In this YAML, we bind the 'terminal-admin' service account (in the 'subjects' section) with the 'secret-admin' role defined above (in the `roleRef` section).

Let's now apply these configurations the same way we applied the deployment and service (using `kubectl apply`):
```bash
kubectl apply -f role.yaml
kubectl apply -f role-binding.yaml
```
You have now configured RBAC for this Kubernetes secret! The only thing left to do is test whether our RBAC is working. We can do this using the `kubectl auth` command, which tells us if a service account has sufficient permission to perform a specific action.

Let us first verify that the regular 'terminal-user' service account CAN NOT access the secret:

```bash
kubectl auth can-i get secret/terminal-creds --as=system:serviceaccount:default:terminal-user
```
It looks like we expected. This "no" response confirms that this service account can no longer access the terminal-creds secret. Now, finally, let us verify that our 'terminal-admin' service account CAN access it:
```bash
kubectl auth can-i get secret/terminal-creds --as=system:serviceaccount:default:terminal-admin
```
With this "yes" output, you have confirmed RBAC is in place and fulfilled your duty as a DevSecOps engineer, fortifying the cluster and taking a good first step into hardening this cluster.

Hope you've enjoyed taking a little tour around this Kubernetes cluster and getting to know the basics. Until next time!

### Can you master the basics of Kubernetes and retrieve the flag?
THM{k8s_k3nno1ssarus}

### What apiVersion is used for the RoleBinding?
rbac.authorization.k8s.io/v1
