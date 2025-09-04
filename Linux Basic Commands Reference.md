# Linux Basic Commands Reference

## Directory Operations

### `mkdir <directory-name>`
Creates a new directory.
```bash
mkdir documents
mkdir -p path/to/nested/directory  # Create nested directories
mkdir dir1 dir2 dir3               # Create multiple directories
```

### `rmdir <directory-name>`
Removes an empty directory.
```bash
rmdir documents
rmdir -p path/to/nested/directory  # Remove nested empty directories
```

**Note**: `rmdir` only works on empty directories. Use `rm -r` to remove directories with content.

## File Operations

### `touch <file-name>`
Creates an empty file or updates the timestamp of an existing file.
```bash
touch file.txt
touch file1.txt file2.txt file3.txt  # Create multiple files
touch "file with spaces.txt"         # Handle filenames with spaces
```

## Additional Essential Commands

### Navigation
- `pwd` - Print working directory (show current location)
- `cd <directory>` - Change directory
- `cd ..` - Go up one directory level
- `cd ~` - Go to home directory
- `cd -` - Go to previous directory

### Listing Files
- `ls` - List files and directories
- `ls -l` - Long format listing with details
- `ls -la` - Include hidden files (starting with .)
- `ls -lh` - Human-readable file sizes

### File Management
- `cp <source> <destination>` - Copy files or directories
- `mv <source> <destination>` - Move or rename files/directories
- `rm <file>` - Remove files
- `rm -r <directory>` - Remove directories and their contents
- `rm -rf <directory>` - Force remove (use with caution!)

### Adding Text to Files
- `echo "text" > file.txt` - Write text to file (overwrites existing content)
- `echo "text" >> file.txt` - Append text to file (adds to end)
- `cat > file.txt` - Write multiple lines (press Ctrl+D to finish)
- `cat >> file.txt` - Append multiple lines (press Ctrl+D to finish)

**Examples:**
```bash
echo "Hello World" > greeting.txt        # Create file with text
echo "Second line" >> greeting.txt       # Add another line
cat > story.txt                          # Write multiple lines
This is line 1
This is line 2
^D                                       # Press Ctrl+D to save
```

### Text Editors
- `nano <file>` - Simple, beginner-friendly editor
- `vim <file>` - Advanced editor (press :q! to quit without saving)
- `gedit <file>` - GUI text editor (if available)

### Viewing File Content
- `cat <file>` - Display entire file content
- `less <file>` - View file content page by page
- `head <file>` - Show first 10 lines
- `tail <file>` - Show last 10 lines

### Getting Help
- `man <command>` - Show manual page for a command
- `<command> --help` - Show command help
- `which <command>` - Show location of command

## Network Commands

### `wget <url>`
Download files from the internet.
```bash
wget https://example.com/file.zip                    # Download file
wget -O newname.zip https://example.com/file.zip     # Save with different name
wget -c https://example.com/largefile.zip            # Resume interrupted download
wget -r https://example.com/directory/               # Download recursively
wget --limit-rate=200k https://example.com/file.zip  # Limit download speed
```

### `curl <url>`
Transfer data from/to servers (more versatile than wget).
```bash
curl https://example.com                              # Display webpage content
curl -O https://example.com/file.zip                 # Download file (keep original name)
curl -o newname.zip https://example.com/file.zip     # Download with new name
curl -L https://example.com/redirect                 # Follow redirects
curl -H "User-Agent: MyApp" https://api.example.com  # Custom headers
curl -X POST -d "data=value" https://api.example.com # POST request with data
curl -I https://example.com                          # Show headers only
```

### Key Differences
- **wget**: Specialized for downloading files, better for recursive downloads
- **curl**: More versatile, supports more protocols, better for API testing and web development

## Text Processing Commands

### `cut` - Extract/Split Text by Delimiter
Extract specific columns or fields from text (like split function in programming).

**By delimiter (most common):**
```bash
cut -d',' -f1 file.csv                    # Extract 1st column from CSV
cut -d',' -f1,3 file.csv                  # Extract 1st and 3rd columns
cut -d':' -f1 /etc/passwd                 # Extract usernames from passwd file
echo "apple,banana,cherry" | cut -d',' -f2  # Extract "banana"
```

**By character position:**
```bash
cut -c1-5 file.txt                        # Extract characters 1-5
cut -c1,3,5 file.txt                      # Extract 1st, 3rd, 5th characters
cut -c5- file.txt                         # Extract from 5th character to end
```

**Examples with sample data:**
```bash
# Sample file: data.txt contains:
# John,25,Engineer
# Mary,30,Doctor
# Bob,28,Teacher

cut -d',' -f1 data.txt        # Output: John, Mary, Bob (names)
cut -d',' -f2 data.txt        # Output: 25, 30, 28 (ages)
cut -d',' -f1,3 data.txt      # Output: John,Engineer; Mary,Doctor; Bob,Teacher
```

**Common use cases:**
- Extract columns from CSV files
- Parse log files
- Extract usernames, IPs, or specific fields from structured text
- Process output from other commands

## Tips
- Use Tab for auto-completion
- Use quotes around filenames with spaces
- Be careful with `rm` commands - deleted files are hard to recover
- Use `ls -la` to see file permissions and hidden files


# VIM Editor - Complete Basic Notes

## What is VIM?

VIM (Vi Improved) is a powerful text editor that works in the terminal. It's an enhanced version of the original Vi editor and is available on virtually all Unix-like systems.

