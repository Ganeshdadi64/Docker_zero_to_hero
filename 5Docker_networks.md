# 🐳 Docker Networking – Complete Guide (Easy + Practical)

This document explains **why Docker networking is required**, its **types**, and **how to use it with real examples**.

---

# 📌 1. Why Docker Networking?

## 🔹 Real Scenario

Assume we have two containers:

* 🟢 **App Container** (Node.js / Spring Boot)
* 🔵 **DB Container** (MySQL / MongoDB)

👉 The App needs to connect to DB to fetch/store data.

---

## 🔹 Problem Without Docker Network

By default:

* Each container gets a **dynamic IP**
* IP changes if container restarts or recreates

👉 Example problem:

```text
App tries to connect DB using IP: 172.17.0.2
After restart, DB IP becomes: 172.17.0.5 ❌
```

👉 Result:

* Application breaks
* Connection fails

---

## 🔹 Solution → Docker Network

👉 Instead of IP, use **container name**

```text
App → connect to "db-container"
```

👉 Docker DNS resolves automatically ✅

---

# 📌 2. What is Docker Network?

Docker Network is used to:

* Enable communication between containers
* Isolate container traffic
* Provide internal DNS (name-based communication)

---

# 📌 3. Types of Docker Networks

---

## 🔹 1. Bridge Network (Default)

👉 Used for containers on **same host**

* Default network created by Docker
* Containers can talk using **IP or name (custom bridge)**

### ✅ Example

```bash id="9xt1kp"
docker network create mybridge

docker run -dit --name app --network mybridge nginx
docker run -dit --name db --network mybridge redis
```

👉 Test connection:

```bash id="c3lg6o"
docker exec -it app bash
apt update && apt install iputils-ping -y
ping db
```

👉 Works using container name ✅

---

## 🔹 2. Host Network

👉 Container uses **host machine network directly**

* No separate IP
* Same IP as EC2 / host

### ✅ Example

```bash id="1rxyzz"
docker run -dit --network host nginx
```

👉 Access directly:

```text
http://<host-ip>:80
```

---

## 🔹 3. None Network

👉 No network at all ❌

* No internet
* No communication

### ✅ Example

```bash id="dqdwpf"
docker run -dit --network none ubuntu
```

👉 Use case:

* Security testing
* Isolated environments

---

## 🔹 4. Overlay Network

👉 Used in **Docker Swarm / Multi-host**

* Containers communicate across **multiple servers**

### ✅ Example

```bash id="o2nqow"
docker network create -d overlay myoverlay
```

👉 Used in:

* Microservices
* Distributed systems

---

# 📌 4. Creating & Managing Networks

## 🔹 Create Network

```bash id="w3fpbm"
docker network create mynetwork
```

---

## 🔹 List Networks

```bash id="0pfz3m"
docker network ls
```

---

## 🔹 Inspect Network

```bash id="0r7l0r"
docker network inspect mynetwork
```

---

## 🔹 Delete Network

```bash id="0v9fxg"
docker network rm mynetwork
```

---

## 🔹 Remove Unused Networks

```bash id="0h9t6c"
docker network prune
```

---

# 📌 5. Connect Container to Network

## 🔹 Step 1: Create Container

```bash id="6k5ozr"
docker run -dit --name cont1 ubuntu
```

---

## 🔹 Step 2: Connect to Network

```bash id="pj8r3r"
docker network connect mynetwork cont1
```

---

## 🔹 Step 3: Disconnect

```bash id="7v0j9p"
docker network disconnect mynetwork cont1
```

---

# 📌 6. Real-Time Example (App + DB Communication)

## 🔹 Step 1: Create Network

```bash id="x4xj7o"
docker network create appnet
```

---

## 🔹 Step 2: Run DB Container

```bash id="p3tzwu"
docker run -dit --name db --network appnet redis
```

---

## 🔹 Step 3: Run App Container

```bash id="pt9y7n"
docker run -dit --name app --network appnet ubuntu
```

---

## 🔹 Step 4: Test Communication

```bash id="33jkmn"
docker exec -it app bash
apt update && apt install iputils-ping -y
ping db
```

👉 App successfully connects to DB using name ✅

---

# 📌 7. Important Concepts

## 🔹 Container DNS

👉 Docker automatically provides DNS:

```text
container_name → IP address
```

---

## 🔹 Network Isolation

👉 Containers in different networks:

* ❌ Cannot communicate
* ✅ Provides security

---

## 🔹 Default Bridge vs Custom Bridge

| Feature       | Default Bridge | Custom Bridge |
| ------------- | -------------- | ------------- |
| DNS support   | ❌ No           | ✅ Yes         |
| Communication | Limited        | Full          |
| Recommended   | ❌ No           | ✅ Yes         |

---

# 📌 8. Real-Time Use Cases

* Microservices communication
* App ↔ DB connection
* Backend ↔ frontend
* Secure internal networks

---

# 📌 9. Best Practices

* Always use **custom bridge network**
* Use **container names instead of IPs**
* Avoid default bridge in production
* Use overlay for multi-host setups

---

# 🚀 Final Summary

* Docker network enables container communication
* Avoid using IPs → use container names
* Types:

  * Bridge → same host
  * Host → direct host network
  * None → no network
  * Overlay → multi-host
* Use custom networks for better control

---

# 🔥 Interview One-Liner

👉
**“Docker networking allows containers to communicate using stable names instead of dynamic IPs, ensuring reliable communication between services like application and database containers.”**

---
