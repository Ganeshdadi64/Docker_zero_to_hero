```
🔹 What is docker export?

docker export is used to save a running or stopped container’s filesystem into a .tar file.

👉 Think of it like:

Taking a snapshot of container data (files only)
Saving it as a portable archive
📌 Important Points
✅ Exports only filesystem (files & folders)
❌ Does NOT include:
Image history
Metadata
Layers
👉 So it is different from docker save
🔹 Syntax
docker export -o <path-to-tar-file> <container_name_or_id>

Example:

docker export -o mycontainer.tar my_container
🔹 Your Example (Corrected)

You wrote:

touch docker/password/secrets/file1.txt
docker export -o docker/password/secrets/file1.txt container_name

❌ Issue:

docker export creates a .tar file, not .txt

✅ Correct:

mkdir -p docker/password/secrets
docker export -o docker/password/secrets/container_backup.tar container_name
🔹 Step-by-Step Implementation
✅ Step 1: Run a container
docker run -dit --name my_container ubuntu
✅ Step 2: Create some data inside container
docker exec -it my_container bash

Inside container:

echo "Hello Ganesh" > /file1.txt
exit
✅ Step 3: Create directory in host
mkdir -p docker/password/secrets
✅ Step 4: Export the container
docker export -o docker/password/secrets/container_backup.tar my_container

👉 Now your container filesystem is saved as:

container_backup.tar
✅ Step 5: Verify file
ls docker/password/secrets
🔹 Step 6: Import back (Optional)

To recreate container from .tar:

cat docker/password/secrets/container_backup.tar | docker import - my_new_image

Then run:

docker run -it my_new_image bash
🔹 Difference: export vs save
Feature	docker export	docker save
Works on	Container	Image
Includes layers	❌ No	✅ Yes
Includes history	❌ No	✅ Yes
Use case	File backup	Full image backup
🔹 Real-time Use Cases
Backup container data before deletion
Move container filesystem to another system
Debug or inspect container files


```



```
# 🐳 Docker Exec & Resource Limits Guide

This document explains:

* How to use `docker exec`
* How to enter a container
* How to set CPU & memory limits
* How to verify limits

---

# 📌 1. Docker Exec

## 🔹 What is `docker exec`?

`docker exec` is used to **run a command inside a running container**.

👉 It does NOT create a new container
👉 It runs commands inside an existing container

---

## 🔹 Syntax

```bash
docker exec <container_name> <command>
```

---

## 🔹 Examples

### ✅ List files inside container

```bash
docker exec cont1 ls
```

---

### ✅ Create directory inside container

```bash
docker exec cont1 mkdir /devops
```

---

## 🔹 Enter into Container (Interactive Mode)

```bash
docker exec -it cont1 /bin/bash
```

OR

```bash
docker exec -it cont1 bash
```

👉 If bash is not available:

```bash
docker exec -it cont1 sh
```

---

## 🔹 Step-by-Step Demo

### Step 1: Run a container

```bash
docker run -dit --name cont1 ubuntu
```

---

### Step 2: Execute command

```bash
docker exec cont1 ls
```

---

### Step 3: Enter container

```bash
docker exec -it cont1 bash
```

Inside container:

```bash
mkdir devops
cd devops
touch file1.txt
```

---

# 📌 2. Resource Limits in Docker

## 🔹 Why Resource Limits?

To prevent a container from consuming **too much CPU or memory**.

---

## 🔹 Syntax

```bash
docker run -dit \
  --name cont_name \
  --memory=250m \
  --cpus="0.25" \
  image_name
```

---

## 🔹 Explanation

* `--memory=250m` → Limits RAM to **250 MB**
* `--cpus="0.25"` → Uses **25% of one CPU core**

---

## 🔹 Example

```bash
docker run -dit --name limited_cont --memory=250m --cpus="0.25" ubuntu
```

---

# 📌 3. Verify Resource Limits

## 🔹 Check Memory

```bash
docker inspect limited_cont | grep -i memory
```

---

## 🔹 Check CPU

```bash
docker inspect limited_cont | grep -i nano
```

---

## 🔹 Real-Time Monitoring (Recommended)

```bash
docker stats
```

👉 Shows live CPU & memory usage

---

# 📌 4. Important Notes

## ⚠️ Memory Limit

* If container exceeds memory → **Container will be killed (OOM)**

---

## ⚠️ CPU Limit

* CPU is throttled → **Container slows down, not killed**

---

## ⚠️ Limits Can’t Be Easily Changed

* You must recreate the container to change limits

---

# 📌 5. Real-Time Use Cases

* Prevent resource overuse in shared environments
* Useful in production microservices
* Important for Kubernetes (requests & limits)

---

# 🚀 Summary

## ✅ docker exec

* Run commands inside running container
* Use `-it` for interactive shell

## ✅ Resource Limits

* `--memory` → RAM control
* `--cpus` → CPU control
* Monitor using `docker stats`

---

# 🔥 Bonus Tip

Use this command to see everything in detail:

```bash
docker inspect cont_name
```

---


```
