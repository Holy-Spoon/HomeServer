## Known Issues

### iOS Files App

Symptoms:
- Authentication succeeds
- Share appears empty

Tests:
- Verified filesystem permissions with getfacl
- Verified Samba connection using smbstatus
- Confirmed Windows/Linux clients work

Conclusion:
Likely an iOS SMB client issue.

## iOs Files App(Detailed)
Apple iPhone:
Using IOs files -> connect to server -> enter (correct) credneitals -> unexpected error -> 
try again -> logins to server but no files are shown.
This was a replicable problem  

# Tested Permission:
using getfacl (get file access control lists) on the samabashare -> getfacl ~/sambashare
Returns{
  user::rwx
  group::rwx
  other::r-x
}
Every user can read the contents so no problem there

# Connected Users
Next test is to see who can connect:
using smbstatus (report on current Samba connections) as root -> sudo smbstatus
  amba version 4.23.6-Ubuntu-4.23.6+dfsg-1ubuntu2.1
PID     Username     Group        Machine                                   Protocol Version  Encryption           Signing              
----------------------------------------------------------------------------------------------------------------------------------------
27438   username   username   xxx.xxx.xx.x                               SMB3_11           -                    partial(AES-128-GMAC)

Service      pid     Machine       Connected at                     Encryption   Signing     
---------------------------------------------------------------------------------------------
sambashare   27438   xxx.xxx.xx.x  Thu xxx xx 01:15:50 PM 2026 xxxx -            -           
It shows only my username is present.

Incase its a issue with logging in as a guest -> users only allowed to use username*
so to make certain only users with my username and password can asscess I can update
the configs for the samba file:

[sambashare]
    comment = Samba on Ubuntu
    path = /home/username*/sambashare
    browseable = yes
    read only = no
    guest ok = no
    valid users = username*

Since other devices can connect to the sambashare (Windows File Explorer and Ubuntu Files) it might be a iphone problem.
Instead of using IOs Files I'll download a differnt File Explorer.

# Using a Different File Explorer
Downloaded Documents by Readdle
Never mind it costs money so I won't try this route
# TLDR
Windows and Linux based devices can connect to the sambasahre server but IOs devices might not be able to see the contents.
