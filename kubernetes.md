### Kubernetes Concepts

**Kubernetes**, often abbreviated as K8s, is an open-source platform designed to automate containerized applications' deployment, scaling, and operation. Think of it as an orchestration tool, managing how containers work together, scale, and recover from failures.

---

### **Core Concepts**

1. **What is Kubernetes?**
   - Kubernetes is like a conductor for an orchestra, ensuring that containerized applications (the musicians) work together in harmony.
   - It dynamically manages workloads by scaling containers, replacing failed ones, and optimizing resource usage.
   - Example: A stock trading app like Robinhood needs to handle millions of trades during market hours but remains idle when markets are closed. Kubernetes scales up resources during high demand and scales them down afterward.

2. **Clusters and Nodes**:
   - **Cluster**: A Kubernetes deployment, consisting of multiple machines.
   - **Nodes**: Machines (physical or virtual) within the cluster that run containerized applications.
     - **Control Plane**: The brain of Kubernetes that manages the cluster, exposes APIs, and coordinates workloads.
     - **Worker Nodes**: Machines that run containers using smaller units called **Pods**.

3. **Pods**:
   - The smallest deployable unit in Kubernetes.
   - A Pod can run one or more tightly coupled containers (e.g., a web server and a logging container).
   - Example:
     - A Pod might run a Nginx container to serve web pages and a sidecar container to log access requests.

4. **Replica Sets**:
   - Ensures a specified number of Pods are running at any given time.
   - Kubernetes automatically replaces failed Pods to maintain the desired state.
   - Example:
     - If you specify 3 replicas of a Pod and one fails, Kubernetes spins up another Pod to replace it.

5. **YAML Configurations**:
   - Developers define the desired state of the cluster using YAML files.
   - Example YAML for an Nginx Deployment:
     ```yaml
     apiVersion: apps/v1
     kind: Deployment
     metadata:
       name: nginx-deployment
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

---

### **How Kubernetes Works**

1. **Control Plane**:
   - Exposes the **API server** for managing the cluster.
   - Stores cluster state information in a key-value database called **etcd**.

2. **Kubelets**:
   - Small agents running on worker nodes to communicate with the control plane and execute commands.

3. **Scaling and Availability**:
   - Kubernetes supports **horizontal scaling** by adding more nodes or Pods as workloads increase.
   - Maintains **high availability** through Replica Sets and automated failover mechanisms.

---

### **Advanced Features**

1. **Secret Management**:
   - Manages sensitive information like API keys and passwords securely.
   - Example:
     ```yaml
     apiVersion: v1
     kind: Secret
     metadata:
       name: db-secret
     type: Opaque
     data:
       username: YWRtaW4=  # Base64 encoded 'admin'
       password: cGFzc3dvcmQ=  # Base64 encoded 'password'
     ```

2. **Persistent Storage**:
   - Kubernetes supports volumes for storing data that persists beyond the life of a Pod.
   - Example:
     ```yaml
     apiVersion: v1
     kind: PersistentVolume
     metadata:
       name: pv-example
     spec:
       capacity:
         storage: 10Gi
       accessModes:
         - ReadWriteOnce
       hostPath:
         path: "/data"
     ```

3. **Load Balancing**:
   - Distributes traffic across multiple Pods.
   - Kubernetes uses Services for exposing Pods to external traffic.
   - Example:
     ```yaml
     apiVersion: v1
     kind: Service
     metadata:
       name: nginx-service
     spec:
       selector:
         app: nginx
       ports:
       - protocol: TCP
         port: 80
         targetPort: 80
       type: LoadBalancer
     ```

---

### **Practical Example**

#### 1. **Deploying a Simple Web Application**
- Define the Deployment:
  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: web-app
  spec:
    replicas: 2
    selector:
      matchLabels:
        app: web
    template:
      metadata:
        labels:
          app: web
      spec:
        containers:
        - name: web
          image: nginx:latest
          ports:
          - containerPort: 80
  ```

- Define a Service for External Access:
  ```yaml
  apiVersion: v1
  kind: Service
  metadata:
    name: web-service
  spec:
    selector:
      app: web
    ports:
    - protocol: TCP
      port: 80
      targetPort: 80
    type: LoadBalancer
  ```

#### 2. **Commands to Deploy**
- Apply the configurations:
  ```bash
  kubectl apply -f deployment.yaml
  kubectl apply -f service.yaml
  ```
- Check the Pods:
  ```bash
  kubectl get pods
  ```
- Access the application:
  - Find the external IP of the service using:
    ```bash
    kubectl get service web-service
    ```

---
Thanks to [Kubernetes 101](https://www.youtube.com/watch?v=PziYflu8cB8) for the visual explanation!
