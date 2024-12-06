### Docker Concepts with Simple Examples

**Docker** is a tool that simplifies software deployment using **containerization**, which ensures that applications run consistently across different environments. Containers are lightweight, portable, and isolate applications from underlying systems, solving common issues like "it works on my machine" and scaling challenges in the cloud.

---

### **Core Concepts**

1. **What is Containerization?**
   - A container packages an application and its dependencies together to run reliably across different environments.
   - Example:
     Imagine you package a web server and its configurations in a container. This ensures it works the same on your local machine and in the cloud.

2. **How Docker Works**:
   - **Docker Engine**: The core component enabling OS-level virtualization.
   - **Docker File**: A blueprint containing instructions for building a Docker image.
   - **Docker Image**: A static snapshot (template) of the application, dependencies, and configurations.
   - **Docker Container**: A running instance of a Docker image.

---

### **Steps to Use Docker**

1. **Create a Docker File**:
   - The Docker file specifies how to build the environment for your app.
   ```dockerfile
   FROM node:14
   WORKDIR /app
   COPY package.json .
   RUN npm install
   COPY . .
   CMD ["node", "server.js"]
   EXPOSE 8080
   ```
   - **Explanation**:
     - `FROM`: Specifies the base image (Node.js in this case).
     - `WORKDIR`: Sets the working directory inside the container.
     - `COPY`: Copies files from your local machine into the container.
     - `RUN`: Executes commands to install dependencies.
     - `CMD`: Specifies the default command to run when the container starts.
     - `EXPOSE`: Opens a port for communication.

2. **Build the Docker Image**:
   ```bash
   docker build -t my-app .
   ```
   - `-t`: Tags the image with a name (`my-app`).
   - `.`: Specifies the current directory as the build context.

3. **Run the Docker Container**:
   ```bash
   docker run -p 8080:8080 my-app
   ```
   - `-p`: Maps the container's port (`8080`) to the host's port.

4. **Stop and Remove Containers**:
   ```bash
   docker stop <container-id>
   docker rm <container-id>
   ```

---

### **Advantages of Docker**

1. **Portability**:
   - Containers can run on any platform supporting Docker (Linux, Windows, macOS).

2. **Resource Efficiency**:
   - Containers share the host OS kernel, unlike virtual machines that need separate OS installations.

3. **Statelessness**:
   - Containers are ephemeral, making them lightweight and easy to replace. Persistent data can be stored using **volumes**.

---

### **Advanced Features**

1. **Docker Compose**:
   - Manage multi-container applications using a single YAML file.
   ```yaml
   version: '3'
   services:
     web:
       build: .
       ports:
         - "8080:8080"
       volumes:
         - ./code:/app
     db:
       image: postgres
       environment:
         POSTGRES_PASSWORD: example
   ```
   - Commands:
     ```bash
     docker-compose up   # Start all containers
     docker-compose down # Stop all containers
     ```

2. **Push Images to a Registry**:
   - Upload your image to **Docker Hub** or other registries:
     ```bash
     docker tag my-app username/my-app
     docker push username/my-app
     ```

3. **Scaling with Kubernetes**:
   - Kubernetes manages containerized applications at scale.
   - Example: If one server fails, Kubernetes can automatically spin up a new container on a healthy server.

---

### **Real-World Example**
Suppose you're deploying a Node.js web application.

1. Create a `Dockerfile`:
   ```dockerfile
   FROM node:14
   WORKDIR /usr/src/app
   COPY package*.json ./
   RUN npm install
   COPY . .
   EXPOSE 3000
   CMD ["npm", "start"]
   ```

2. Build and Run:
   ```bash
   docker build -t node-app .
   docker run -p 3000:3000 node-app
   ```

3. Access the App:
   - Open `http://localhost:3000` in your browser.

4. Use Docker Compose for Backend and Database:
   ```yaml
   version: '3'
   services:
     app:
       build: .
       ports:
         - "3000:3000"
     database:
       image: mysql
       environment:
         MYSQL_ROOT_PASSWORD: example
   ```
   ```bash
   docker-compose up
   ```

---

### **Common Commands**
- View running containers:
  ```bash
  docker ps
  ```
- Stop all containers:
  ```bash
  docker stop $(docker ps -q)
  ```
- Remove all containers:
  ```bash
  docker rm $(docker ps -aq)
  ```
- Pull an image from Docker Hub:
  ```bash
  docker pull nginx
  ```

---
Thanks to [Docker 101](https://www.youtube.com/watch?v=rIrNIzy6U_g) for the visuals!
