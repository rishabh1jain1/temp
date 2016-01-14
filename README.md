### Before Installation

Please make sure that you have access to the following:
 
 * All the Zeus Ansible Projects needed to deploy the system.
 * An Openstack cluster with at least resources to launch 16 new VMs(each with 4GB RAM, 1 VCPU, 50GB Disk). You will also need 16 free floating IPs if the deployer server that you would be starting is in a machine not present in the same Openstack Cluster.

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
OS_USERNAME = "rishabja"
OS_PASSWORD = "XXXX"
```

  - Generate a public-private key pair. Add the public key to the Openstack Project with some name. Fill the name at *KEYPAIR_NAME*.
    Fill the path to the private key file at *PRIVATE_KEY_PATH*

  - *NETWORK_ID* is the fixed Network ID where you want to put your VMs into. *NETWORK_NAME* is the name of that network. It can be obtained using the *Networks* button present on the side bar in Horizon.

  - Fill other Openstack Cluster related settings.

  - **Please note that OS_PASSWORD is not your Cisco Cloud Account password**. Its an API key that can be generated from the Horizon dashboard.
    1. Open Horizon.
    2. Hover over your username and select 'settings' from the dropdown.
    3. Select 'other' and choose 'Generate API Key'
  
##### 2. Setting up Packer variables in *packerfile.json* file.
```
"variables": {
      "ssh_username": "ubuntu",
      "username": "rishabja",
      "password": "XXXX",
      "source_image": "460275a4-fb44-4e42-954b-be50e85b45da",
      "flavor": "c2516854-f8b4-4abb-b476-36eb80e32d6f",
      "networks": "e4d39d7d-b706-4e09-b55e-6484542a84a4",
      "use_floating_ip": "False",
      "floating_ip_pool": "public-floating-601",  
      "playbook_file": "zeus.yml",
      "playbook_dir": ".",
  }
```
Ideally you need to change the following:

  - *username* and *password* are same as OS_USERNAME and OS_PASSWORD set in the settings.py.

  - *networks* is the fixed network id used above in settings.py. 

  - *flavor* is the ID of the flavour that you want to use for each Zeus VM that is to be used for deployment. Recommended (GP2-Medium - 4GB RAM, 1 VCPU, 50 GB Disk).

  - *source_image* is the ID of the base image that you want to use in these VMs. Recommended is Ubuntu-Trusty.

  - *use_floating_ip* must be true if your machine on which you have started the server is outside the cluster fixed network. In that case, *floating_ip_pool* must also be provided; it can be obtained from network topology map in horizon.

  - Image, Network and Flavour IDs can be obtained using Openstack CLI commands. Before you run any CLI command, source the openstack-rc file for your project. Instructions can be found at:  <br /> http://docs.openstack.org/user-guide/common/cli_set_environment_variables_using_openstack_rc.html

  > source `<openstack-rc.sh>`  <br /> 
  > nova image-list  <br /> 
  > nova flavor-list  <br /> 
  > neutron net-list  <br /> 

  **Please note that *source `<openstack-rc.sh>`* will prompt for your Cloud Account Password. But as we did before, fill in the API key instead.**

