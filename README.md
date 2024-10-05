# azure-linux-vm-deployment
useful bash commands used in Microsoft Applied skills lab: "Deploy a linux machine in Azure"
deployed the Linux VM via Azure portal with required configuration
used Azure CLI scripts to automate the deployment of the Linux VM
Post-deployment scripts that I used for used for partitioning the disk, installing Nginx

step one was to configure the Virtual Machine (VM1)
accessed VM1 via SSH (IP is found in Azure portal at the Linux VM details) using command:    

"ssh <username>@<public-ip-of-VM1>"

For partitioning the Data Disk, I list first the attached dissks using command:

"sudo fdisk -l"

Here I will see the needed disk listed as: /dev/sdb. Will next create a partition on the new disk using command:

"sudo fdisk /dev/sdb"

In terminal I follow the prompts (note there is no hint that they are interactive, the prompts are static) so we have to:

Press n for a new partition.
Press p for a primary partition.
Press 1 to select partition number 1.
Press Enter to accept the default first sector.
Press Enter again to accept the default last sector.
Press w to write the changes.

Next I format the partition as requested in task using command:
"sudo mkfs.ext4 /dev/sdb1"

Next step is to mount the Disk on a newly created directory:

"sudo mkdir /mnt/data" (this command creates the directory)

To mount the disk I enter commmand:

"sudo mount /dev/sdb1 /mnt/data"

I have to now make sure the disk is mounted on system boot so will open to check it in a nano editor
using command:

"sudo nano /etc/fstab"

NOTE: in this MS applied skil lab the usual keyboard shortcuts for saving&exiting the nano editor do not work as usual. 
I needed to try multiple times as it froze and lost all my work. My solution was to kill the nano editor from a second terminal while my  main one is still work in progress.
In other words, I advise for this task to use tow terminals to avoid getting stuck in the nano editor and loosing lab time.

Next in the editor I added the following line at the bottom of the file to mount the disk automatically:

"/dev/sdb1  /mnt/data  ext4  defaults  0  0"

Save and exit the editor (Ctrl+X, then Y, then Enter).

Last step is to install Nginx with these commands:

"sudo apt update"

"sudo apt install nginx -y"

I checked that Nginx is Running and Nginx service status:

"sudo systemctl status nginx"
It was not so I started it with this command from Microsoft learn module:

"sudo systemctl start nginx"

To ensure Nginx starts on boot, enabled the service with:

"sudo systemctl enable nginx"

Last step was to verify the mount:

"df -h"
This showed me see /dev/sdb1 mounted to /mnt/data.

I checked that Nginx is running by visiting the public IP address of my Azure VM in a browser:

"http://<public-ip-of-VM1>"

In browser I saw the default Nginx landing page.

Done.

Task summary:
provisioned a new Azure VM with Ubuntu 22.04 LTS
configured the data disk, mounted it persistently
installed the Nginx web server
and ensured it is running.
