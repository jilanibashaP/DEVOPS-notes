# Differece between the windows and Linux

Windows
-------------------
1) Heavy graphics
2) More cost
3) slow
4) heavy resource consumption
5) performance -> frequest restarts ( other wise laptop will become stuck)
6) security -> Anti virus installed is must
7) upgrate to other versions is little tough


Linux
--------------------------
1) open source
2) freee
3) no graphics, only text
4) supper fast
5) less resources -> 9mb of the OS can be installed
6) run for years with out restarts
7) By default security provided in OS
8) upgrate is easy



AWS Regions and Avaliability Zones
----------------------------------------------
1) North virginia is the first AWS region . so cost is very less compared to any other region. 
     us-east-1

2) Avaliability zones (Data centers) => meaning our servers are present over there.
    -> Every regain have atleat 2 avliabity zones for backeup purpose.
    -> In HYD 3 data cenetrs are there.



Launch instance
------------------------
 => AMI (amazon machine image) we need to select i.e OS.

 1) we need to create firewall i.e security groups
 2) we need to generate the SSH keys  by using this command (ssh-keygen -f <file-name>)
 3) then launch the instance


Firewall and authentication
----------------------------

## Authentication

1) What you know?   -> User and password
2) What you have?   -> username  , RSA/OTP/ Goggle auth/ microsoft auth
3) What you are?    -> fingure prints, retina, palm, etc.


# Linix Server , How to connect?
  -> we need clint software -> putty, mobaxterm, gitbash, windows CMD, Mac terminal.

  -> Git bash is one of the SSH client.

  -> Git bash you can run the linux commands in windows.

  windows -> \  (backward)
  linux -> / (forward)


# generating the SSH key

  => ssh-keygen -f <file-name>    (file-name is nothing but your ssh key file).

        -> By this command we will get the 2 files sored in our directory 
            1) private and public key
        -> we need to give public key to aws while creating the instance.

Firewall setup == security groups
----------------------
-> you can think of AWS Security Groups as a kind of virtual firewall,
-> They control inbound and outbound traffic to AWS resources (like EC2, RDS, Lambda with VPC).
✅ Similar to a firewall: Controls who can talk to your server and on what ports.

✅ Easier: You don’t manage complex rules, just define allow rules (no deny rules).
-> Incoming and outgoing  (inbound and outbound trafic)


# How to connect to server using Git Bash

## Server Details
- **Public IP Address**: `13.221.76.80`
- **Username**: `ec2-user` (default)

## SSH Command Format
```bash
ssh -i <private-key file name> ec2-user@<server ip address>
```

## Connection Methods

### Method 1: Using Full Path
If your key file is named `devops-key.pem` and located in `D:\DEVOPS-notes`:

```bash
ssh -i /d/DEVOPS-notes/devops-key.pem ec2-user@13.221.76.80
```

### Method 2: From Key Directory
If you are already in the folder where the key is present:

```bash
ssh -i devops-key.pem ec2-user@13.221.76.80
```

## Note
In Git Bash, navigate to the folder where your private key is stored before running the SSH command.
OR if you are already in the folder where the key is present:

## Quick Reference Commands

```bash
# Basic connection
ssh -i devops-key.pem ec2-user@13.221.76.80

# Full path connection
ssh -i /d/DEVOPS-notes/devops-key.pem ec2-user@13.221.76.80

# Set key permissions
chmod 400 devops-key.pem

# Check current directory
pwd

# List files in current directory
ls -la

# Navigate to key directory
cd /d/DEVOPS-notes
```


# SSH Command Explanation

## Command
```bash
ssh -i devops-key.pem ec2-user@13.221.76.80
```

## What Happens When You Run This Command

### 1. Command Breakdown
- `ssh`: The SSH client program
- `-i devops-key.pem`: Specifies the private key file to use for authentication
- `ec2-user`: The username to connect as
- `13.221.76.80`: The IP address of the target EC2 instance

### 2. Step-by-Step Process

#### Step 1: Local Machine (Client Side)
- SSH client reads the private key from `devops-key.pem`
- Initiates connection to `13.221.76.80` on port 22 (default SSH port)
- Presents the username `ec2-user` for authentication

#### Step 2: Network/Security Layer
- Connection request passes through AWS Security Groups (firewall)
- **Requirement**: Port 22 must be open for your IP address
- If blocked: Connection will timeout or be refused

#### Step 3: EC2 Instance (Server Side)
- SSH daemon (`sshd`) receives the connection request
- Looks for the user `ec2-user` on the system
- Checks `/home/ec2-user/.ssh/authorized_keys` file

#### Step 4: Key Authentication
- Server compares your private key with stored public keys
- **Requirement**: The public key corresponding to `devops-key.pem` must exist in `authorized_keys`
- If keys match: Authentication succeeds
- If keys don't match: Authentication fails

#### Step 5: Connection Result
**Success**: You are logged into the EC2 instance as `ec2-user`
**Failure**: Various error messages (see below)

## Prerequisites for Success

