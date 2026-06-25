# Known Issues

## iOS Files App - SMB Share Appears Empty

**Status:** Unresolved  
**Severity:** Low  
**Affected Devices:** iPhone / iOS Files App

---

## Summary

The iOS Files app can authenticate successfully with the Samba server, but the shared directory appears empty.

Windows and Linux clients can access the share normally.

---

## Symptoms

- Authentication succeeds using valid Samba credentials.
- The Samba share appears in the iOS Files app.
- After connecting, no files or directories are displayed.
- The issue is reproducible.

Expected:

```
iOS Files App
      |
      v
Samba Share
      |
      v
/home/username/sambashare
```

Actual:

```
iOS Files App
      |
      v
Authentication successful
      |
      v
Empty directory
```

---

# Investigation

## 1. Checked filesystem permissions

Command:

```bash
getfacl ~/sambashare
```

Output:

```
user::rwx
group::rwx
other::r-x
```

Result:

- Owner has full access.
- Group has full access.
- Other users have read and execute access.

Conclusion:

Filesystem permissions do not appear to be the cause.

---

## 2. Checked active Samba connections

Command:

```bash
sudo smbstatus
```

Output:

```
Samba version 4.23.6-Ubuntu-4.23.6+dfsg-1ubuntu2.1

PID     Username     Group      Machine
------------------------------------------------
27438   username     username   xxx.xxx.xx.x
```

Result:

- Samba connection is established.
- User authentication is working.
- Connection is not using guest access.

Conclusion:

The issue is likely occurring after authentication.

---

## 3. Checked guest access configuration

To eliminate guest login issues, Samba was configured to require authentication:

```ini
[sambashare]
    comment = Samba on Ubuntu
    path = /home/username/sambashare
    browseable = yes
    read only = no
    guest ok = no
    valid users = username
```

Result:

No change.

---

# Attempted Solutions

## Alternative File Explorer

Attempted:

- Documents by Readdle

Result:

Not tested because the application required payment.

---

# Current Conclusion

Windows and Linux SMB clients work correctly.

The issue is likely related to:
- iOS Files app SMB compatibility
- SMB protocol negotiation
- iOS handling of Linux Samba shares

Further testing required.

---

# Future Investigation

- Test with another iOS SMB client.
- Check Samba protocol versions.
- Test SMB1/SMB2/SMB3 compatibility settings.
- Capture Samba logs during iOS connection attempt.
