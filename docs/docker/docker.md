# Docker

## Goals

* [ ] Understand what Docker does
* [ ] Set up Docker
* [ ] Containerize applications

---

## Installing Docker

Docker was installed using the **APT repository**.

### Installation

* Set up Docker's APT repository
* Installed Docker packages
* Verified that the installation was successful

![Docker installation verification](https://github.com/user-attachments/assets/c106b0f0-a3ff-47e1-a138-a705a8b32685)

---

## Learning About Containerization

### `chroot`

Learned about `chroot` and how it can be used to change the apparent root directory for a process.

### Linux Namespaces

Linux namespaces provide isolation between processes. Different namespace types isolate different system resources.

| Namespace | Flag              | Man Page                | Isolates                             |
| --------- | ----------------- | ----------------------- | ------------------------------------ |
| Cgroup    | `CLONE_NEWCGROUP` | `cgroup_namespaces(7)`  | Cgroup root directory                |
| IPC       | `CLONE_NEWIPC`    | `ipc_namespaces(7)`     | System V IPC, POSIX message queues   |
| Network   | `CLONE_NEWNET`    | `network_namespaces(7)` | Network devices, stacks, ports, etc. |
| Mount     | `CLONE_NEWNS`     | `mount_namespaces(7)`   | Mount points                         |
| PID       | `CLONE_NEWPID`    | `pid_namespaces(7)`     | Process IDs                          |
| Time      | `CLONE_NEWTIME`   | `time_namespaces(7)`    | Boot and monotonic clocks            |
| User      | `CLONE_NEWUSER`   | `user_namespaces(7)`    | User and group IDs                   |
| UTS       | `CLONE_NEWUTS`    | `uts_namespaces(7)`     | Hostname and NIS domain name         |

Namespaces can be composed together to provide multiple layers of isolation.

### Putting It Together

The `rootfs` extracted from an image during the `chroot` section can be used with a low-level container runtime such as `runc`.

```bash
sudo runc run -b bundle container
```

We can then inspect the system namespaces:

```bash
sudo lsns | grep bash
```

Example output:

```text
4026532499 mnt         1  6409 root   /bin/bash
4026532500 uts         1  6409 root   /bin/bash
4026532504 ipc         1  6409 root   /bin/bash
4026532505 pid         1  6409 root   /bin/bash
4026532511 net         1  6409 root   /bin/bash
```

`runc` has created several namespaces for the container:

* Mount (`mnt`)
* UTS (`uts`)
* IPC (`ipc`)
* PID (`pid`)
* Network (`net`)

> **Source:** [Demystifying Containers — Part I: Kernel Space](https://medium.com/@saschagrunert/demystifying-containers-part-i-kernel-space-2c53d6979504)

---

## Containerizing an Application

Cloned the Docker getting-started application:

```bash
git clone https://github.com/docker/getting-started-app.git
```

### Building the Application Image

Docker uses a **Dockerfile** to build container images.

A Dockerfile is a text file containing instructions that Docker uses to construct an image.

Inside the `getting-started-app` directory, create a file named `Dockerfile`:

```dockerfile
# syntax=docker/dockerfile:1

FROM node:24-alpine
WORKDIR /app
COPY . .
RUN npm install --omit=dev
CMD ["node", "src/index.js"]
EXPOSE 3000
```

### Build the Image

```bash
docker build -t getting-started .
```

The `-t` flag assigns the image the name `getting-started`.

### Run the Container

```bash
docker run -d -p 127.0.0.1:3000:3000 getting-started
```

The application can now be accessed at:

```text
http://localhost:3000
```

### Check Running Containers

Use `docker ps` to list running containers:

```bash
docker ps
```

Example output:

```text
CONTAINER ID   IMAGE             COMMAND                  CREATED       STATUS       PORTS                      NAMES
df784548666d   getting-started   "docker-entrypoint.s…"   2 minutes ago  Up 2 minutes  127.0.0.1:3000->3000/tcp   priceless_mcclintock
```

---

## Updating an Application

TODO

---

## Sharing an Application

TODO

---

## Persisting Data

TODO

---

## Bind Mounts

TODO

---

## Multi-Container Applications

TODO

---

## Docker Compose

TODO

---

## Image Building Best Practices

TODO

---

## What's Next

TODO
