# ANSIBLE CONFIGURATION MANAGEMENT

## Ansible Client as a Jump Server (Bastion Host)

## A Jump Server (sometimes also referred as Bastion Host) is an intermediary server through which access to internal network can be provided. If you think about the current architecture you are working on, ideally, the webservers would be inside a secured network which cannot be reached directly from the Internet. That means, even DevOps engineers cannot SSH into the Web servers directly and can only access it through a Jump Server – it provide better security and reduces attack surface ##.

## STEP 1 - INSTALL AND CONFIGURE ANSIBLE ON EC2 INSTANCE

## STEP 2 – PREPARE YOUR DEVELOPMENT ENVIRONMENT USING VISUAL STUDIO CODE (Vscode).

**First part of ‘DevOps’ is ‘Dev’, which means you will require to write some codes and you shall have proper tools that will make your coding and debugging comfortable – you need an Integrated development environment (IDE) or Source-code Editor. There is a plethora of different IDEs and Source-code Editors for different languages with their own advantages and drawbacks, you can choose whichever you are comfortable with, but we recommend one free and universal editor that will fully satisfy your needs – Visual Studio Code (VSC)**.

**After you have successfully installed VSC, you will have to install an extentsion called REMOTE DEVELOPMET for ssh purposes to your instances, configure Vscode to connect to your newly created GitHub repository*


## STEP 4 – SET UP AN ANSIBLE INVENTORY

### An Ansible inventory file defines the hosts and groups of hosts upon which commands, modules, and tasks in a playbook operate. Since our intention is to execute Linux commands on remote hosts, and ensure that it is the intended configuration on a particular server that occurs. It is important to have a way to organize our hosts in such an Inventory.

**Save below inventory structure in the inventory/dev file to start configuring your development servers. Ensure to replace the IP addresses according to your own setup*.


Note: 

### Ansible uses TCP port 22 by default, which means it needs to ssh into target servers from Jenkins-Ansible host – for this you can implement the concept of ssh-agent. Now you need to import your key into ssh-agent:


## STEP 5 – CREATE A COMMON PLAYBOOK

### It is time to start giving Ansible the instructions on what you needs to be performed on all servers listed in inventory/dev.

### In common.yml playbook you will write configuration for repeatable, re-usable, and multi-machine tasks that is common to systems within the infrastructure.

## STEP 6 – UPDATE GIT WITH THE LATEST CODE.

### Now all of your directories and files live on your machine and you need to push changes made locally to GitHub.

### In the real world, you will be working within a team of other DevOps engineers and developers. It is important to learn how to collaborate with help of GIT. In many organisations there is a development rule that do not allow to deploy any code before it has been reviewed by an extra pair of eyes – it is also called "Four eyes principle".

## STEP 7 – RUN FIRST ANSIBLE TEST.

### Now, it is time to execute ansible-playbook command and verify if your playbook actually works:

