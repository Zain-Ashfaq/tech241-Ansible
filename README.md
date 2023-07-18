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
   For Ubuntu: `sudo apt install ansible -y`
   Update the package manager:
   For Ubuntu: `sudo apt update`
   Install Ansible:
   For Ubuntu: `sudo apt install ansible -y`

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
