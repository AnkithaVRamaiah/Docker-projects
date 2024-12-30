### Steps for Docker Networking: Testing Different Network Modes

In this exercise, we will walk through various network modes in Docker, including the default bridge network, custom networks, and the host network. We will create containers using **Nginx** images and test their connectivity between each other based on different network configurations. This will help you understand how Docker networks operate and how containers communicate in different scenarios.

---

### **1. Working with the Default Bridge Network**

#### **Objective**: 
We will first create two containers (`login` and `logout`), run them, and check their connectivity using the default **bridge network**. We will also inspect the container's IP addresses and ping between containers.

#### **Steps**:
1. **Run the `login` container**:
   ```bash
   docker run -d --name login nginx:latest
   ```
   - This creates a container named `login` using the Nginx image and runs it in detached mode.

2. **Access the `login` container**:
   ```bash
   docker exec -it login /bin/bash
   ```
   - This will allow you to access the shell of the running `login` container.

3. **Install `iputils-ping` inside the `login` container**:
   ```bash
   apt-get install iputils-ping -y
   ```
   - We need this package to enable the `ping` command inside the container.

4. **Ping the `logout` container**:
   - First, run the `logout` container:
     ```bash
     docker run -d --name logout nginx:latest
     ```
   - Now, use `docker ps` to verify that both containers (`login` and `logout`) are running:
     ```bash
     docker ps
     ```
   - Inspect the `login` container to get its IP address:
     ```bash
     docker inspect login
     ```
   - Similarly, inspect the `logout` container to get its IP address:
     ```bash
     docker inspect logout
     ```
   - In the `login` container shell, try pinging the `logout` container's IP address:
     ```bash
     ping <logout-container-ip>
     ```
   - This will check the connectivity between the two containers using the **default bridge network**.

---

### **2. Creating and Using a Custom Docker Network**

#### **Objective**: 
We will now create a custom Docker network (`secure`) and run a container (`database`) on this network. We will test if containers from the **default bridge network** (e.g., `login`) can ping containers in the **custom network** (`database`).

#### **Steps**:
1. **Create a custom Docker network**:
   ```bash
   docker network create secure
   ```
   - This creates a new custom network called `secure`.

2. **Run a container (`database`) on the `secure` network**:
   ```bash
   docker run -d --name database --network=secure nginx:latest
   ```
   - This starts the `database` container and attaches it to the `secure` network.

3. **Verify containers on the `secure` network**:
   ```bash
   docker ps
   docker inspect database
   ```
   - Use `docker ps` to check if the `database` container is running.
   - Use `docker inspect` to get the IP address of the `database` container.

4. **Ping the `database` container from the `login` container**:
   - As `login` is on the default bridge network, it won't be able to directly ping `database` on the `secure` network. Try pinging the `database` container's IP from the `login` container:
     ```bash
     docker exec -it login /bin/bash
     ping <database-container-ip>
     ```
   - This will fail because containers on different networks (bridge vs. secure) cannot communicate directly unless explicitly configured.

---

### **3. Testing with Host Network Mode**

#### **Objective**: 
We will now test Docker's **host network mode**, where the container shares the network namespace with the host. This will provide a different behavior for network connectivity compared to bridge and custom networks.

#### **Steps**:
1. **Run a container (`host`) using the host network mode**:
   ```bash
   docker run -d --name host --network=host nginx:latest
   ```
   - This runs the container named `host` using the host’s network interface. The container will have the same IP address as the host.

2. **Verify the container is running**:
   ```bash
   docker ps
   docker inspect host
   ```
   - Use `docker ps` to ensure the container is running and `docker inspect` to get details of the container, including its network configuration.

3. **Ping the `host` container from other containers**:
   - Since the `host` container is using the host’s network, other containers in the default bridge network (or any other network) can communicate with it using the host's IP address.
   - Try to ping the `host` container from another container, like `login`:
     ```bash
     docker exec -it login /bin/bash
     ping <host-ip-address>
     ```

---

### **4. Overview of Network Modes**

Here’s a quick summary of the different network modes and what we tested:

- **Bridge Network**: The default network. Containers on this network are isolated from each other, but they can communicate if their IP addresses are known. Containers can access the outside world through NAT (Network Address Translation), but other containers in different networks cannot communicate unless specifically configured.
  
- **Custom Network**: Created with the `docker network create` command. Containers on a custom network can communicate with each other easily, but containers on different networks cannot communicate by default.

- **Host Network**: The container shares the network namespace with the host, meaning it will use the host's IP and can communicate easily with the host and other containers via the host’s IP.

---

### **5. Docker Network Management Commands**

- **Listing networks**:
  ```bash
  docker network ls
  ```
  - This command lists all the available networks in Docker, including default (`bridge`, `host`, `none`) and custom networks (like `secure` in our case).

- **Inspecting networks**:
  ```bash
  docker network inspect <network-name>
  ```


