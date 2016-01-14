### Before Installation

Please make sure that you have access to the following:
 
 * All the Zeus Ansible Projects needed to deploy the system.
 * An Openstack cluster with at least resources free to launch 16 VMs(each with 4GB RAM, 1 VCPU, 50GB Disk)

### Setup

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
    **Create a directory /usr/local/packer and unzip the package downloaded from www.packer.io there only.**

### Setting the variables
  
##### 1. Setting up django settings.py
```
OS_AUTH_URL = "https://us-texas-3.cloud.cisco.com:5000/v2.0"
OS_TENANT_NAME = "zeus-prep-cluster-tx-3"
NETWORK_ID = "e4d39d7d-b706-4e09-b55e-6484542a84a4"
NETWORK_NAME = "Zeus-Net"
KEYPAIR_NAME = "rishabja"
PRIVATE_KEY_PATH = "~/.ssh/id_rsa"
FLOATING_IP_POOL = "public-floating-601"
OS_USERNAME = "XXXX"
OS_PASSWORD = "XXXX"
```

  - Generate a public-private key pair. Add the public key to the Openstack Project with some name. Fill the name at *KEYPAIR_NAME.*
  - Fill the path to the private key file at *PRIVATE_KEY_PATH*
  - Fill other Openstack related settings.
  - Please note that OS_PASSWORD is not the Cisco Cloud Account password from the Icehouse version of Openstack. Its an API key that can be generated from the Horizon dashboard.
    1. Open Horizon.
    2. Hover over your username and select 'settings' from the dropdown.
    3. Select 'other' and choose 'Generate API Key'
    


