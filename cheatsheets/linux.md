# Linux / Terminal Commands Cheatsheet

## Navigation
```bash
pwd                    # Print current directory
ls                     # List files
ls -la                 # Long format, all files (including hidden)
ls -lh                 # Human-readable sizes
ls --sort=size -lh     # Sort by size
cd folder              # Enter directory
cd ..                  # Go up one level
cd ~                   # Home directory
cd -                   # Previous directory
cd /var/log            # Absolute path
```

## Files & Directories
```bash
# Create
touch file.txt                # Create empty file
mkdir folder                  # Create directory
mkdir -p a/b/c                # Create nested dirs

# Copy & Move
cp file.txt backup.txt        # Copy file
cp -r folder/ backup/         # Copy directory recursively
mv file.txt new-name.txt      # Rename file
mv file.txt /path/to/dest/    # Move file

# Delete
rm file.txt                   # Delete file
rm -i file.txt                # Confirm before delete
rm -rf folder/                # Delete folder + contents ⚠️
rmdir folder                  # Delete empty folder only

# View
cat file.txt                  # Print entire file
less file.txt                 # Scrollable view (q to quit)
head -n 20 file.txt           # First 20 lines
tail -n 20 file.txt           # Last 20 lines
tail -f app.log               # Follow/watch log file
wc -l file.txt                # Count lines
wc -w file.txt                # Count words

# Search in files
grep "error"    file.txt      # Search in file
grep -r "error" ./            # Recursive search
grep -n "error" file.txt      # Show line numbers
grep -i "error" file.txt      # Case-insensitive
grep -v "error" file.txt      # Invert match (exclude)
grep -E "err|warn" file.txt   # Extended regex

# Find files
find . -name "*.js"                  # By name
find . -name "*.log" -type f         # Files only
find . -type d -name "node_modules"  # Directories
find . -size +10M                    # Larger than 10MB
find . -mtime -7                     # Modified in last 7 days
find . -name "*.log" -delete         # Find and delete
```

## File Permissions
```bash
chmod 755 script.sh    # rwxr-xr-x
chmod 644 file.txt     # rw-r--r--
chmod +x script.sh     # Make executable
chmod -R 755 folder/   # Recursive
chown user file        # Change owner
chown user:group file  # Change owner and group
chown -R user:group /  # Recursive

# Permission bits
# r=4  w=2  x=1
# 7 = rwx  6 = rw-  5 = r-x  4 = r--  0 = ---
# 755 → owner: rwx | group: r-x | others: r-x
# 644 → owner: rw- | group: r-- | others: r--

ls -l file.txt    # View permissions
```

## Process Management
```bash
ps aux                # All processes
ps aux | grep nginx   # Filter by name
top                   # Live process monitor
htop                  # Better monitor (install separately)

kill    PID           # Terminate (SIGTERM)
kill -9 PID           # Force kill (SIGKILL)
killall nginx         # Kill all by name
pkill -f "pattern"    # Kill by pattern

command &             # Run in background
jobs                  # List background jobs
fg                    # Bring last job to foreground
fg %2                 # Bring job 2 to foreground
bg                    # Resume suspended job in background
nohup command &       # Run even after terminal closes
disown                # Detach from shell

Ctrl + C              # Kill foreground process (SIGINT)
Ctrl + Z              # Suspend foreground process
Ctrl + L              # Clear terminal
```

## Networking
```bash
# Test & Info
ping -c 4 google.com         # Ping (4 packets)
traceroute google.com         # Trace route
nslookup google.com           # DNS lookup
dig google.com                # Detailed DNS
curl -I https://example.com   # HTTP headers only
curl -s https://api.ipify.org # Get your public IP

# Download
curl -O https://example.com/file.zip     # Download to current dir
curl -L -o out.zip https://url/file      # Follow redirects, custom name
wget https://example.com/file            # Alternative
wget -r -np https://site.com/            # Recursive download

# API requests
curl -X GET  'https://api.example.com/users'  -H 'Authorization: Bearer TOKEN'
curl -X POST 'https://api.example.com/users'  -H 'Content-Type: application/json' -d '{"name":"Ahmad"}'
curl -X PUT  'https://api.example.com/user/1' -d 'name=Ahmad'

# SSH
ssh user@192.168.1.1               # Connect
ssh -p 2222 user@server.com        # Custom port
ssh -i ~/.ssh/key.pem user@server  # With key

# SCP (secure copy)
scp file.txt user@server:/path/
scp -r folder/ user@server:/path/
scp user@server:/path/file.txt .   # Download from server

# Ports & Connections
ss -tulnp                          # Show listening ports (modern)
netstat -tulnp                     # Classic netstat
lsof -i :3000                      # What's on port 3000
nc -zv host 80                     # Check if port is open
```

