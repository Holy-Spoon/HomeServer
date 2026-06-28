# Goal
- [ ] Understand what Docker does
- [ ] Setup Docker
- [ ] Container apps


# Installing Docker
  -  Installed using apt repository
  -  -  Setup Dockers apt repo
     -  Installed the Docker pages
     -  Verifed sucess 
<img width="644" height="226" alt="image" src="https://github.com/user-attachments/assets/c106b0f0-a3ff-47e1-a138-a705a8b32685" />

# Learning about Containerization 
- Learnt about chroot ()
- namespaces

  Namespace |  Flag    |          Page    |                Isolates|
       ───────────────────────────────────────────────────────────────────────────────────────────────────────────
       Cgroup      CLONE_NEWCGROUP   cgroup_namespaces(7)    Cgroup root directory
       IPC         CLONE_NEWIPC      ipc_namespaces(7)       System V IPC, POSIX message queues
       Network     CLONE_NEWNET      network_namespaces(7)   Network devices, stacks, ports, etc.
       Mount       CLONE_NEWNS       mount_namespaces(7)     Mount points
       PID         CLONE_NEWPID      pid_namespaces(7)       Process IDs
       Time        CLONE_NEWTIME     time_namespaces(7)      Boot and monotonic clocks
       User        CLONE_NEWUSER     user_namespaces(7)      User and group IDs
       UTS         CLONE_NEWUTS      uts_namespaces(7)       Hostname and NIS domain name
  Namespaces are composing are well.
  Putting it all togeather:
  
  Do you remember the rootfs we extracted from the image within the chroot section? We can use a low level container runtime like runc to easily run a container from the rootfs:

> sudo runc run -b bundle container

If we now inspect the system namespaces, we see that runc already created mnt, uts, ipc, pid and net for us:

> sudo lsns | grep bash
4026532499 mnt         1  6409 root   /bin/bash
4026532500 uts         1  6409 root   /bin/bash
4026532504 ipc         1  6409 root   /bin/bash
4026532505 pid         1  6409 root   /bin/bash
4026532511 net         1  6409 root   /bin/bash

I will stop here and we will learn more about container runtimes and what they do, in upcoming blog posts and talks.
  (Copied from https://medium.com/@saschagrunert/demystifying-containers-part-i-kernel-space-2c53d6979504)
# Containerize a Application
Cloned a git repo
 git clone https://github.com/docker/getting-started-app.git
 ## Building app image
 Build the app's image

To build the image, you'll need to use a Dockerfile. A Dockerfile is simply a text-based file with no file extension that contains a script of instructions. Docker uses this script to build a container image.

In the getting-started-app directory,  create a file named Dockerfile with the following contents:
```int
    # syntax=docker/dockerfile:1

    FROM node:24-alpine
    WORKDIR /app
    COPY . .
    RUN npm install --omit=dev
    CMD ["node", "src/index.js"]
    EXPOSE 3000
  ```
Then to build the image:
 docker build -t getting-started .

 Then started the docker:
  docker run -d -p 127.0.0.1:3000:3000 getting-started

  Run the docker ps command in a terminal to list your containers.

 docker ps

Output similar to the following should appear.

CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS                      NAMES
df784548666d        getting-started     "docker-entrypoint.s…"   2 minutes ago       Up 2 minutes        127.0.0.1:3000->3000/tcp   priceless_mcclintock


# Updating a Application
# Share a Application
# Persit DB
# Bind Mounts
# Multi Container Applications
# Docker compose
# Image building Best Practices 
# What's Next 

