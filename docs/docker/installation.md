# Docker Installation

## Goal

Install Docker Engine on my Ubuntu home server using a method that is:

* Officially supported
* Easy to maintain
* Suitable for a long-running server
* Commonly used in production environments
* Helps me understand how Docker integrates with Linux

---

# Remove Conflicting Packages

Before installing Docker, any unofficial or conflicting packages should be removed.

Possible conflicting packages include:

* docker.io
* docker-compose
* docker-compose-v2
* docker-doc
* podman-docker

Remove them using:

```bash
sudo apt remove $(dpkg --get-selections docker.io docker-compose docker-compose-v2 docker-doc podman-docker containerd runc | cut -f1)
```

Removing these packages helps avoid version conflicts with Docker's official packages.

---

# Evaluating the Installation Methods

Docker provides four official installation methods.

Rather than immediately choosing one, I wanted to understand the trade-offs between each option.

## Option 1 - Docker Desktop

### What is it?

Docker Desktop is a graphical application that bundles Docker Engine with additional developer tools.

```
Docker Desktop
        │
        ▼
 Docker Engine
        │
        ▼
  Containers
```

It includes:

* Docker Engine
* Docker CLI
* Docker Compose
* Desktop GUI
* Kubernetes (optional)
* Extensions
* Automatic updates

### Advantages

* Very beginner friendly
* Little terminal knowledge required
* Easy container management
* Built-in tooling
* Automatic updates

### Disadvantages

* Installs much more than just Docker Engine
* Uses additional system resources
* Encourages GUI usage instead of learning Docker commands
* Rarely installed on Linux servers

### Why I didn't choose it

My goal is to build a Linux home server and understand how Docker works internally.

Docker Desktop hides many of the concepts I want to learn, such as the Docker daemon, images, networking, volumes, and command-line management.

It is an excellent tool for desktop application development but doesn't match the goals of this project.

---

## Option 2 - Docker APT Repository

### What is it?

Docker provides an official APT repository containing the latest supported Docker packages.

Ubuntu's package manager downloads Docker directly from Docker's repository.

```
Docker Repository
        │
        ▼
       APT
        │
        ▼
 Docker Engine
        │
        ▼
   Containers
```

### Advantages

* Official installation method
* Integrates with Ubuntu's package manager
* Easy updates using `apt upgrade`
* Automatic dependency management
* Secure package verification
* Widely used on Linux servers
* Matches most online documentation

### Disadvantages

* Slightly longer installation process
* Requires adding Docker's repository and signing key

### Why I chose it

This method best matches the purpose of this project.

I want to learn how Docker integrates with Linux rather than simply getting Docker running.

Using the APT repository also teaches several Linux administration concepts:

* Software repositories
* GPG package signing
* Package management
* Dependency resolution
* System services

Since this home server will run continuously, having Docker update alongside the rest of the operating system also makes long-term maintenance much easier.

---

## Option 3 - Manual Installation

### What is it?

Instead of using a repository, Docker packages are downloaded manually and installed individually.

```
Docker Website
        │
Download Packages
        │
        ▼
Docker Engine
```

### Advantages

* Complete control over installed versions
* Useful for offline systems
* Good for environments that require specific Docker versions

### Disadvantages

* Updates must be performed manually
* Higher chance of dependency problems
* More maintenance over time

### Why I didn't choose it

Although this method provides the greatest level of control, it also creates unnecessary maintenance for a home server.

My objective is to learn Docker, not manually manage package updates.

I may revisit this method later to better understand package management, but it isn't the best choice for a long-running server.

---

## Option 4 - Convenience Script

### What is it?

Docker provides a shell script that automatically installs Docker.

The script:

* Detects the Linux distribution
* Adds Docker's repository
* Installs dependencies
* Installs Docker Engine

Everything happens with a single command.

### Advantages

* Very quick installation
* Good for testing
* Useful for disposable virtual machines

### Disadvantages

* Installation steps are hidden
* Harder to troubleshoot
* Less educational

### Why I didn't choose it

One of the goals of this project is understanding every installation step.

Running a script would install Docker successfully, but I would learn very little about what the script actually changed on the system.

I would rather perform each step manually so I know exactly how Docker is configured.

---

# Final Decision

I chose the **APT repository installation** because it provides the best balance between learning, maintainability, and real-world usage.

It is the standard installation method for Ubuntu servers, integrates with the operating system's package manager, and exposes the Linux concepts I want to understand.

Most importantly, it supports the primary goal of this project:

> Build a home server while understanding every component instead of treating it as a black box.