### 1. Private Key File
- `devops-key.pem` must exist and be readable
- File permissions should be `600` (read-write for owner only)
  ```bash
  chmod 600 devops-key.pem
  ```

### 2. Security Group Configuration
- Inbound rule allowing SSH (port 22) from your IP
- Example rule:
  - Type: SSH
  - Protocol: TCP
  - Port: 22
  - Source: Your IP address or 0.0.0.0/0 (less secure)

### 3. Public Key Setup
- The corresponding public key must be in `/home/ec2-user/.ssh/authorized_keys`
- This is typically done when the EC2 instance is launched with the key pair

### 4. EC2 Instance Status
- Instance must be running
- SSH daemon must be running on the instance

## Common Error Scenarios

### Connection Timeout
```
ssh: connect to host 13.221.76.80 port 22: Operation timed out
```
**Cause**: Security group doesn't allow SSH from your IP

### Permission Denied (Public Key)
```
Permission denied (publickey).
```
**Cause**: Private key doesn't match any public key in `authorized_keys`

### Bad Permissions
```
Permissions 0644 for 'devops-key.pem' are too open.
```
**Solution**: Run `chmod 600 devops-key.pem`

### Host Unreachable
```
No route to host
```
**Cause**: Instance is stopped, terminated, or IP address is incorrect

### File Not Found
```
devops-key.pem: No such file or directory
```
**Solution**: Ensure the key file is in the current directory or provide full path

## Successful Connection
When successful, you'll see something like:
```
The authenticity of host '13.221.76.80 (13.221.76.80)' can't be established.
ECDSA key fingerprint is SHA256:...
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '13.221.76.80' (ECDSA) to the list of known hosts.

       __|  __|_  )
       _|  (     /   Amazon Linux 2 AMI
      ___|\___|___|

[ec2-user@ip-172-31-xx-xx ~]$
```

## Best Practices
- Keep private keys secure and never share them
- Use specific IP addresses in security groups instead of 0.0.0.0/0
- Regularly rotate key pairs
- Consider using AWS Systems Manager Session Manager as an alternative to direct SSH





# EC2 SSH File Structure

## Overview
Every user **must** have these files and permissions to establish SSH remote connections to EC2 instances.

## Directory Structure
```
/home/${USERNAME}/
 └── .ssh/
      ├── authorized_keys    # PUBLIC keys
      ├── known_hosts        # optional
      └── config             # optional
```

## Required Permissions
```bash
/home/${USERNAME}                        -> 755 (owned by ${USERNAME})
/home/${USERNAME}/.ssh                   -> 700 (owned by ${USERNAME})
/home/${USERNAME}/.ssh/authorized_keys   -> 600 (owned by ${USERNAME})
```

## Setup Commands
```bash
# Create structure
sudo useradd -m ${USERNAME}
sudo mkdir -p /home/${USERNAME}/.ssh

# Set permissions
sudo chmod 755 /home/${USERNAME}
sudo chmod 700 /home/${USERNAME}/.ssh
sudo chmod 600 /home/${USERNAME}/.ssh/authorized_keys

# Set ownership
sudo chown -R ${USERNAME}:${USERNAME} /home/${USERNAME}
```

## Common Default Usernames
- Amazon Linux: `ec2-user`
- Ubuntu: `ubuntu` 
- CentOS/RHEL: `centos`
- Debian: `admin`


------------------------------------------------------------------------
# Linux User Basics

## User Types
- `$` → **Normal user**
- `#` → **Root user**

---

## Home Directories
- Root user home directory:  
  ```
  /root
  ```

- Normal user home directory:  
  Example (for ec2-user):  
  ```
  /home/ec2-user
  ```

---

## Switching to Root User
Use the following command to switch from a normal user to the root user:
```bash
sudo su
```
This command will change the prompt from `$` to `#`, indicating you are now the root user.

---

## Important Note: `sudo su` vs `sudo su -`

- **`sudo su`** → Switches to root but stays in the current directory
- **`sudo su -`** → Switches to root AND changes to root's home directory (`/root`)

The `-` (hyphen) makes it a **login shell**, which:
- Sources root's environment variables
- Changes to root's home directory
- Loads root's shell configuration files

---

## Additional Useful Commands

### Check Current Status
```bash
# Check current user
whoami

# Check current directory
pwd

# Check user ID and groups
id
```

---

## Visual Prompt Examples
- **Normal user**: `ec2-user@hostname:~$`
- **Root user**: `root@hostname:~#`

---

## Security Best Practices

1. **Prefer `sudo` over `su`**: Use `sudo` for individual commands rather than switching to root permanently
2. **Use `-` for complete environment**: When switching users, use `sudo su -` for a clean environment
3. **Exit root promptly**: Don't stay logged in as root longer than necessary
4. **Check your location**: Use `pwd` to verify your current directory after user switches

---

## Quick Reference

| Command | Description |
|---------|-------------|
| `sudo su` | Switch to root (stay in current dir) |
| `sudo su -` | Switch to root (go to /root) |
| `sudo command` | Run single command as root |
| `whoami` | Show current username |
| `pwd` | Show current directory |
| `exit` | Return to previous user |
