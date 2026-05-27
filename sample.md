# 📁 SCP — Secure Copy Protocol

SCP copies files securely between hosts over SSH.

```bash
# Copy local file to remote server
scp file.txt user@192.168.1.10:/home/user/

# Copy remote file to local machine
scp user@192.168.1.10:/home/user/file.txt /local/path/

# Copy entire directory recursively
scp -r /local/dir/ user@192.168.1.10:/remote/dir/

# Copy with a specific SSH port
scp -P 2222 file.txt user@192.168.1.10:/home/user/

# Copy multiple files to remote
scp file1.txt file2.txt user@192.168.1.10:/home/user/

# Copy with identity file (SSH key)
scp -i ~/.ssh/id_rsa file.txt user@192.168.1.10:/home/user/

# Preserve file timestamps and permissions
scp -p file.txt user@192.168.1.10:/home/user/

# Limit bandwidth (in Kbits/s)
scp -l 500 largefile.tar.gz user@192.168.1.10:/home/user/

# Copy between two remote servers
scp user1@server1:/path/file.txt user2@server2:/path/
```

---

# SFTP — SSH File Transfer Protocol

SFTP is an interactive, secure file transfer client.

```bash
# Connect to remote server
sftp user@192.168.1.10

# Connect on a custom port
sftp -P 2222 user@192.168.1.10

# Connect using SSH key
sftp -i ~/.ssh/id_rsa user@192.168.1.10
```

### SFTP Interactive Commands (inside sftp session)

```bash
# List remote directory
ls
ls -la

# List local directory
lls

# Change remote directory
cd /remote/path

# Change local directory
lcd /local/path

# Upload a file
put file.txt

# Upload multiple files
put *.txt

# Upload a directory recursively
put -r local_dir/

# Download a file
get remote_file.txt

# Download multiple files
get *.log

# Download directory recursively
get -r remote_dir/

# Show current remote directory
pwd

# Show current local directory
lpwd

# Create remote directory
mkdir new_folder

# Delete remote file
rm remote_file.txt

# Rename remote file
rename old_name.txt new_name.txt

# Check file permissions / size
ls -lh

# Exit sftp
exit
bye
```

---

# AWK — Pattern Scanning and Processing

AWK processes text line by line using fields (`$1`, `$2`, ..., `$NF`).

```bash
# Print the first column of a file
awk '{print $1}' file.txt

# Print multiple columns
awk '{print $1, $3}' file.txt

# Print with custom delimiter (CSV)
awk -F',' '{print $1, $2}' file.csv

# Print lines matching a pattern
awk '/error/ {print}' file.log

# Print line numbers with content
awk '{print NR, $0}' file.txt

# Print total number of lines
awk 'END {print NR}' file.txt

# Sum values in column 2
awk '{sum += $2} END {print "Total:", sum}' file.txt

# Print lines where column 3 > 100
awk '$3 > 100 {print}' file.txt

# Print lines between patterns
awk '/START/,/END/ {print}' file.txt

# Print only the last field of each line
awk '{print $NF}' file.txt

# Replace a word in output
awk '{gsub(/foo/, "bar"); print}' file.txt

# Use multiple delimiters
awk -F'[,;]' '{print $1}' file.txt

# Print specific field from /etc/passwd
awk -F':' '{print $1, $7}' /etc/passwd

# Print lines with more than 5 fields
awk 'NF > 5 {print}' file.txt

# Print unique values of column 1
awk '!seen[$1]++' file.txt

# Add header to output
awk 'BEGIN {print "Name Age"} {print $1, $2}' file.txt

# Print last 3 fields
awk '{print $(NF-2), $(NF-1), $NF}' file.txt
```

---

# HEAD — View Beginning of a File

```bash
# Show first 10 lines (default)
head file.txt

# Show first N lines
head -n 20 file.txt

# Show first 50 bytes
head -c 50 file.txt

# Show first 5 lines of multiple files
head -n 5 file1.txt file2.txt

# Show all lines except last N lines
head -n -5 file.txt

# Real-time: watch growing log (first lines)
head -n 100 /var/log/syslog
```

---

# TAIL — View End of a File

```bash
# Show last 10 lines (default)
tail file.txt

# Show last N lines
tail -n 50 file.txt

# Show last 100 bytes
tail -c 100 file.txt

# Follow a file in real-time (great for logs)
tail -f /var/log/syslog

# Follow with retries (useful if file is recreated)
tail -F /var/log/app.log

# Show last N lines of multiple files
tail -n 5 file1.txt file2.txt

# Skip first N lines (show rest)
tail -n +5 file.txt

# Follow multiple log files simultaneously
tail -f /var/log/syslog /var/log/auth.log
```

---
# `sort` — Sort Lines

```bash
# Alphabetical sort
sort file.txt

# Reverse sort
sort -r file.txt

# Numeric sort
sort -n file.txt

# Sort by specific column (field)
sort -k2 file.txt

# Sort CSV by column 3 numerically
sort -t',' -k3 -n file.csv

# Remove duplicate lines while sorting
sort -u file.txt

# Sort by human-readable sizes (1K, 2M)
sort -h sizes.txt

# Sort in-place
sort -o file.txt file.txt
```

---

### `uniq` — Remove / Report Duplicates

```bash
# Remove consecutive duplicate lines
uniq file.txt

# Count occurrences
uniq -c file.txt

# Show only duplicate lines
uniq -d file.txt

# Show only unique (non-repeated) lines
uniq -u file.txt

# Ignore case when comparing
uniq -i file.txt

# Combine sort + uniq for full dedup
sort file.txt | uniq -c | sort -rn
```

# `tr` — Translate / Delete Characters

```bash
# Convert lowercase to uppercase
tr 'a-z' 'A-Z' < file.txt

# Delete specific characters
tr -d '0-9' < file.txt

# Squeeze repeated characters
tr -s ' ' < file.txt

# Replace colons with newlines
tr ':' '\n' < file.txt

# Remove newlines
tr -d '\n' < file.txt
```

---

### `find` — Find Files

```bash
# Find by name
find /path -name "*.log"

# Find by type (f=file, d=directory)
find /path -type f -name "*.txt"

# Find files modified in last N days
find /path -mtime -7

# Find files larger than 100MB
find /path -size +100M

# Find and delete
find /path -name "*.tmp" -delete

# Find and execute command
find /path -name "*.sh" -exec chmod +x {} \;

# Find files by permission
find /path -perm 644

# Find empty files
find /path -empty
```
