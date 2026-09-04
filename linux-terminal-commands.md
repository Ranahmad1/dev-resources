# Linux / Terminal Commands Cheatsheet

## Navigation
```bash
pwd                    # Print current directory
ls                     # List files
ls -la                 # List all files with details (hidden included)
cd folder              # Change directory
cd ..                  # Go up one level
cd ~                   # Go to home directory
cd -                   # Go to previous directory
clear                  # Clear terminal (or Ctrl+L)
```

## Files & Directories
```bash
# Create
touch file.txt         # Create empty file
mkdir folder           # Create directory
mkdir -p a/b/c         # Create nested directories

# Copy & Move
cp file.txt backup.txt         # Copy file
cp -r folder/ backup/          # Copy directory
mv file.txt new-name.txt       # Rename file
mv file.txt /path/to/folder/   # Move file

# Delete
rm file.txt            # Delete file
rm -rf folder/         # Delete folder (CAREFUL!)

# View
cat file.txt           # Print file content
less file.txt          # Scroll through file (q to quit)
head -n 20 file.txt    # First 20 lines
tail -n 20 file.txt    # Last 20 lines
tail -f logfile.log    # Live follow log file

# Search
find . -name "*.js"            # Find by name
find . -type f -size +1M       # Find files > 1MB
grep -r "search_term" .        # Search text in files
grep -n "error" logfile.log    # Search with line numbers
```

## File Permissions
```bash
chmod 755 file.sh      # rwxr-xr-x
chmod 644 file.txt     # rw-r--r--
chmod +x script.sh     # Make executable
chown user:group file  # Change owner

# Permission Numbers
# 4 = read (r)
# 2 = write (w)
# 1 = execute (x)
# 7 = rwx, 6 = rw-, 5 = r-x, 4 = r--
```

## Process Management
```bash
ps aux                 # List all processes
top                    # Live process monitor
htop                   # Better live monitor (if installed)
kill PID               # Kill process by ID
kill -9 PID            # Force kill
pkill node             # Kill by name
jobs                   # List background jobs
command &              # Run in background
fg                     # Bring to foreground
Ctrl + C               # Stop running process
Ctrl + Z               # Suspend process
```

## Networking
```bash
ping google.com        # Test connection
curl https://api.example.com       # Make GET request
curl -X POST url -d 'data'         # POST request
curl -O https://example.com/file   # Download file
wget https://example.com/file      # Download file
netstat -tulnp         # Show open ports
ss -tulnp              # Modern netstat
ifconfig               # Network interfaces (old)
ip addr                # Network interfaces (new)
ssh user@192.168.1.1  # SSH into server
scp file.txt user@server:/path/  # Secure copy to server
```

## Package Management
```bash
# Ubuntu / Debian (apt)
sudo apt update                    # Update package list
sudo apt upgrade                   # Upgrade packages
sudo apt install nginx             # Install package
sudo apt remove nginx              # Remove package
sudo apt autoremove                # Remove unused

# Node (npm)
npm install package                # Install locally
npm install -g package             # Install globally
npm uninstall package              # Remove
npm list                           # List installed
npm outdated                       # Check outdated

# Python (pip)
pip install package
pip list
pip freeze > requirements.txt      # Export dependencies
pip install -r requirements.txt    # Install from file
```

## Text Editors in Terminal
```bash
nano file.txt          # Simple editor
                       # Ctrl+O save, Ctrl+X exit

vim file.txt           # Powerful editor
                       # i = insert mode
                       # Esc = normal mode
                       # :w = save
                       # :q = quit
                       # :wq = save & quit
                       # :q! = force quit
```

## Useful Tricks
```bash
history                        # Show command history
!!                             # Repeat last command
!npm                           # Repeat last npm command
Ctrl + R                       # Search history
Ctrl + A                       # Go to start of line
Ctrl + E                       # Go to end of line
Alt + F / B                    # Move word forward/back

# Pipe & Redirect
ls | grep ".js"                # Pipe output
command > output.txt           # Redirect to file
command >> output.txt          # Append to file
command 2>&1 | tee log.txt     # Save + show output

# Chain commands
command1 && command2           # Run 2 if 1 succeeds
command1 || command2           # Run 2 if 1 fails
command1 ; command2            # Always run both

# Aliases (add to ~/.bashrc)
alias ll='ls -la'
alias gs='git status'
alias gp='git push'
alias ..='cd ..'
```

## Disk & System Info
```bash
df -h                  # Disk usage (human readable)
du -sh folder/         # Folder size
free -h                # RAM usage
uname -a               # System info
hostname               # Machine name
whoami                 # Current user
date                   # Current date/time
uptime                 # System uptime
```