## Why Use VIM?

### Key Benefits
- **Speed & Efficiency**: Edit files entirely with keyboard - no mouse needed
- **Universal Availability**: Pre-installed on most Linux/Unix systems
- **Lightweight**: Uses minimal system resources, starts instantly
- **Powerful**: Can handle large files and complex editing tasks efficiently
- **Remote-Friendly**: Perfect for editing files on servers via SSH
- **Highly Customizable**: Can be tailored to any workflow
- **Stable & Reliable**: Rarely crashes, very dependable
- **Professional Tool**: Used by developers and system administrators worldwide

### VIM vs Normal Text Editors
- **No GUI Required**: Works directly in terminal - no need to open separate applications
- **Faster Workflow**: No switching between mouse and keyboard
- **Direct File Access**: Edit files instantly from command line (`vim filename`)
- **Memory Efficient**: Doesn't load heavy interfaces like GUI editors
- **Consistent Experience**: Same editor on all systems (local/remote)
- **Batch Operations**: Can perform complex edits across multiple files quickly
- **Never Breaks**: GUI editors may fail, but terminal always works

---

## The Three Modes

VIM operates in three distinct modes:

### 1. Normal Mode (Escape Mode)
- **Default mode** when opening a file
- Used for navigation and commands
- Press `Esc` to return to Normal mode from any other mode
- This is where you spend most of your time

### 2. Insert Mode
- Used for typing and editing text content
- Enter from Normal mode using various commands
- Press `Esc` to return to Normal mode

### 3. Command Mode (Colon Mode)
- Enter by typing `:` from Normal mode
- Used for file operations (save, quit, search/replace)
- Execute commands by pressing `Enter`

---

## Getting Started

### Opening Files
```bash
vim filename        # Opens existing file or creates new one
vim                 # Opens vim without a file
vim file1 file2     # Opens multiple files
```

When you open a file, you're automatically in **Normal Mode**.

### Basic Workflow
1. **Open file**: `vim filename`
2. **Enter Insert mode**: Press `i` to start typing
3. **Edit your content**: Type your text
4. **Return to Normal mode**: Press `Esc`
5. **Save and quit**: Type `:wq` and press `Enter`

---

## Entering Insert Mode

From Normal mode, press:
- `i` - Insert at cursor position (most common)
- `a` - Append after cursor
- `o` - Open new line below current line
- `I` - Insert at beginning of line
- `A` - Append at end of line
- `O` - Open new line above current line

**Remember**: Press `Esc` to exit Insert mode and return to Normal mode.

---

## Navigation (Normal Mode)

### Basic Movement
- `h` - Move left
- `j` - Move down
- `k` - Move up  
- `l` - Move right
- Arrow keys also work

### Word Movement
- `w` - Move to next word
- `b` - Move to previous word
- `e` - Move to end of current word

### Line Movement
- `0` - Move to beginning of line
- `$` - Move to end of line

### File Movement
- `gg` - Go to first line of file
- `G` - Go to last line of file

---

## File Operations (Command Mode)

Type `:` to enter Command mode, then:

### Saving and Quitting
- `:w` - Save file (write)
- `:q` - Quit (only works if no unsaved changes)
- `:wq` - Save and quit
- `:q!` - Quit without saving (force quit)
- `:x` - Save and quit (same as :wq)

### Other File Operations
- `:w newname` - Save as new filename
- `:e filename` - Open/edit another file

---

## Basic Editing (Normal Mode)

### Deletion
- `x` - Delete character under cursor
- `dd` - Delete entire current line
- `dw` - Delete word

### Copy and Paste
- `yy` - Copy (yank) current line
- `yw` - Copy word
- `p` - Paste after cursor/below line
- `P` - Paste before cursor/above line

### Undo and Redo
- `u` - Undo last change
- `Ctrl+r` - Redo

---

## Search and Find

### Basic Search
- `/word` - Search forward for "word"
- `?word` - Search backward for "word"
- `n` - Go to next search result
- `N` - Go to previous search result

---

## Essential Tips for Beginners

### Getting Unstuck
- **Lost?** Press `Esc` to get back to Normal mode
- **Can't quit?** Type `:q!` to force quit without saving
- **Made mistake?** Press `u` to undo

### Practice Commands
Start with these essential commands:
1. `vim testfile` - Open a file
2. `i` - Start typing
3. Type some text
4. `Esc` - Stop typing
5. `:wq` - Save and quit

### Common Beginner Mistakes
- Trying to type while in Normal mode (press `i` first)
- Forgetting to press `Esc` before using commands
- Not knowing how to quit (use `:q!` if stuck)

---

## Quick Reference Card

| Task | Command |
|------|---------|
| Open file | `vim filename` |
| Start typing | `i` |
| Stop typing | `Esc` |
| Save | `:w` |
| Quit | `:q` |
| Save and quit | `:wq` |
| Quit without saving | `:q!` |
| Delete line | `dd` |
| Copy line | `yy` |
| Paste | `p` |
| Undo | `u` |
| Search | `/word` |

---

## Why VIM is Actually Easy

Once you understand the mode concept, VIM becomes very logical:
- **Normal mode**: Navigate and give commands
- **Insert mode**: Type text
- **Command mode**: File operations

The key insight is that VIM separates "navigating/commanding" from "typing text" - this separation makes it incredibly powerful and efficient once learned.

**Remember**: When in doubt, press `Esc` to return to Normal mode!



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