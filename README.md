# Learning-DevOps

This repository will be used to track my progress from zero, hopefully, hero. 
## Game Plan

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



Tried setting up API Access and a main.tf file, but the tutortial mentioned above seemed to miss out on several neccesary details. I am instead opting to run WSL2 with Ubuntu on my local machine (not on Win10Pro for Hyper-V) and start fresh with [this tutorial](https://www.youtube.com/watch?v=dvyeoDBUtsU&t) by Christian Lempa.



After undoing my previous work, I remoted into my Ubuntu/WSL2 Machine. I opened the Proxmox WebGUI and created a new user called ```terraformSA``` and gave it ```PVEAdmin``` perms as well as ```Administator``` Perms to the ```local-lvm``` storage. Then, I created a API token for the ```terraformSA``` account without privelege seperation so it would have the same permissions. 

Although I could have installed it on the Ubuntu machine, I opted to connect VSCode on my local machine to the Ubuntu File System with the WSL VSCode extension. Back on WSL2:
```bash
mkdir /home/user/terraformProxmox #this will be our working directory
cd /home/user/terraformProxmox
sudo nano provider.tf
```
I then saved and closed the empty ```provider.tf``` file and opened it with VSCode. This is where we will declare the version and provider. Although Proxmox does not have an official Terraform provider, the telmate/proxmox provider seems to be reputable. We will also create three variables for the Proxmox API Credentials. (this will be explained later.)
```terraform
terraform {

    required_version = >= "0.13.0" 

    required_providers {
        proxmox = {
            source = "telmate/proxmox"
            version ="2.9.3"
        }
    }
}

variable "proxmox_api_url" {
    type = string
}

variable "proxmox_api_token_id"{
    type = string
    sensitive = true # will prevent it from being displayed in the terminal output
}

variable "proxmox_api_token_secret" {
    type = string
    sensitive = true
}
```
In the same directory, we will create a ```credentials.auto.tfvars``` file where we will store and define the above variables. Saving the file as a ```.auto.tfvars``` will allow it to be automatically loaded as long as it is in the same directory, meaning we will not have to call or load it later. This will also allow us to exclude only one file from our GitHub repository when we upload it later.
```terraform
proxmox_api_url = ***
proxmox_api_token_id = ***
proxmox_api_token_secret = ***
```
Now that we have our credentials defined above, we need to go back to ```provider.tf``` and assign them according to the provider's language:
```terraform
provider "proxmox" { # assigned lines 6-8

    pm_api_url = var.proxmox_api_url # the var. states we are referring to a variable.
    pm_api_token_id = var.proxmox_api_token_id
    pm_api_token_secret = var.proxmox_api_token_secret

    pm_tls_insecure = true # needed because certificate is self-signed.
}
```
After saving, we can now initalize Terraform from WSL2
```bash
terraform init
```
If everyting went smoothly, it should return that it found the telmate/proxmox provider and succesfully initialized.
