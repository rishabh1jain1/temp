## Before Installation

Please make sure that you have access to the following:
 
 * All the Zeus Ansible Projects needed to deploy the system.
 * An Openstack cluster with at least resources free to launch 16 VMs(each with 4GB RAM, 1 VCPU, 50GB Disk)

## Setup

 1. Clone the git repo zeus_deployer.
   > git clone https://github.com/CiscoZeus/zeus_deployer.git

 2. Install the requirements using the requirements.txt file present in the project
   > sudo pip install -r requirements.txt
 
 3. Please ensure that django installed has the version 1.8.X. If not, please install it using
   > sudo pip install django==1.8

 4. Install django_rq 
   > sudo pip install django_rq

 5. Install ansible
   > sudo apt-get install ansible

 6. Create directory to store the pod related files temporarily.
   > mkdir /tmp/deployments

 7. Install redis-server
   > sudo apt-get install redis-server

 8. Install Packer www.packer.io.
    Create a directory /usr/local/packer and unzip the package downloaded from www.packer.io there only.

## Setting up Zeus_Deployer
  
  ### Setting up django settings.py

    1. 


