# tech241-Ansible

# 1. Infrastructure as Code (IaC): A Primer

Infrastructure as Code (IaC) is a paradigm in software engineering that leverages machine-readable definition files for the management and provisioning of infrastructure, superseding manual processes. This approach empowers developers and system administrators to handle infrastructure via code that is version-controlled, repeatable, and consistent. IaC essentially treats infrastructure as software, thereby enhancing the efficiency of managing, configuring, and deploying resources.

# 2. The Advantages of IaC

The use of IaC offers several benefits, such as:

- **Automated Deployment**: IaC supports automated deployment, minimizing human errors and accelerating the development process.
- **Documentation**: The code for the infrastructure serves as documentation, simplifying the understanding and maintenance of the system.
- **Version Control**: Infrastructure configurations are version-controlled, facilitating easy rollbacks and collaboration among team members.
- **Scalability and Flexibility**: Infrastructure can be effortlessly scaled up or down, offering flexibility to adapt to fluctuating demands.
- **Consistency and Reproducibility**: IaC enables teams to establish consistent and reproducible infrastructure environments across various stages of development and production.

# 3. IaC Tools

There are several tools available for implementing IaC, such as:

- **Azure Resource Manager**: Microsoft's IaC tool for managing resources in Azure.
- **Google Cloud Deployment Manager**: A tool similar to CloudFormation, but for the Google Cloud Platform (GCP).
- **AWS CloudFormation**: A tool specific to Amazon Web Services (AWS), allowing users to define infrastructure as JSON or YAML templates.
- **Terraform**: A popular tool for provisioning and managing infrastructure across multiple cloud providers.

# 4. Ansible for Configuration Management

Configuration management is the process of managing and maintaining the desired state of an infrastructure's configuration. Ansible is a robust open-source configuration management tool that automates the provisioning, configuration, and management of infrastructure resources.

# 5. The Benefits of Ansible

Using Ansible comes with several advantages, such as:

- **Large Community and Extensibility**: Ansible boasts a vast user community and supports a wide array of modules and plugins.
- **Idempotent Execution**: Ansible ensures that applying configurations multiple times yields the same result, preventing unexpected changes.
- **Simple and Human-readable Syntax**: Ansible playbooks are written in YAML, making them easy to understand and maintain.
- **Agentless Architecture**: Ansible connects to remote hosts via SSH, eliminating the need for agents on managed systems.

# 6. Setting Up an Ansible Control Machine in AWS

## 6.1. Launching an EC2 Instance for the Ansible Control Machine

1. Sign in to the AWS Management Console.
2. Go to the EC2 Dashboard.
3. Click "Launch Instance" and select a suitable Amazon Machine Image (AMI) for the control machine (e.g., Amazon Linux, Ubuntu, CentOS).
4. Select an instance type that meets your requirements.
5. Configure the instance details (e.g., VPC, subnet, security groups).![Alt text](images/setting-up-ansible-EC2/1.JPG)![Alt text](images/setting-up-ansible-EC2/2.JPG)![Alt text](images/setting-up-ansible-EC2/3.JPG)
6. Add storage as per your needs.
7. Configure any additional details, such as tags.
8. Review and launch the instance.
9. Choose an existing key pair or create a new one to access the instance via SSH.

## 6.2. Adding SSH Key Pair to the Control Machine

1. If you are creating a new key pair, AWS will provide a .pem private key file for download.
2. Securely store the .pem file on your local machine.

## 6.3. Preparing the Ansible Control Machine

1. Open a terminal or command prompt on your local machine.
2. Change the permissions of the .pem file to allow read-only access: `chmod 400 /path/to/your-key.pem`.

## 6.4. Installing Ansible

1. Connect to the Ansible control machine using SSH:
   ```bash
   ssh -i /path/to/your-key.pem ubuntu@your-ansible-control-machine-ip
   ```
2. Update the package manager:
   For Ubuntu: `sudo apt update -y`

3. Install Ansible:
4. Run this command first
   `sudo apt-add-repository ppa:ansible/ansible`
   For Ubuntu: `sudo apt install ansible -y`
   Update the package manager:
   For Ubuntu: `sudo apt update`
   Install Ansible:
   For Ubuntu: `sudo apt install ansible -y`
   4.Set this in `/etc/ansible/hosts `
   `ec2-instance ansible_host=52.16.232.170 ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/tech241.pem`

# 7. Pinging Ansible Agents

## 7.1. Preparing the Ansible Agents

1. Launch EC2 instances that you want to manage as Ansible agents.
2. Ensure that the security groups of these instances allow inbound SSH (port 22) traffic from the Ansible control machine's IP.

## 7.2. Pinging Ansible Agents

