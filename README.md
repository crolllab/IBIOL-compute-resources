# IBIOL compute resources

Different labs at the institute host compute and storage servers. This is to complement the resources provided by the SITEL. The servers are managed by the respective labs and but are available for all members of the institute as long as there is capacity. 

Resources or storage space may be limited. So, please discuss with your PI and Daniel Croll if you need more resources.

The labs managing the servers can provide only very limited support beyond providing you access as we have no staff dedicated to this (all volunteers). If you need training or troubleshooting help, please discuss this with your PI.

 <img src="https://github.com/crolllab/IBIOL-compute-resources/blob/main/image.jpeg?raw=true" alt="alt text" width="400" height="300">

## Available servers & resources

### Compute servers
- LEGcompute1-3 are hosted by the Laboratory of Evolutionary Genetics with the following specs: 
  - 1x Intel, 2x AMD processors with 592 cores in total
  - 1x NVIDIA GeForce RTX 3090, 4x NVIDIA RTX 5000 GPUs
  - 2TB RAM
  - 60TB SSD storage 
  - Ubuntu 24.04 LTS
  - Slurm queueing system
- IBIOLcompute is hosted by the Laboratory of Molecular and Cellular Biology with the following specs:
  - 1x AMD processor with 128 cores
  - 3x NVIDIA RTX A5500 GPUs
  - 0.5TB RAM
  - 15 TB storage
  - Ubuntu 22.04 LTS
  - Slurm queueing system

### Large storage servers
- LEGserv: Synology storage server for the Laboratory of Evolutionary Genetics. 
- IBIOLdata: Synology storage server for the Laboratory of Molecular and Cellular Biology & microscopy data.

## New user? Need resources?

If you are a new user, please use [this form](https://forms.gle/JwfPj5VRLhjTFNf57) to request an access to the servers. 

You can also use the form to communicate future needs. This way, we know about your needs and will keep you updated.

## How to access the servers

**From UNINE**: To access the servers, you need to be on the `unine` Wifi or connected through a cable to the network. 

**From home**: A connection over a VPN is possible through the [GlobalProtect VPN client](https://mydoc.unine.ch/fr/Internet_VPN_WiFi/VPN) (you need to log in to view the site). You need to have a computer getting at the least the "orange" status during the checks by the VPN client. "Orange" can be achieved both by SITEL provided machines but also by personal machines that are up-to-date and have a working antivirus. Check the SITEL documentation how to install GlobalProtect to get further information.

For specific instructions on how to connect to the servers, please see the following sections.

# Working on LEGcompute

### Command-line access over ssh

Note that you receive your username and password after registration.

LEGcompute: `ssh username@legcompute2.unine.ch`

Optional: establish a connection with ssh keys (recommended)

```
### on your local machine
# accept the default suggestion for saving the file
ssh-keygen 
# copy the key
ssh-copy-id username@legcompute2.unine.ch

# Important for macOS: change the ssh configuration file like this (e.g. with nano):
nano ~/.ssh/config

# Copy exactly this text in:
Host *
    UseKeychain yes
```

Once you've logged in, you can change your default password with the `passwd` command.

### Access to RStudio Server (including Terminal access)

[RStudio web interface](http://legcompute2.unine.ch:8787): `http://legcompute2.unine.ch:8787`

### Access files with a client

You can use a client like Cyberduck to access the files on the server. Choose "SFTP (SSH File Transfer Protocol)" and use the same credentials as for the ssh connection.

### Server organization and quota

Every user has a home folder: `/home/username` with a quota of `20 GB`. This is where you could store your scripts and small data files.

You also have access to a data folder: `/data/username` with a quota of `1 TB`. This is the place to download, analyze and store your data.

### Transferring files

You can use `scp` or `rsync` to transfer files between your local machine and the server. Here is an example:

```
# from your local machine to the server
scp /path/to/local/file username@legcompute2.unine.ch:/data/username/
scp -r /path/to/local/folder username@legcompute2.unine.ch:/data/username/

# from the server to your local machine, run this command on your local machine!
scp username@legcompute2.unine.ch:/data/username/file /path/to/local/
scp -r username@legcompute2.unine.ch:/data/username/folder /path/to/local/
```

It's recommended to use `rsync` for large files or directories.

You can also access any public website / FTP server from the server to directly download/upload data.

### Data backup

***CAUTION: No backups are made of your data on the server. It is your responsability to create meaningful copies at regular intervals.***


### Queuing system (slurm)

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
- `-p normal.1000h` to specify that the job can run a maximum of 1000 hours. The default queue is `normal.168h`.
- `-c 4` to specify that the job needs 4 CPUs. The default is 1 CPU. Make sure that your command is parallelized and configured to use multiple CPUs to avoid blocking resources.
- `-o output_file.txt` to specify the output file. The default is `slurm-<jobID>.out`.
- `--mem=8G` to specify that the job needs 8 GB of RAM. The default is 1 GB.

Useful tools:
- `showq` to see the status of the queue and how busy the server is
- `sinfo` to see the status of the nodes (LEGcompute2 and 3)
- `speek jobID` to see the status of your jobs
- `scancel jobID` to cancel a job
- `scontrol show job jobID` to see the details of a job
- `scontrol hold job jobID` to hold a job
- `scontrol release job jobID` to release a job


# Future improvements (under construction)

Please use the sign-up form above to communicate your needs for resources and improvements. This way, we know about your needs and will keep you updated.

## GPU access

LEGcompute and IBIOLcompute have a total of 8 powerful GPUs for machine learning.

## IBIOLcompute

Access to IBIOLcompute will be opened depending on demand.

### Remote desktop access (VNC) for Ubuntu

This options currently exists for `IBIOLcompute`. Only cable connections are allowed for VNC access (SITEL rules).

### Windows virtual machine

We are looking into offering this option. 
