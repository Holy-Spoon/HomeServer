# Samba Share Setup

## Goal

Allow devices on the local network to access files stored on the server using Samba.

---

## Installing Samba

Update package lists and install Samba:

```bash
sudo apt update
sudo apt install samba
```

Verify Samba is installed:

```bash
whereis samba
```

Example output:

```text
samba: /usr/lib/x86_64-linux-gnu/samba /etc/samba /usr/libexec/samba /usr/share/samba
```

---

## Creating the Shared Directory

Create a directory that will be shared over the network:

```bash
mkdir ~/sambashare
```

Verify it exists:

```bash
ls -ld ~/sambashare
```

---

## Configuring Samba

Samba configuration files are located at:

```text
/etc/samba/smb.conf
```

Open the configuration file:

```bash
sudo nano /etc/samba/smb.conf
```

Add the following share definition:

```ini
[sambashare]
    comment = Samba Share
    path = /home/username/sambashare
    browseable = yes
    read only = no
    guest ok = no
    valid users = username
```

### Configuration Options

| Option         | Purpose                                |
| -------------- | -------------------------------------- |
| `[sambashare]` | Name of the share visible to clients   |
| `comment`      | Description of the share               |
| `path`         | Directory being shared                 |
| `browseable`   | Whether clients can discover the share |
| `read only`    | Controls write access                  |
| `guest ok`     | Allows anonymous access if enabled     |
| `valid users`  | Restricts access to specified users    |

More can found using: 
```ini
man smb.conf
```                 
---

## Restarting Samba

After saving the configuration, restart the Samba service:

```bash
sudo systemctl restart smbd
```

Verify it is running:

```bash
sudo systemctl status smbd
```

---

## Firewall Configuration

Allow Samba traffic through the firewall:

```bash
sudo ufw allow samba
```

Verify the rule:

```bash
sudo ufw status
```

---

## Creating a Samba User

Samba uses its own password database.

Create a Samba password for an existing Linux user:

```bash
sudo smbpasswd -a username
```

Enable the account:

```bash
sudo smbpasswd -e username
```

---

## Testing the Configuration

Validate the Samba configuration:

```bash
testparm
```

Expected result:

```text
Loaded services file OK.
Server role: ROLE_STANDALONE
```

Check active Samba connections:

```bash
sudo smbstatus
```

---

## Finding the Server Address

Determine the server IP address:

```bash
hostname -I
```

Example:

```text
192.168.1.100
```

---

## Connecting to the Share

### Ubuntu

Open Files and connect to:

```text
smb://192.168.1.100/sambashare
```

### Windows

Open File Explorer and enter:

```text
\\192.168.1.100\sambashare
```

### iPhone / iPad

Files → Browse → Connect to Server

Enter:

```text
smb://192.168.1.100
```

Authenticate using your Samba username and password.

---

## Troubleshooting

### Verify Permissions

```bash
getfacl ~/sambashare
```

### Verify Samba Status

```bash
sudo smbstatus
```

### Check Configuration Syntax

```bash
testparm
```

### View Service Logs

```bash
sudo journalctl -u smbd
```

---

## Future Improvements

1. Move share to dedicated storage drive
2. Configure automatic backups
3. Create dedicated Samba user
4. Enable auditing/logging
5. Configure read-only shares
