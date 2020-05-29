# Azure Cloud init script for VMs

This script when dropped into azure cloud init section while provisioning helps with custom settings on boot.
You can either use this script along with an azure vm create or copy paste it in the portal.
Note: that your VM image should support cloud init scripts


# Lines
1. Line 1, Just says packge update if any
2. Lines 2 all the the way to line 10, specifies to partition as ext4 an additional disk
3. Lines 12-13 is mounting the the newly created device to the /data drive folder
4. Lines 14-16 is for installing the required packages
5. Lines 17-22 is for add an entry into daemon.json to tell docker that the docker home has changed to /data/docker (note: check if you need to have the docker folder created in advance)
6. Lines 23-29 is restart docker service after daemon.json has been updated, adding user for docker, creating a mount to my nfs servers and enabling docker for start up.
7. Line 30-31 is for adding my ssh keys for easy login

## VM Scale sets

I am using this cloud init script in a VM scale set. Each time a new VM is scaled these configs are applied to the newly scaled VM.

## Example az create

Below is an example of creating a VM using the az vm create command

```
az vm create \
    --resource-group <myrg> \
    --name <vmname> \
    --image UbuntuLTS \
    --location eastus \
    --admin-username <username> \
    --admin-password <password> \
    --subnet "Existing Subnet" \
    --size Standard_B2s \
    --data-disk-sizes-gb 128 \
    --public-ip-address "" \
    --authentication-type all \
    --custom-data yaml/cloud-init.yml
    
```

