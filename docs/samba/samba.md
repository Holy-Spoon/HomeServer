# Samba Share Setup

## Goal

Allow other devices on the local network to access files stored on the server.

## Initial Setup

Created a shared directory:

/home/username/shared

Installed Samba:

sudo apt install samba

## Configuration

Added a share to smb.conf:

[shared]
path = /home/username/shared
browseable = yes
read only = no



## Future Improvements

- Create dedicated Samba user
- Move share to dedicated storage drive
- Add backup automation
