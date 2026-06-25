# Example Samba Configuration
```ini
[global]
    workgroup = WORKGROUP
    server role = standalone server
    map to guest = never

[sambashare]
    comment = Samba Share
    path = /home/<username>/sambashare

    browseable = yes
    read only = no

    guest ok = no
    valid users = <username>
```
---
Note: to access the configureation:
```ini
sudo nano /etc/samba/smb.conf
```