1. Open a terminal on the Ansible control machine.
2. Navigate to the directory where your Ansible playbooks will reside (e.g.,` /etc/ansible`).
3. Create an inventory file (e.g., `hosts`) and add the IP addresses or DNS names of your Ansible agents:

## Test the connectivity to Ansible agents using the ping module:

`ansible -i hosts web -m ping`

# Ansible Architecture Setup

This repository provides instructions and scripts to set up an Ansible architecture on AWS. It includes steps for creating EC2 instances, configuring Ansible, and running playbooks and ad-hoc commands for automation.

## Step 1: Creating EC2 Instances on AWS

Create three EC2 instances on AWS:

- One for the Ansible controller
- Two for the application and database agent nodes

Ensure SSH security group rules are set. Use Ubuntu 18.04 as the operating system since it comes with preinstalled Python, which is the only dependency required for Ansible.

### 3vms for ansible

## Step 2: Installing Ansible on the Controller Node

SSH into the Ansible controller node.

Update and upgrade the system:

```bash
sudo apt update -y
sudo apt upgrade -y
Install Ansible using the PPA repository:
```

```bash

sudo apt-add-repository ppa:ansible/ansible
sudo apt install ansible -y
sudo apt update -y
```

Step 3: Configuring the Controller Node
Create a .pem file named "tech241" in the ~/.ssh/ folder on the controller node.

Step 4: Preparing the Agent Nodes
SSH into the application and database agent nodes manually.

Update and upgrade the system to ensure they are available for connection:

```bash

sudo apt update -y
sudo apt upgrade -y
```

Step 5: Configuring the Hosts File
Navigate to the default Ansible directory:

```bash

cd /etc/ansible/
```

Edit the hosts file using the nano text editor:

```bash

sudo nano hosts
```

Add the IP addresses and SSH connection details for the app and db VMs:

````bash

[web]
web-instance ansible_host= app ip ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/tech241.pem

[db]
db-instance ansible_host=db ip ansible_user=ubuntu ansible_ssh_private_key_file=~/.ssh/tech241.pem
Test the connection between the controller and agents:

```bash

sudo ansible web -m ping
sudo ansible db -m ping
````

Step 6: Testing the Connection
Test the connection between the controller and agent nodes using the ping module:

```bash

sudo ansible web -m ping
sudo ansible db -m ping
```

## Ad-hoc Commands

Ad-hoc commands in Ansible are one-liners that can be executed directly from the command line without the need for separate playbooks or scripts. Here are some examples:

Check the OS version on the agent node:

```bash

sudo ansible web -a "uname -a"
```

Check the time zone on the agent node:

```bash

sudo ansible web -a "date"
```

Check free space on the agent node:

```bash

sudo ansible web -a "free"
```

List installed packages on the agent node:

```bash

sudo ansible web -a "ls -a"
```

Playbooks - Automating Tasks
To automate tasks, we use playbooks. Here's an example playbook to install Nginx on the web node and configure its security group to allow port 80.

Create a playbook named nginx.yml in the default location /etc/ansible:

```yaml


---

# Which host to perform the task

- hosts: web

# See the logs by gathering facts

gather_facts: yes

# Admin access (sudo)

become: true

# Add the instructions - install Node.js 12 with PM2 and run the app

tasks:

- name: Installing Node.js v12 - Adding the GPG key for Node.js
  apt_key:
  url: "https://deb.nodesource.com/gpgkey/nodesource.gpg.key"
  state: present

- name: Add NodeSource repository
  apt_repository:
  repo: "deb https://deb.nodesource.com/node_12.x {{ ansible_distribution_release }} main"
  state: present

- name: Install Node.js
  apt:
  name: nodejs
  state: present
  update_cache: yes

- name: Install PM2
  npm:
  name: pm2
  global: yes
  state: present
  version: "4.5.6"

- name: Install app dependencies
  command: npm install
  args:
  chdir: "/home/ubuntu/sparta-app/sparta-app/app"

- name: Start the Node.js app
  command: pm2 start app.js
  args:
  chdir: "/home/ubuntu/sparta-app/sparta-app/app"
  Execute the playbook using the following command:
```

```bash

sudo ansible-playbook nginx.yml
```

The playbook will install Nginx on the web node, and the output will indicate success.

## For installation of mongodb on the db vm

```yaml
---
# create a playbook to install mongodb in db-instance

# host
- hosts: db

  # get logs
  gather_facts: yes

  # admin access
  become: true

  # provide instructions - task: install mongodb
  tasks:
    # install mongodb
    - name: Installing MongoDB
      apt: pkg=mongodb state=present
# ensure the db is running

# check the status if its running with adhoc commands
```

You can use this coommand to get more info
`sudo ansible-playbook mongodb-playbook.yml -vvv`

At the end run `sudo ansible db -a "systemctl status mongodb"` to check if mongodb is running
