# IBIOL compute resources

Different labs at the institute host compute and storage servers. This is to complement the resources provided by the SITEL. The servers are managed by the respective labs and but are available for all members of the institute as long as there is capacity. 

Resources or storage space may be limited. So, please discuss with your PI and Daniel Croll if you need more resources.

The labs managing the servers can provide only very limited support beyond providing you access as we have no staff dedicated to this (all volunteers). If you need training or troubleshooting help, please discuss this with your PI.

 <img src="https://github.com/crolllab/IBIOL-compute-resources/blob/main/image.jpeg?raw=true" alt="alt text" width="400" height="300">

## Available servers & resources

### Compute cluster
- `LEGcompute` is hosted by the Laboratory of Evolutionary Genetics with the following specs: 
  - CPUs: Intel + AMD processors with ~1000 cores in total
  - GPUs: 1x NVIDIA GeForce RTX 3090, 3x NVIDIA RTX A5500 GPUs, 4x NVIDIA RTX 5000 GPUs, 2x NVIDIA RTX Pro Blackwell 96 GB
  - 4 TB RAM
  - 80 TB SSD storage 
  - Ubuntu 26.04 LTS
  - Slurm queuing system
  - RStudio server

### Large storage servers
- LEGserv: Synology storage server for the Laboratory of Evolutionary Genetics and other IBIOL labs (>200 TB). 
- IBIOLdata: Synology storage server for the Laboratory of Molecular and Cellular Biology & microscopy data.

## New user? Need resources?

If you are a new user, please use [this form](https://forms.gle/JwfPj5VRLhjTFNf57) to request an access to the servers. 

You can also use the form to communicate future needs. This way, we know about your needs and will keep you updated.

## How to access the servers

**From UNINE**: To access the servers, you need to be on the `unine` Wifi or connected through a cable to the network. 

**From home**: A connection over a VPN is possible through the [GlobalProtect VPN client](https://mydoc.unine.ch/fr/Internet_VPN_WiFi/VPN) (you need to log in to view the site). You need to have a computer getting at the least the "orange" status during the checks by the VPN client. "Orange" can be achieved both by SITEL provided machines but also by personal machines that are up-to-date and have a working antivirus. Check the SITEL documentation how to install GlobalProtect to get further information.

For specific instructions on how to connect to the servers, please see the following sections.

# Working on LEGcompute

## Command-line access over ssh

Note that you receive your username and password after registration.

LEGcompute: `ssh username@legcompute.unine.ch`

Recommeded: establish a connection with ssh key

```
### on your local machine
# accept the default suggestion for saving the file
ssh-keygen 
# copy the key
ssh-copy-id username@legcompute.unine.ch

# Important for macOS: change the ssh configuration file like this (e.g. with nano):
nano ~/.ssh/config

# Copy exactly this text in:
Host *
    UseKeychain yes
```

## Lost or change your password?

(NB: the `passwd` command on the cluster does not properly reset your password)

_Lost password_:  
1.) Connect to the [User account server](https://legserv.de6.quickconnect.to)  
2.) Enter your username  
3.) If you forgot your password, click on "Forgot your password?". Type your email (your unine.ch address in most cases) and follow the instructions you get by mail. Check your spam folder if necessary. If no message comes in, please contact Daniel.    

_Change password_:  
1.) Connect to the [User account server](https://legserv.de6.quickconnect.to)  
2.) Enter your username, then your password  
3.) Click on the user icon top right, select "Personal"  
4.) Click on "Change password"  


## Access to RStudio Server (including Terminal access)

