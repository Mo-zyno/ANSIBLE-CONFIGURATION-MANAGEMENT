
# ANSIBLE CONFIGURATION MANAGEMENT

## Ansible Client as a Jump Server (Bastion Host)

## A Jump Server (sometimes also referred as Bastion Host) is an intermediary server through which access to internal network can be provided. If you think about the current architecture you are working on, ideally, the webservers would be inside a secured network which cannot be reached directly from the Internet. That means, even DevOps engineers cannot SSH into the Web servers directly and can only access it through a Jump Server – it provide better security and reduces attack surface ##.

## On the diagram below the Virtual Private Network (VPC) is divided into two subnets – Public subnet has public IP addresses and Private subnet is only reachable by private IP addresses.


![intro image](https://user-images.githubusercontent.com/97651517/161695501-88f932ca-4847-4702-a453-c7a212f51d68.png)


## Task

1. Install and configure Ansible client to act as a Jump Server/Bastion Host

2. Create a simple Ansible playbook to automate servers configuration

## STEP 1 - INSTALL AND CONFIGURE ANSIBLE ON EC2 INSTANCE

1. Update Name tag on your Jenkins EC2 Instance to Jenkins-Ansible. We will use this server to run playbooks.

![Ansible Instance](https://user-images.githubusercontent.com/97651517/161695953-cc347777-6b81-47ba-b3c1-e0412a4fcffb.png)


2. In your GitHub account create a new repository and name it ansible-config-mgt

![ansible config mgt](https://user-images.githubusercontent.com/97651517/161696268-306c45e7-9516-4a66-bf89-ba8501f249e5.png)

3. Update Ansible Server and Install Ansible

- Ansible Terminal

![Ansible terminal](https://user-images.githubusercontent.com/97651517/161696359-9a82ab8c-8f4d-4468-9211-1ae92c09b8ba.png)

`sudo apt update`

`sudo apt upgrade`

`sudo apt install ansible`

![sudo apt update](https://user-images.githubusercontent.com/97651517/161696528-1e66ef2b-f74c-4f7e-bd06-40a7598ad05c.png)


![sudo apt upgrade](https://user-images.githubusercontent.com/97651517/161696545-426d20a8-8462-4dce-b051-354293f28042.png)


![sudo apt install ansible](https://user-images.githubusercontent.com/97651517/161696562-ac169dbd-abfb-472d-bdd8-2bfe26b5c157.png)


**Check your Ansible version by running ansible --version*

`ansible --version`


![ansible version](https://user-images.githubusercontent.com/97651517/161696952-0a1d1b6b-8a3c-4120-8890-00bc1d4b41da.png)


4. Configure Jenkins build job to save your repository content every time you change it

- Create a new Freestyle project ansible in Jenkins and point it to your ‘ansible-config-mgt’ repository.

- Configure Webhook in GitHub and set webhook to trigger ansible build.

- Configure a Post-build job to save all (**) files.


![free style ansible](https://user-images.githubusercontent.com/97651517/161697297-66eb263e-d6b7-4642-8ddd-d66a704dfaa0.png)

![source code mgt](https://user-images.githubusercontent.com/97651517/161697360-8a2a75c0-379a-408e-ba44-5f4fa50b3311.png)

![new webhook](https://user-images.githubusercontent.com/97651517/161697428-36333ef8-329e-4760-97fb-7cdb122ef788.png)

![github jenkins URL](https://user-images.githubusercontent.com/97651517/161697476-1b9e966a-1610-4ee6-a9e8-467f83237847.png)

![archive artifact 1](https://user-images.githubusercontent.com/97651517/161697567-44d7c131-4fab-4e71-9ab8-f091793b6f1a.png)

![archive artifact 2](https://user-images.githubusercontent.com/97651517/161697582-261fea1d-e74d-4851-97eb-9dac24fba53d.png)

NB: had error from jenkins build trigger from github


![fixing error config](https://user-images.githubusercontent.com/97651517/161697975-b57822d7-a139-4cc0-9078-450d59d53e4f.png)

![fixing error back to project](https://user-images.githubusercontent.com/97651517/161697987-35085a27-d7b5-402e-84a5-9bb18e3ac384.png)


NB: Github doesn't uses word "master" anymore instead it uses "main"


![fixing error master to main](https://user-images.githubusercontent.com/97651517/161698271-900240f0-890c-4536-a62f-d93111cb3b06.png)


![build succesful after fixing error](https://user-images.githubusercontent.com/97651517/161698365-8046c895-daf3-4f8f-8de7-ceb164844b9e.png)


![multiple succesful build triggerd](https://user-images.githubusercontent.com/97651517/161698430-3dedd7ee-5145-4ea0-812a-b3769695d43b.png)


5. Test your setup by making some change in README.MD file in master branch and make sure that builds starts automatically and Jenkins saves the files (build artifacts) in following folder


ls /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/


![chnages made showing](https://user-images.githubusercontent.com/97651517/161698663-64e1046f-2b53-49e0-87e3-b33da25eb0fb.png)


## Note: 

**Trigger Jenkins project execution only for /main (master) branch*

**Now your setup will look like this**:

![new set up](https://user-images.githubusercontent.com/97651517/161698818-c11859fc-6150-4f5f-9ba3-e9e5ba4e7868.png)



## STEP 2 – PREPARE YOUR DEVELOPMENT ENVIRONMENT USING VISUAL STUDIO CODE (Vscode).

**First part of ‘DevOps’ is ‘Dev’, which means you will require to write some codes and you shall have proper tools that will make your coding and debugging comfortable – you need an Integrated development environment (IDE) or Source-code Editor. There is a plethora of different IDEs and Source-code Editors for different languages with their own advantages and drawbacks, you can choose whichever you are comfortable with, but we recommend one free and universal editor that will fully satisfy your needs – Visual Studio Code (VSC)**.

**After you have successfully installed VSC, you will have to install an extentsion called REMOTE DEVELOPMET for ssh purposes to your instances, configure Vscode to connect to your newly created GitHub repository*


![install remote dev pack in visual code](https://user-images.githubusercontent.com/97651517/161699020-5eccfbc7-9506-4b12-b28d-8ba48ccb7486.png)


![vscode document](https://user-images.githubusercontent.com/97651517/161699058-7ccc447d-fc7f-4f19-9727-863076489078.png)


# BEGIN ANSIBLE DEVELOPMENT

1. In your ansible-config-mgt GitHub repository, create a new branch that will be used for development of a new feature.

`git status`

`git checkout prj-11`

`git branch`

Tip: 

Give your branches descriptive and comprehensive names, for example, if you use Jira or Trello as a project management tool – include ticket number (e.g. PRJ-145) in the name of your branch and add a topic and a brief description what this branch is about – a bugfix, hotfix, feature, release (e.g. feature/prj-145-lvm)

2. Checkout the newly created feature branch to your local machine and start building your code and directory structure.


![create branch, mkdir playbooks and inventory](https://user-images.githubusercontent.com/97651517/161699299-0f46367f-c52c-4155-8746-120e343f7551.png)


3. Create a directory and name it playbooks – it will be used to store all your playbook files.

4. Create a directory and name it inventory – it will be used to keep your hosts organised.


5. Within the playbooks folder, create your first playbook, and name it common.yml

6. Within the inventory folder, create an inventory file (.yml) for each environment (Development, Staging Testing and   Production) dev, staging, uat, and prod respectively.


![created  yml file](https://user-images.githubusercontent.com/97651517/161699546-d386248d-6e83-4c81-ad5a-4d01aa71c6e6.png)


## STEP 4 – SET UP AN ANSIBLE INVENTORY

### An Ansible inventory file defines the hosts and groups of hosts upon which commands, modules, and tasks in a playbook operate. Since our intention is to execute Linux commands on remote hosts, and ensure that it is the intended configuration on a particular server that occurs. It is important to have a way to organize our hosts in such an Inventory.

**Save below inventory structure in the inventory/dev file to start configuring your development servers. Ensure to replace the IP addresses according to your own setup*.

Note: 

### Ansible uses TCP port 22 by default, which means it needs to ssh into target servers from Jenkins-Ansible host – for this you can implement the concept of ssh-agent. Now you need to import your key into ssh-agent:

`ssh-agent -s`

`ssh-add <path-to-private-key>`


*Confirm the key has been added with the command below, you should see the name of your key*

`ssh-add -l`


![persist key on server](https://user-images.githubusercontent.com/97651517/161699885-e625d027-176b-49d3-8976-731529f86c8a.png)

### Now, ssh into your Jenkins-Ansible server using ssh-agent


`ssh -A ubuntu@public-ip`


![successful connection after trouble shooting](https://user-images.githubusercontent.com/97651517/161700026-fe6a73a7-3054-44eb-9540-4b77659bcac7.png)


*Ansible connecting to other Servers.*


![Ansible connecting succesfuly to LB](https://user-images.githubusercontent.com/97651517/161700339-ee10f3e1-058e-45fc-90a8-62088530b47d.png)


![Ansible connecting to my NFS server](https://user-images.githubusercontent.com/97651517/161700354-799b454b-e381-4c22-8081-532d30eee86c.png)


**Update your inventory/dev.yml file with this snippet of code:*


![dev yml file](https://user-images.githubusercontent.com/97651517/161700533-5118d0f8-a475-4628-9cbb-fc976ce08be9.png)



# CREATE A COMMON PLAYBOOK


## STEP 5 – CREATE A COMMON PLAYBOOK

### It is time to start giving Ansible the instructions on what you needs to be performed on all servers listed in inventory/dev.

### In common.yml playbook you will write configuration for repeatable, re-usable, and multi-machine tasks that is common to systems within the infrastructure.


**Update your playbooks/common.yml file with following code:*


![editing ansible config file](https://user-images.githubusercontent.com/97651517/161700690-59f4d1e2-9c87-45e1-9daa-41615054bc40.png)


### Examine the code above and try to make sense out of it. This playbook is divided into two parts, each of them is intended to perform the same task: install wireshark utility (or make sure it is updated to the latest version) on your RHEL 8 and Ubuntu servers. It uses root user to perform this task and respective package manager: yum for RHEL 8 and apt for Ubuntu.

### Feel free to update this playbook with following tasks:

- Create a directory and a file inside it

- Change timezone on all servers

- Run some shell script.


## STEP 6 – UPDATE GIT WITH THE LATEST CODE.

### Now all of your directories and files live on your machine and you need to push changes made locally to GitHub.

### In the real world, you will be working within a team of other DevOps engineers and developers. It is important to learn how to collaborate with help of GIT. In many organisations there is a development rule that do not allow to deploy any code before it has been reviewed by an extra pair of eyes – it is also called "Four eyes principle".

### Now you have a separate branch, you will need to know how to raise a Pull Request (PR), get your branch peer reviewed and merged to the master branch.

- Commit your code into GitHub:

1. Use git commands to add, commit and push your branch to GitHub.


`git status`

`git add <add files>`

`git commit -m "commit message`


![Git status, add and commit](https://user-images.githubusercontent.com/97651517/161701154-e8d2126a-1d04-4aed-9e06-282c256475c3.png)


`git push origin prj-11`


![Git push](https://user-images.githubusercontent.com/97651517/161701308-97d5ed94-0710-4dcc-a2c4-6063c562899f.png)


2. Create a Pull request (PR)

![create a pull request](https://user-images.githubusercontent.com/97651517/161701670-18951f96-62ee-46aa-82ef-353009f4a1c8.png)


3. If you are satisfied with your new feature development, merge the code to the master branch.

![merge pull request](https://user-images.githubusercontent.com/97651517/161702144-cec6d88c-b284-4cb5-94bf-2e998acc0253.png)


![confirm merge](https://user-images.githubusercontent.com/97651517/161701949-63e75ebf-e633-4091-9b36-23d9d3b5f2c0.png)


![merge successfuly](https://user-images.githubusercontent.com/97651517/161702246-6bca3a78-45e0-4a20-a0f3-a60e7c509b96.png)


![inventory and playbooks in github](https://user-images.githubusercontent.com/97651517/161702347-d34b404a-6450-4f0f-9083-64274d97e788.png)


![inventory dev yml file in git](https://user-images.githubusercontent.com/97651517/161702504-2b98155a-2c6d-4fbe-b544-eaa925ae4d61.png)


![playbook common file](https://user-images.githubusercontent.com/97651517/161702524-0d357d51-16e3-4644-9955-c3bd6abecc7b.png)

**Jenkins triggered new build from github*


![Jenkins build Ansible](https://user-images.githubusercontent.com/97651517/161702829-e66bcf09-ea59-47bd-8fcf-5299ba2d87a3.png)


![multiple succesful build triggerd](https://user-images.githubusercontent.com/97651517/161702861-d47d99b8-e698-49c7-a038-fdb32457d007.png)


![view in Jenkins](https://user-images.githubusercontent.com/97651517/161702899-37182551-6338-40e4-8731-53adf584753a.png)



4. Head back on your terminal, checkout from the feature branch into the master, and pull down the latest changes.


![git checkout status and pull](https://user-images.githubusercontent.com/97651517/161703181-de5886f0-0e5d-4069-9774-7b49e8bf38bb.png)


5. Once your code changes appear in master branch – Jenkins will do its job and save all the files (build artifacts) to /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/ directory on Jenkins-Ansible server.


![files in Ansible server](https://user-images.githubusercontent.com/97651517/161703342-470760bf-a521-4217-a45b-b383d4b0ad14.png)


![Ansible server showing all  yml files](https://user-images.githubusercontent.com/97651517/161703410-73ba0082-f4fd-42b5-a066-c1eb7630c4e9.png)



# RUN FIRST ANSIBLE TEST

## STEP 7 – RUN FIRST ANSIBLE TEST.

### Now, it is time to execute ansible-playbook command and verify if your playbook actually works:

`ansible-playbook -i /var/lib/jenkins/jobs/ansible/builds/<build-number>/archive/inventory/dev.yml /var/lib/jenkins/jobs/ansible/builds/<build-number>/archive/playbooks/common.yml`


![running playbook](https://user-images.githubusercontent.com/97651517/161703598-1abf2fac-fe1e-408c-90ba-23bc8523a3ec.png)


![succesfully creat a playbook](https://user-images.githubusercontent.com/97651517/161703667-95fcaa83-40f4-43e9-80bf-ee40559fb0f1.png)


Note: 

**Previous command we ran without sudo, this is because we had added an ssh key to ssh-agent for our regular user. If you try to run this command with sudo you will have to explicitly pass the ssh key with --private-key <path-to-private-key> parameter.*

### You can go to each of the servers and check if wireshark has been installed by running which wireshark or wireshark --version


![web1 wireshark](https://user-images.githubusercontent.com/97651517/161703865-431dcf39-2d55-4b3c-a8c8-6fad7ced5c9f.png)


![web2 wireshark](https://user-images.githubusercontent.com/97651517/161703879-68a011cb-2523-4943-995d-9cce0f324044.png)


![db wireshark](https://user-images.githubusercontent.com/97651517/161703948-6f4a2e82-9d38-484e-9e9c-84ba61c2d375.png)


![LB wireshark](https://user-images.githubusercontent.com/97651517/161703975-3565f879-114a-48d1-9ad3-8bb56e8eb371.png)


![NFS wireshark](https://user-images.githubusercontent.com/97651517/161703999-15e526d9-8f6b-4498-8798-e5f8567b5faf.png)


### Your updated with Ansible architecture now looks like this:


![final result](https://user-images.githubusercontent.com/97651517/161704443-45772a85-5273-41e2-aa5e-00c276578a11.png)

  
### Update your ansible playbook with some new Ansible tasks and go through the full checkout -> change codes -> commit -> PR -> merge -> build -> ansible-playbook cycle again to see how easily you can manage a servers fleet of any size with just one command!

  
![adding directory and time zone in playbook](https://user-images.githubusercontent.com/97651517/161704614-7c6e8e06-0ea2-46ad-952a-82a219ac8dc8.png)

  
![timezone chnaged](https://user-images.githubusercontent.com/97651517/161704714-45e03b61-5065-40d0-94f6-acd5c4304251.png)

  
## And that Concludes this project.


