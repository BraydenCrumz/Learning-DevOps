# Learning-DevOps

This repository will be used to track my progress from nothing to, hopefully, DevOps Engineer. Currently SysAdmin.

## Game Plan
### September 25th, 1:00 AM (EST)

I dug up this tiny ThinkCentre I had with an i5-10400T CPU (6 Physical Cores, 12 Logical Cores) and 24GB of DDR4 RAM and installed Proxmox VE 8.2.2.

I began doing research on what I should do to learn, but I am kinda lost. My current idea is to use Terraform (for the first time) and integrate it with Proxmox's API using [this tutorial](https://spacelift.io/blog/terraform-proxmox-provider).

After I set up Terraform and get a connection with Proxmox's API, I can begin learning the basics of Terraform, such as creating VMs, managing their resources, and destroying VMs, etc.

### Virtual Machine Plans

As far as the virtual machines themselves, I want to set up the following:

1. **Windows Server 2019 (headless)**  
   - **Cores:** 4  
   - **Memory:** 4GB  
   - **Purpose:** Domain Controller; will help me learn PowerShell. (I realize redundancy is good, but I am working within a confined number of resources.)

2. **CentOS**  
   - **Cores:** 4  
   - **Memory:** 12GB  
   - **Purpose:** Joined to Active Directory using realmd. I plan to utilize the CLI to get my hands dirty with Linux again and learn Bash. The extra memory is to learn Docker.

3. **Windows 10**  
   - **Cores:** 4  
   - **Memory:** 4GB  
   - **Purpose:** Joined to the domain to work on Group Policy from a domain level.
