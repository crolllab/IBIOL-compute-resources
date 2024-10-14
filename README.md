# IBIOL compute resources

## Available servers & resources

### Compute servers
- LEGcompute1-3 are hosted by the Laboratory of Evolutionary Genetics with the following specs: 
  - 1x Intel, 2x AMD processors with 592 cores in total
  - 1x NVIDIA GeForce RTX 3090, 4x NVIDIA RTX 5000 GPUs
  - 4x 2TB RAM
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
- LEGserv: Synology storage server with for the Laboratory of Evolutionary Genetics. 
- IBIOLdata: Synology storage server for the Laboratory of Molecular and Cellular Biology & microscopy data.

## How to access the servers

To access the servers, you need to be on the `unine` Wifi or connected through a cable to the network. A connection over a VPN may be possible but for this you need to consult the SITEL resources or open a ticket with specific questions. Not all computers are allowed to connect.

### Command-line access over ssh

LEGcompute2 `ssh username@legcompute2.unine.ch`

IBIOLcompute `ssh username@ibiolcompute.unine.ch`

### Access to RStudio Server (including Terminal access)

LEGcompute2 `http://legcompute2.unine.ch:8787`

IBIOlcompute `http://ibiolcompute.unine.ch:8787`

### Remote desktop access (VNC) for Ubuntu

This options exists for `IBIOLcompute`. Only cable connections are allowed for VNC access.

### Windows virtual machine

We are looking into offering this option. Please contact us if you are interested.

# New user?

If you are a new user, please use this form to request an access to the servers: [https://forms.gle/JwfPj5VRLhjTFNf57]