# Linux Administration Guide
## User Management, Groups, Roles & Permissions

---

## Overview

### Core Concepts
- **User** → A human being who accesses the system
- **Groups** → A collection of users organized as a team
- **Roles** → Position or function assigned to users
- **Permissions** → Access rights granted to users/groups for system resources

---

## User and Group Management Commands

### Basic Operations
1. **Create user**
2. **Create group**
3. **Add user to group**
4. **Remove user from group**
5. **Remove user**
6. **Remove group**

### Practical Example Workflow
1. Create `devops` group
2. Create `jilani` user
3. Add `jilani` to `devops` group
4. Remove `jilani` from `devops` group
5. Remove `jilani` user

---

## Command Reference

### User Management
```bash
# Create a new user
useradd jilani

# Create a new group
groupadd devops

# Set password for user (run as administrator)
passwd jilani
```

### Group Management
```bash
# Add user to primary group
usermod -g <group-name> <user-name>

# Add user to secondary group (append mode)
usermod -aG <group-name> <user-name>

# Remove user from group
gpasswd -d <user-name> <group-name>

# Delete user
userdel <user-name>

# Delete group
groupdel <group-name>
```

---

## Important System Files

### User and Group Information
- **User information**: `/etc/passwd`
- **Group information**: `/etc/group`
- **Password hashes**: `/etc/shadow`

### Key Points
- When creating a user in Linux, a group with the same username is automatically created
- Every user must have **1 primary group** and can have **multiple secondary groups**

---



By default passwardAuthentication to our remote server set as NO.
this configuartion we ne to change in this file /etc/ssh/sshd_config --> SSH configuration
make PasswordAuthentication yes.
## SSH Configuration for Password Authentication

### Configuration File
- **SSH config file**: `/etc/ssh/sshd_config`

### Steps to Enable Password Authentication
1. **Edit SSH configuration** (handle with extreme care):
   ```bash
   vim /etc/ssh/sshd_config
   ```

2. **Change setting**:
   ```
   PasswordAuthentication yes
   ```

3. **Test SSH configuration syntax**:
   ```bash
   sshd -t
   ```

4. **Restart SSH service**:
   ```bash
   systemctl restart sshd
   ```

### ⚠️ **Critical Warning**
- The `/etc/ssh/sshd_config` file is extremely sensitive
- Any syntax errors can lock out ALL users from the system
- Always use `sshd -t` to verify configuration before restarting the service
- Keep a backup session open when making changes

### Remote Login
After configuration, users can log in remotely:
```bash
ssh <user-name>@<ip-address>
```

---

## File Permissions

### Permission Values
| Permission | Value | Description |
|------------|-------|-------------|
| **Read**   | 4     | View file contents |
| **Write**  | 2     | Modify file contents |
| **Execute** | 1     | Run file as program |

### Permission Structure
```
-    rw-    r--    r--
│     │      │      │
│     │      │      └── Other users (o)
│     │      └────────── Group members (g)  
│     └───────────────── Owner/User (u)
└─────────────────────── File type indicator
```

### Symbolic Permission Commands
```bash
# Give write permission to group
chmod g+w <file-name>

# Give execute permission to group
chmod g+x <file-name>

# Give all permissions to everyone
chmod ugo+rwx <file-name>

# Remove execute permission from everyone
chmod ugo-x <file-name>
```

### Numeric Permission System
```bash
# Example: chmod 744 filename
# 7 (4+2+1) = Owner has read+write+execute
# 4 (4+0+0) = Group has read only
# 4 (4+0+0) = Others have read only
chmod 744 <file-name>
```

### Common Permission Combinations
| Numeric | Symbolic | Meaning |
|---------|----------|---------|
| 755     | rwxr-xr-x | Owner: full, Group/Others: read+execute |
| 644     | rw-r--r-- | Owner: read+write, Group/Others: read only |
| 600     | rw------- | Owner: read+write, Group/Others: no access |
| 777     | rwxrwxrwx | Everyone: full permissions (use with caution) |

---

## System Monitoring

### Check Logged-in Users
```bash
# Show currently logged-in users
who

# Show detailed login information
w

# Show last login information
last
```

---

## Best Practices

1. **Always test SSH configuration** with `sshd -t` before restarting
2. **Keep backup administrative access** when modifying SSH settings
3. **Use principle of least privilege** for file permissions
4. **Regularly audit user accounts** and group memberships
5. **Document user management procedures** for team consistency
6. **Use strong passwords** and consider key-based authentication
7. **Monitor system access** through log files and user activity

---

## Troubleshooting Tips

- If locked out after SSH config changes, access via console or recovery mode
- Use `id <username>` to check user's groups and permissions
- Check `/var/log/auth.log` for authentication issues
- Use `getent passwd <username>` to verify user exists in system database