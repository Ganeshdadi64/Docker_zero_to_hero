# 🐳 Create Docker Image from Container

This guide explains how to create a **new Docker image from an existing container** using the `docker commit` command.

---

# 📌 1. Overview

In Docker, you can create a new image from a container after making changes inside it. This is useful when you manually modify a container and want to save those changes as a reusable image.

👉 This process is called **committing a container**

---

# 📌 2. Step-by-Step Process

## 🔹 Step 1: Pull and Run Base Image

First, start a container using a base image (e.g., nginx or ubuntu).

```bash
docker run -dit --name cont1 ubuntu
```

---

## 🔹 Step 2: Enter into Container

```bash
docker exec -it cont1 bash
```

---

## 🔹 Step 3: Make Changes Inside Container

Go to `/tmp` directory and create files:

```bash
cd /tmp
touch file1.txt file2.txt
```

---

## 🔹 Step 4: Check Changes (Optional)

From another terminal, check what changes were made:

```bash
docker diff cont1
```

👉 This shows:

* `A` → Added files
* `C` → Changed files
* `D` → Deleted files

---

## 🔹 Step 5: Exit from Container

```bash
exit
```

---

## 🔹 Step 6: Create Image from Container

Use `docker commit` to create a new image:

```bash
docker commit cont1 my_custom_image
```

---

## 🔹 Step 7: Verify Image

```bash
docker images
```

👉 You will see `my_custom_image` in the list

---

## 🔹 Step 8: Create New Container from New Image

```bash
docker run -it my_custom_image bash
```

---

## 🔹 Step 9: Verify Changes

Inside the new container:

```bash
cd /tmp
ls
```

👉 You will see `file1.txt` and `file2.txt` (created earlier)

---

# 📌 3. Important Notes

## ⚠️ docker commit Limitations

* Not recommended for production use
* Does not track changes like Dockerfile
* Hard to maintain and version

---

## ✅ Best Practice

👉 Use **Dockerfile** instead of `docker commit` for production environments

Example:

```dockerfile
FROM ubuntu
WORKDIR /tmp
RUN touch file1.txt file2.txt
```

---

# 📌 4. Real-Time Use Cases

* Quick backup of container state
* Debugging and testing changes
* Learning and experimentation

---

# 🚀 Summary

* `docker commit` → Converts container → image
* `docker diff` → Shows changes in container
* New image can be reused to create containers
* Prefer Dockerfile for production

---