## Package Management
```bash
# APT (Debian/Ubuntu)
sudo apt update              # Refresh package list
sudo apt upgrade             # Upgrade all packages
sudo apt install nginx php   # Install packages
sudo apt remove nginx        # Remove
sudo apt purge nginx         # Remove + config files
sudo apt autoremove          # Remove unused deps
apt search nginx             # Search packages
apt show nginx               # Package info

# Snap
sudo snap install code --classic

# NPM
npm install package          # Local install
npm install -g package       # Global install
npm install -D package       # Dev dependency
npm uninstall package
npm update
npm list -g --depth=0        # Global packages
npm outdated
npm audit

# Pip (Python)
pip install package
pip install package==1.2.3          # Specific version
pip install -r requirements.txt
pip freeze > requirements.txt
pip list --outdated
pip install --upgrade pip
```

## Text Processing
```bash
# AWK
awk '{print $1}' file.txt           # Print first column
awk -F: '{print $1}' /etc/passwd    # Custom delimiter
awk 'NR>1 {print}' file.txt         # Skip first line
awk '{sum+=$1} END {print sum}'     # Sum a column

# SED
sed 's/old/new/' file.txt           # Replace first occurrence
sed 's/old/new/g' file.txt          # Replace all
sed -i 's/old/new/g' file.txt       # In-place edit
sed -n '5,10p' file.txt             # Print lines 5-10
sed '/pattern/d' file.txt           # Delete matching lines

# Sort, Uniq, Cut
sort file.txt
sort -r file.txt                    # Reverse
sort -k2 -n file.txt                # Sort by col 2 numerically
uniq file.txt                       # Remove adjacent duplicates
sort file.txt | uniq -c             # Count occurrences
cut -d',' -f1,3 data.csv            # Extract columns 1 and 3
```

## Useful Tricks
```bash
# History & Shortcuts
history                        # Command history
!!                             # Repeat last command
!nginx                         # Repeat last nginx command
Ctrl + R                       # Reverse search history
Ctrl + A                       # Go to start of line
Ctrl + E                       # Go to end of line
Alt + .                        # Insert last argument of previous command

# Pipe & Redirect
ls | grep ".md"                # Pipe: pass output as input
command > output.txt           # Redirect stdout (overwrite)
command >> output.txt          # Redirect stdout (append)
command 2> error.txt           # Redirect stderr
command &> all.txt             # Redirect stdout + stderr
command | tee output.txt       # Output to file AND terminal

# Command chaining
cmd1 && cmd2   # Run cmd2 only if cmd1 succeeds
cmd1 || cmd2   # Run cmd2 only if cmd1 fails
cmd1 ; cmd2    # Always run both

# Aliases (add to ~/.bashrc or ~/.zshrc)
alias ll='ls -lahF'
alias gs='git status'
alias gp='git push'
alias ..='cd ..'
alias ...='cd ../..'
alias serve='python3 -m http.server 8000'

# Environment variables
export PATH="$HOME/.local/bin:$PATH"
echo $HOME $PATH $USER
env                        # List all env vars
env | grep NODE
```

## System Info
```bash
uname -a               # System info
hostname               # Machine name
whoami                 # Current user
id                     # User ID & groups
uptime                 # How long running
date                   # Current date/time
cal                    # Calendar

df -h                  # Disk usage (human readable)
df -h /                # Disk usage of root
du -sh folder/         # Folder size
du -sh * | sort -h     # All items, sorted by size

free -h                # RAM usage
cat /proc/cpuinfo      # CPU info
cat /proc/meminfo      # Memory info
nproc                  # Number of CPU cores
lscpu                  # Detailed CPU info

# Logs
journalctl -xe         # System journal
journalctl -u nginx    # Logs for a service
tail -f /var/log/syslog

# Services (systemd)
sudo systemctl status  nginx
sudo systemctl start   nginx
sudo systemctl stop    nginx
sudo systemctl restart nginx
sudo systemctl enable  nginx    # Start on boot
sudo systemctl disable nginx
```