[RStudio web interface](http://legcompute.unine.ch:8787): `http://legcompute.unine.ch:8787`


## Remote desktop access (VNC) for Ubuntu

A full graphical desktop on the cluster head node, in addition to SSH. Log in with your **usual cluster username and password** — the same ones you use for SSH. No separate account, no key to set up.

### Server address

```
legcompute.unine.ch
```

### On macOS

1. Install **Windows App** from the Mac App Store — free, published by Microsoft. It was called *Microsoft Remote Desktop* until 2024, and you may still find it under that name.
2. Open it and click **+** → **Add PC**.
3. **PC name:** `legcompute.unine.ch`
4. **User account:** choose *Ask when required*, or add your cluster username and password to save it.
5. Double-click the entry to connect. Accept the certificate warning the first time — the server uses a self-signed certificate, which is expected.

### On Windows

1. **Remote Desktop Connection** is already installed. Press the Windows key and type `Remote Desktop Connection`, or run `mstsc`.
2. **Computer:** `legcompute.unine.ch`
3. Click **Connect**, then enter your cluster username and password.
4. Accept the certificate warning on first connect — expected, as above.

### Good to know

**You can browse and install software from the internet.** Firefox is already installed for this. Do a right-click on the Desktop => Select Applications => Web Browser. Alternatively, locate the "Applications" menu in the top left corner.

**Your session survives disconnection.** Close the window or lose your network, and everything stays exactly as you left it — reconnect and your applications are still open. This is the main advantage over `ssh -X`.

**Sessions disconnected for more than 8 hours are closed** and unsaved work in them is lost. Log out properly when you finish for the day.

**Eight people can be connected at once.** If you get turned away, someone will need to disconnect first.

### Please don't run heavy compute jobs on the desktop

The head node is also a compute node, and the desktop shares resources: **24 GB of memory and 8 CPUs per person**. Exceed the memory limit and your processes are killed without warning.

The desktop is for interactive work — graphical software, editors, file management, IGV, plotting, light scripting. Anything that needs real memory, many cores, or a GPU belongs in a Slurm job:

```bash
srun --cpus-per-task=8 --mem=32G --pty bash
```

**GPU work must go through Slurm.** The two GPUs on this machine are scheduled by Slurm and are not available from the desktop.


## Server organization and quota

Every user has a home folder: `/home/username` with a quota of 50-100 GB. This is where you could store your scripts and small data files.

You also have access to a data folder: `/data/username` with a quota of 1+ TB. This is the place to download, analyze and store your data.

All users have access to `/scratch`. This is a space to storage large amounts of temporary files. Files older than 30 days will automatically be deleted!

Please check your quota on the login screen.


## Transferring files

You can use a client like Cyberduck to access the files on the server. Choose "SFTP (SSH File Transfer Protocol)" and use the same credentials as for the ssh connection.
You can use `scp` or `rsync` to transfer files between your local machine and the server. Here is an example:

```
# from your local machine to the server
scp /path/to/local/file username@legcompute.unine.ch:/data/username/
scp -r /path/to/local/folder username@legcompute.unine.ch:/data/username/

# from the server to your local machine, run this command on your local machine!
scp username@legcompute.unine.ch:/data/username/file /path/to/local/
scp -r username@legcompute.unine.ch:/data/username/folder /path/to/local/
```

It's recommended to use `rsync` for large files or directories.

You can also access any public website / FTP server from the server to directly download/upload data.

## Data backup

***CAUTION: No backups are made of your data on the server. It is your responsability to create meaningful copies at regular intervals.***


## Queuing system (Slurm)

If your analyses take hours and use multiple CPUs, you should use the queuing system. This is to ensure that the server is not overloaded and that everyone has a fair share of the resources.

To submit a job, you need to create a script file with the commands you want to run. Here is an example of a script file:

```
#!/bin/bash
your command1 here
your command2 here
...
```

Run the script with the following command: `sbatch your_script.sh`

Single commands can be provided using the `wrap` option: `sbatch --wrap="your command1 here"`

Options for the `sbatch` command:
- `-p normal.1000h` to specify that the job can run a maximum of 1000 hours. `-p normal.12h` for jobs with a maximum of 12h. The default queue is `normal.168h`.
- `-c 4` to specify that the job needs 4 CPUs. The default is 1 CPU. Make sure that your command is parallelized and configured to use multiple CPUs to avoid blocking resources.
- `-o output_file.txt` to specify the output file. The default is `slurm-<jobID>.out`.
- `--mem=8G` to specify that the job needs 8 GB of RAM. The default is 1 GB.

Useful tools:
- `showq` to see the status of your jobs
- `showq -a` shows all pending and running jobs on the server
- `seff JOBID` shows memory and CPU efficiency stats (successfully completed jobs only)
- `seff -a` show these stats the last 100 jobs
- `sinfo` to see the status of the nodes (LEGcompute2 and 3)
- `speek jobID` to see the status of your jobs
- `scancel jobID` to cancel a job
- `scontrol show job jobID` to see the details of a job
- `scontrol hold job jobID` to hold a job
- `scontrol release job jobID` to release a job

## GPU Resources

The cluster provides 10 large GPUs: RTX 3090, RTX 5000 Ada, RTX A5500 and RTX PRO 6000 Blackwell (96 GB)

Request a GPU in your job script with `--gres=gpu:<count>` (either in a script or using `sbatch --gres=...`). To target a specific model, add the type before the count.

```bash
#SBATCH --gres=gpu:1              # any available GPU
#SBATCH --gres=gpu:2              # any 2 GPUs on the same node
#SBATCH --gres=gpu:rtx3090:1      # specifically an RTX 3090
#SBATCH --gres=gpu:rtx5000ada:2   # 2× RTX 5000 Ada
#SBATCH --gres=gpu:rtxa5500:1     # RTX A5500
#SBATCH --gres=gpu:rtxpro6000:1   # RTX PRO 6000 Blackwell
```

## Installing software

`LEGcompute` has installations of R, Java, Python and Perl. Bioinformatics software is most easily accessed using the available modules (see below). Additional options include using conda / micromamba or containers.

## Using software modules

Available modules include many popular bioinformatics tools, R, nextflow, some Java, Python and Perl versions, etc.

There are currently ~200 tools available.

### Finding available software (and specific versions)
```bash
module overview           # List view of all modules
module avail              # List all available modules (including available versions)
module avail SAM          # Search for specific tool (e.g., SAMtools)
module spider BLAST       # Search with descriptions (if you are unsure about the spelling)
```

### Loading and using modules
```bash
module load SAMtools/1.19.2-GCC-13.2.0     # Load a specific version
module load SAMtools BCFtools GATK         # Load multiple modules at once
samtools --version                         # Use the loaded software
```

### Unloading modules
```bash
module unload SAMtools    # Unload specific module
module purge              # Unload all modules
```

### Using more than one module at once
```bash
module load SAMtools VCFtools              # loads the default SAMtools and VCFtools
```

**NB: Some modules might have a conflict when being used together. Loading a problematic module will trigger a warning message. In most cases, the modules still work together though.**

You can avoid any module conflict by unloading a module directly after use.

### Using modules in Slurm jobs

**In a job script:**
```bash
#!/bin/bash
#SBATCH --cpus-per-task=4
module load BWA SAMtools
bwa mem -t 4 ref.fa reads.fq | samtools sort -o output.bam
```

**With sbatch --wrap:**
```bash
module load FastQC
sbatch --wrap "fastqc sample.fastq.gz" --cpus-per-task=2
```


# Future improvements (under construction)

Please use the sign-up form above to communicate your needs for resources and improvements. This way, we know about your needs and will keep you updated.

### Windows virtual machine

We are currently unable to provide this option. 



