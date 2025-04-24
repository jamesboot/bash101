# bash101

## 🔐 SSH into an HPC using Public/Private Keys

https://wiki.thecrick.org/display/HPC/Access+compute+resources

### 🧾 What is SSH?

SSH (Secure Shell) is a cryptographic network protocol used to securely access remote machines.

### 🔑 What are Public and Private Keys?

- Private Key: Stays on your machine. It's like your personal signature.
- Public Key: Uploaded to the server. It allows the server to recognize your private key.
- Together, they form a key pair used for authentication without typing a password.

```
ssh bootj@login.nemo.thecrick.org
```

### 🔌 Create SSH Session in MobaXterm GUI

- Install MobaXterm if not already.
- Open MobaXterm.
- Click Session → SSH.
- Enter:
  - Remote host: hpc.address.edu
  - Username: your HPC username
- Under Advanced SSH Settings:
  - Check “Use private key” and select your private key (id_rsa)
  - `C:\Users\<YourUsername>\Documents\MobaXterm\home\.ssh` OR `~/.ssh`
- Click OK to save and connect.

## 🖥️ What is SLURM Scheduler?

### 🧾 SLURM = Simple Linux Utility for Resource Management
It’s a job scheduler used in many HPC systems to allocate resources (like CPUs and memory) and run jobs.

### 🛠️ How it works:

- You submit a job script to SLURM.
- SLURM queues the job based on resources and priority.
- SLURM executes the job when resources are available.

### ✍️ Example SLURM Job Script (job.slurm)
```
#!/bin/bash
#SBATCH --job-name=my_job
#SBATCH --output=output_%j.txt
#SBATCH --ntasks=1 # No need to worry about this most of the time
#SBATCH --cpus-per-task=1
#SBATCH --time=01:00:00
#SBATCH --mem=4G # OR  --mem-per-cpu or --mem-per-gpu

# Load required modules
module load python/3.9

# Run your script
python my_script.py
```

### Arrays

Easy way to send off multiple processes/tasks/jobs at once (e.g. multiple samples)

```
#SBATCH --array=0-9              # 10 tasks, IDs 0 to 9
```

### 📤 Submit a Job

SBATCH
```
# sbatch - submit and forget
sbatch job.slurm
```

SRUN - interactive
```
# 1 task with 1 core per task with 4 GB of memory on the interactive partition for 8 hours and is running the command /bin/bash
srun --ntasks=1 --cpus-per-task=1 --partition=nint --time=08:00:00 --mem=4G --pty bash
```

### 📋 Other slurm commands
```
# Run a job in batch mode (set and forget)
sbatch <your-job-script-name>

# View status of all jobs for my username:
squeue -u bootj

# Get non-abbreviated queue info with:
squeue -u bootj --long

# When checking the status of a job, you may want to repeatedly call the squeue command to check for updates:
squeue -u bootj --long --iterate

# Cancel a job (you can use a comma-separated list of job IDs):
scancel your_job-id

# Job status (CPU usage, task information, node information, resident set size (RSS), and virtual memory (VM))
sstat --jobs=your_job-id

# Analysing past jobs
sacct --jobs=your_job-id

# Suspend a job:
scontrol suspend job_id

# Resume a job
scontrol resume job_id
```

## Modules

- Some software is available as a module on NEMO
- This means its pre-intalled - you can load it and then use it!
- You can search for software using `ml spider PROGRAM_NAME`
- You can load modules using `ml PROGRAM_NAME`
- 

## 🔧 Cheatsheet

### 🔍 File Navigation
- `pwd` : Show current directory
- `ls` : List files
- `cd <dir>` : Change directory
- `cd ..` : Move up one level

### 📂 File Management
- `cp <src> <dst>` : Copy files
- `mv <src> <dst>` : Move/Rename
- `rm <file>` : Remove file
- `mkdir <dir>` : Make directory

### 📝 File Viewing
- `cat <file>` : Show file contents
- `less <file>` : Scrollable view
- `head -n 10 <file>` : First 10 lines
- `tail -n 10 <file>` : Last 10 lines

### 🔍 Searching
- `grep 'pattern' file` : Search for pattern
- `find . -name "*.txt"` : Find files by name

### 🛠️ Permissions
- `chmod +x <file>` : Make file executable
- `chown user:group <file>` : Change ownership

### 📁 Archive & Compression
- `tar -cvf archive.tar dir/` : Create archive
- `tar -xvf archive.tar` : Extract archive
- `gzip file` : Compress
- `gunzip file.gz` : Decompress

### 🧪 Process Management
- `ps aux` : List running processes
- `top` or `htop` : Real-time monitoring
- `kill <PID>` : Terminate process

### 🔐 SSH Commands
- `ssh user@host` : Connect to remote server
- `scp file user@host:/path/` : Secure file copy

### 🗂 SLURM Basics
- `sbatch job.slurm` : Submit job
- `squeue` : View job queue
- `scancel <job_id>` : Cancel job
- `sinfo` : Show cluster info
