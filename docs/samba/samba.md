# Samba Share Setup

## Goal

Allow other devices on the local network to access files stored on the server.

## Installing Samba:
sudo apt update
sudo apt install samba

check samba is installed:
whereis samba
Output should be: 
samba: /usr/lib/x86_64-linux-gnu/samba /etc/samba /usr/libexec/samba /usr/share/samba /usr/share/man/man7/samba.7.gz
## Setup:

Created a shared directory:
mkdir /home/username/sambashare

## Configuration
### Add new directory as a share:
config files are located in:/etc/samba/smb.conf
we open this file and add our new share directory:
sudo nano /etc/samba/smb.conf

[sambashare]
    comment = Samba on Ubuntu
    path = /home/allknowing/sambashare
    browseable = yes
    read only = no
    guest ok = no
    valid users = username*

Explination of each line:
[samabashare] : name of the share
comment : description of the share
path : directory to our share
browseable : whether users can discover the server 
read only : Set to "yes" users will be unable to create or modfiy files 
            Set to "no" users can create and modify files
guest ok: Set to "Yes" no password is required to connect to the service.
          Set to "No" users are required to enter a username and password 
valid users : restricts access to only specified users (your username) 



## Future Improvements

- Create dedicated Samba user
- Move share to dedicated storage drive
- Add backup automation
