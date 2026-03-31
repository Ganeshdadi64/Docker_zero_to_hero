# 🐳 Dockerfile Components – Quick Guide

This document explains the most important Dockerfile instructions with **simple theory + practical examples**.

---

# 📌 1. FROM

## 🔹 What is `FROM`?

`FROM` is used to define the **base image** for your Docker image.
👉 It must be the **first instruction** in a Dockerfile.

## 🔹 Example

```dockerfile id="z9yq6n"
FROM ubuntu
```

👉 Other examples:

* `nginx`
* `redis`
* `jenkins`

---

# 📌 2. LABEL

## 🔹 What is `LABEL`?

`LABEL` is used to add **metadata** to an image (author, version, description).

## 🔹 Example

```dockerfile id="tsr6q0"
LABEL author="ganesh" \
      email="ganesh@example.com" \
      version="1.0"
```

---

# 📌 3. RUN

## 🔹 What is `RUN`?

`RUN` executes commands **during image build time**.

## 🔹 Example

```dockerfile id="zz7nqg"
RUN apt update && apt install -y nginx
```

👉 Used for installing packages, dependencies, etc.

---

# 📌 4. COPY

## 🔹 What is `COPY`?

`COPY` copies files from **local system → container image**.

## 🔹 Example

```dockerfile id="2y0d0v"
COPY app.py /app/app.py
```

👉 Source = local machine
👉 Destination = inside container

---

# 📌 5. ADD

## 🔹 What is `ADD`?

`ADD` works like COPY but with extra features:

* Can **download files from URL**
* Can **auto-extract tar files**

## 🔹 Example

```dockerfile id="6znw07"
ADD https://example.com/file.tar.gz /app/
```

👉 It will download and extract automatically

---

# 📌 6. EXPOSE

## 🔹 What is `EXPOSE`?

`EXPOSE` defines which port the container will use.

## 🔹 Example

```dockerfile id="h6kg4h"
EXPOSE 8080
```

👉 Examples:

* `80` → nginx
* `8080` → tomcat

---

# 📌 7. WORKDIR

## 🔹 What is `WORKDIR`?

`WORKDIR` sets the **working directory** inside the container.

## 🔹 Example

```dockerfile id="v2q4n6"
WORKDIR /app
```

👉 All commands will run inside `/app`

---

# 📌 8. CMD

## 🔹 What is `CMD`?

`CMD` runs a command when the **container starts**.

## 🔹 Example

```dockerfile id="z3r8xq"
CMD ["nginx", "-g", "daemon off;"]
```

👉 Only one CMD is allowed (last one will run)

---

# 📌 9. ENTRYPOINT

## 🔹 What is `ENTRYPOINT`?

`ENTRYPOINT` defines the **main command** that always runs in the container.

## 🔹 Example

```dockerfile id="2o8vlg"
ENTRYPOINT ["nginx", "-g", "daemon off;"]
```

👉 Unlike CMD, it cannot be easily overridden

---

# 📌 10. ENV

## 🔹 What is `ENV`?

`ENV` sets environment variables inside the container.

## 🔹 Example

```dockerfile id="gpt3a7"
ENV APP_ENV=production
ENV PORT=8080
```

---

# 📌 11. Best Complete Example

```dockerfile id="x6v0k5"
FROM ubuntu

LABEL author="ganesh" version="1.0"

ENV APP_HOME=/app

WORKDIR $APP_HOME

RUN apt update && apt install -y nginx

COPY index.html /app/index.html

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
```

---

# 🚀 Summary

* `FROM` → Base image
* `LABEL` → Metadata
* `RUN` → Execute commands during build
* `COPY` → Copy local files
* `ADD` → Copy + download + extract
* `EXPOSE` → Define ports
* `WORKDIR` → Set working directory
* `CMD` → Default command at container start
* `ENTRYPOINT` → Main command
* `ENV` → Environment variables

---
