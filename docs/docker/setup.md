# Docker Setup

After installing Docker there are a few configuration steps that make Docker easier and safer to use.

These are not strictly required to run containers, but they are considered best practice on most Linux systems.

---

# Verify the Installation

The first thing to do is ensure Docker was installed correctly.

Check the installed version:

```bash
docker --version
```

Example output:

```text
Docker version 28.x.x
```

Docker also installs the Docker Compose plugin.

Verify it is available:

```bash
docker compose version
```

---

# Check the Docker Service

Docker runs as a background service (daemon).

Check whether it is running:

```bash
sudo systemctl status docker
```

You should see:

```
Active: active (running)
```

If Docker is not running:

```bash
sudo systemctl start docker
```

---

# Enable Docker at Boot

By default, Docker should start automatically whenever Ubuntu boots.

To ensure this is enabled:

```bash
sudo systemctl enable docker
```

This creates a systemd service that launches Docker during system startup.

Without this, containers cannot start after a reboot until Docker itself is started.

---

# Test Docker

Run Docker's official test container.

```bash
sudo docker run hello-world
```

Docker will:

1. Search for the image locally.
2. Download it if necessary.
3. Create a container.
4. Execute it.
5. Print a success message.
6. Stop the container.

If you see the "Hello from Docker!" message then Docker is functioning correctly.

---

# Running Docker Without sudo

By default Docker requires administrator privileges because the Docker daemon communicates through a Unix socket owned by the root user.

Instead of typing:

```bash
sudo docker ps
```

every time, you can add your user to the Docker group.

```bash
sudo usermod -aG docker $USER
```

Log out and back in (or reboot) for the changes to take effect.

You can then verify your permissions:

```bash
docker run hello-world
```

without using `sudo`.

## Why?

Linux controls access to resources through users and groups.

Docker creates a group named:

```
docker
```

Any user in this group can communicate directly with the Docker daemon.

### Security Warning

Members of the Docker group effectively have root-level privileges.

This is because Docker containers can mount the host filesystem and perform privileged operations.

Only add trusted users to the Docker group.

---

# Verify the Docker Service

Check that Docker is enabled:

```bash
systemctl is-enabled docker
```

Expected output:

```
enabled
```

Check that Docker is currently running:

```bash
systemctl is-active docker
```

Expected output:

```
active
```

---

# Check Installed Information

Display Docker system information:

```bash
docker info
```

Useful information includes:

- Docker version
- Storage driver
- Logging driver
- Number of images
- Number of containers
- CPU information
- Memory information

This command is useful when troubleshooting Docker installations.

---

# Verify Docker Compose

Docker Compose is included with modern Docker installations.

Check its version:

```bash
docker compose version
```

If this command works, Compose is installed correctly.

---

# Optional Cleanup

The hello-world image is no longer needed.

View downloaded images:

```bash
docker images
```

Remove it:

```bash
docker image rm hello-world
```

---

# Final Checklist

- [ ] Docker installed
- [ ] Docker service running
- [ ] Docker enabled on boot
- [ ] Hello World container executed
- [ ] User added to Docker group
- [ ] Docker Compose verified
- [ ] Installation verified using `docker info`
