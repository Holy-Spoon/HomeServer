# Pi-Hole
## Goals
- Learn how a Pi-hole works
- Install Pi-hole
- troublehooting 
- Make it reproduceable

# Troubleshooting 
in dirctory:
/home/username/Docker/pihole
created a file:
compose.yaml
then ran the following command while inside the pihold foler
docker compose up -d
<img width="922" height="115" alt="image" src="https://github.com/user-attachments/assets/e469a5bf-2e48-42f4-9187-5be7e341ba13" />

Issue: 0.0.0.0:53/tcp : address is already in use
Very common issue and mentioned in pihole documnetation:
https://docs.pi-hole.net/docker/tips-and-tricks/
systemd-resolved which is configured by default to implement a caching DNS stub resolver. This will prevent pi-hole from listening on port 53.
So systemd-resolved is listening on port 53 so we have to disable it.
The stub resolver should be disabled with:
```bash
sudo sh -c 'mkdir -p /etc/systemd/resolved.conf.d && printf "[Resolve]\nDNSStubListener=no\n" | tee /etc/systemd/resolved.conf.d/no-stub.conf'
```
This will not change the nameserver settings, which point to the stub resolver thus preventing DNS resolution. Change the /etc/resolv.conf symlink to point to /run/systemd/resolve/resolv.conf, which is automatically updated to follow the ubuntu system's netplan or fedora system's sysconfig:
```bash
sudo sh -c 'rm -f /etc/resolv.conf && ln -s /run/systemd/resolve/resolv.conf /etc/resolv.conf'
```
After making these changes, you should restart systemd-resolved using:
```bash
systemctl restart systemd-resolved
```
