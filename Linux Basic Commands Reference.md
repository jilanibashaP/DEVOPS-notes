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