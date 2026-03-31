# 🐳 Docker Volumes – Complete Guide (Deep Dive)

This document explains Docker Volumes in a **clear, practical, and real-world way**.

---

# 📌 1. What is a Docker Volume?

A **Docker Volume** is a mechanism to **persist data outside the container filesystem**.

👉 By default:

* Container data is **lost when container is deleted**
* Volumes solve this problem

👉 Simple definition:

> Volume = **Persistent storage for containers**

---

# 📌 2. Key Concepts (Very Important)

* Volume is **NOT inside container**, it is managed by Docker
* Data persists even after container deletion
* Can be shared between multiple containers
* Decouples storage from container lifecycle

---

# 📌 3. Why We Need Volumes?

## 🔹 Problems without volumes

* Data lost when container stops/removes
* Cannot share data between containers
* Not suitable for databases

## 🔹 Solutions with volumes

* Persistent storage
* Data sharing
* Backup & restore

---

# 📌 4. Types of Volume Mapping

## 🔹 1. Container → Container

```bash
docker run -it --name container1 -v /data ubuntu
docker run -it --name container2 --volumes-from container1 ubuntu
```

👉 Both containers share same data

---

## 🔹 2. Host → Container (Bind Mount)

```bash
docker run -it --name cont1 -v /home/user/data:/app ubuntu
```

👉 Host folder ↔ Container folder

---

# 📌 5. Create Volume from Dockerfile

## 🔹 Dockerfile Example

```dockerfile
FROM ubuntu
VOLUME ["/myvolume"]
```

---

## 🔹 Build Image

```bash
docker build -t myimage .
```

---

## 🔹 Run Container

```bash
docker run -it --name container1 myimage bash
```

Inside container:

```bash
cd /myvolume
touch file1.txt
```

---

## 🔹 Share Volume with Another Container

```bash
docker run -it --name container2 --volumes-from container1 ubuntu bash
```

👉 Now both containers share `/myvolume`

---

## 🔹 Verify Data Sharing

In container2:

```bash
touch /myvolume/file2.txt
```

Now check in container1:

```bash
docker start container1
docker attach container1
ls /myvolume
```

👉 You will see `file2.txt`

---

# 📌 6. Create Volume Using Command

## 🔹 Step 1: Create Container with Volume

```bash
docker run -it --name container3 -v /volume2 ubuntu bash
```

---

## 🔹 Step 2: Add Data

```bash
cd /volume2
touch file1.txt
```

---

## 🔹 Step 3: Share Volume

```bash
docker run -it --name container4 --volumes-from container3 ubuntu bash
```

---

## 🔹 Step 4: Verify

```bash
cd /volume2
ls
```

👉 Shared data visible

---

# 📌 7. Host to Container Volume (Bind Mount)

## 🔹 Step 1: Run Container

```bash
docker run -it --name hostcont -v /home/ec2-user:/raham ubuntu bash
```

---

## 🔹 Step 2: Access Files

```bash
cd /raham
ls
```

👉 Shows host files

---

## 🔹 Step 3: Create File

```bash
touch file1.txt
```

👉 Check in host → file exists

---

# 📌 8. Named Volumes (Best Practice)

## 🔹 Create Volume

```bash
docker volume create volume99
```

---

## 🔹 Use Volume

```bash
docker run -it -v volume99:/my-volume --name container1 ubuntu bash
```

---

## 🔹 Reuse Volume

```bash
docker run -it -v volume99:/my-volume2 --name container2 ubuntu bash
```

👉 Same data shared

---

# 📌 9. Mount Volumes (Modern Way)

```bash
docker run -it \
  --name example1 \
  --mount source=vol1,destination=/vol1 \
  ubuntu
```

👉 Recommended over `-v`

---

# 📌 10. Copy Files from Local to Container

```bash
docker run -it --name cont1 -v "$(pwd)":/my-volume ubuntu
```

👉 Current directory → container

---

# 📌 11. Important Volume Commands

```bash
docker volume ls
docker volume create myvol
docker volume inspect myvol
docker volume rm myvol
docker volume prune
docker system df -v
docker container inspect cont1
```

---

# 📌 12. Key Characteristics (Must Know)

* Volumes persist even after container deletion
* Multiple containers can share same volume
* Changes are reflected in all containers
* Volumes are not part of Docker image
* Cannot easily create volume from running container (must define during run/build)

---

# 📌 13. Real-Time Use Cases

* Database storage (MySQL, MongoDB)
* Log storage
* Sharing files between microservices
* CI/CD pipelines
* Backup and restore

---

# 📌 14. Best Practices

* Use **named volumes** instead of anonymous
* Avoid `--volumes-from` in production (legacy)
* Use `--mount` for clarity
* Separate app & data layers

---

# 📌 15. Docker Registry (Related Concept)

Docker Registry is used to **store and manage Docker images**.

## 🔹 Types of Registry

### Cloud Registries

* Docker Hub (default)
* Google Container Registry (GCR)
* Amazon Elastic Container Registry (ECR)

### Private Registries

* Nexus
* JFrog
* Docker Trusted Registry (DTR)

---

# 🚀 Final Summary

* Volume = Persistent storage
* Data survives container deletion
* Can be shared across containers
* Types:

  * Bind Mount (Host ↔ Container)
  * Named Volume
* Use volumes for production apps

---

# 🔥 Interview Tip

👉 **Volume vs Bind Mount**

| Feature     | Volume     | Bind Mount  |
| ----------- | ---------- | ----------- |
| Managed by  | Docker     | Host        |
| Portability | High       | Low         |
| Use case    | Production | Development |

---
