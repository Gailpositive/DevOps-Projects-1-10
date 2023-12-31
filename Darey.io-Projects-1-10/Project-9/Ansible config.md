# ANSIBLE CONFIGURATION MANAGEMENT -AUTOMATE-

## Ansible Client as a Jump Server (Bation Host)
A Jump Server (sometimes also referred as Bastion Host) is an intermediary server through which access to internal network can be provided. If you think about the current architecture you are working on, ideally, the webservers would be inside a secured network which cannot be reached directly from the Internet. That means, even DevOps engineers cannot SSH into the Web servers directly and can only access it through a Jump Server – it provide better security and reduces attack surface. On the diagram below the Virtual Private Network (VPC) is divided into two subnets – Public subnet has public IP addresses and Private subnet is only reachable by private IP addresses.

  
![image](https://github.com/travdevops/PBL-DevOps/assets/111061512/ff06fbe9-2c59-4d05-b7f4-e059055da8b4)


## TASKS
* Install and configure Ansible client to act as a Jump Server/Bastion Host
* Create a simple Ansible playbook to automate servers configuration


## STEP 1: INSTALL AND CONFIG ANSIBLE ON UBUNTU INSTANCE.
1. Open Jenkins and  Name the server Jenkins-ansible 
2. In my github account ,create a new repo called "ansible-config.mgt"
3. Install ansible.

* First, I create unbuntu ec2 instance
* I update my server with the command "Sudo apt update"
* To install ansible, I run the command "sudo apt install ansible -y
* To check the version, I run the command "ansible--version"
* Notice there is no ansible config file 
<img width="814" alt="Ansible version 101" src="https://github.com/travdevops/PBL-DevOps/assets/111061512/c6e95bab-3f3b-4adc-971d-e3d142396c38">

* To install ansible config file, I use the "touch" command to create it
<img width="822" alt="ansible 102" src="https://github.com/travdevops/PBL-DevOps/assets/111061512/0f806fbc-d954-4cf0-805c-892a48abe10b">

 ## 4,  STEP 2: CONFIG JENKINS TO BUILD JOB TO ARCHIVE MY REPOSITORY CONTENT EVERY TIME I SAVE IT.
* Create a freestyle project "ansible" in Jenkens and point it to "ansible-config.mgt" repository
* Configure a webhook in github and set the webhook to trigger ansible build
* Configure a Post-build job to save all (**) files.

* Ansible freestyle job pointing to "ansible-config.mgt repo
 <img width="960" alt="config1a" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/71b3ee57-9196-4d75-ac21-08debbd2737c">

* Github http url 
<img width="918" alt="Config2" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/398343d8-85ef-4051-8280-59947a58674b">

* Specify  "/main" branch
* <img width="953" alt="Config3" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/0d8086dd-d14a-49e9-87fb-f9b4effe504b">

* Set Github webhook trigger 
<img width="939" alt="Config4" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/d9ee7881-8862-4e2c-8ac4-1bf4fdb38870">

* On build steps, build job configured to save (**) files
<img width="952" alt="Config5" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/d5c970fe-c906-442a-b85d-52046ecb5d75"> 

## 5, STEP 3: TEST SETUP BY MAKING SOME CHANGINGS IN README.MD FILE IN MAIN BRANCH , MAKE SURE BUILDS STARTS AUTOMATICALLY AND JENKINS BUILDS ARTIFACTS IN FOLLOWING ORDER.

* Webhook successfully created
<img width="844" alt="webhook sucessful" src="https://github.com/Gailpositive/ansible-config-mgt/assets/111061512/cc13f592-8795-414c-bda6-bda558a60bf1">

* Readme file editted
<img width="635" alt="testing the readme" src="https://github.com/Gailpositive/ansible-config-mgt/assets/111061512/12ffc663-38e2-43d5-947d-97357ff8c02c">

* Jenkins builds artifacts automatically
<img width="284" alt="testing jenkins  builds auto" src="https://github.com/Gailpositive/ansible-config-mgt/assets/111061512/51130079-aa69-4a44-8b41-e197aea385c6">

ls /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/

* On my Terminal
<img width="655" alt="j terminal" src="https://github.com/Gailpositive/ansible-config-mgt/assets/111061512/f9797ef6-8843-4367-bd7a-06dd75fc0ebd">

## Note: Trigger Jenkins project execution only for /main (master) branch.

Now, my setup looks like this:
![image](https://github.com/travdevops/PBL-DevOps/assets/111061512/db84f534-eed6-4689-a4e2-16bd9f08e9f5)


## Note: everytime I stop/start my my Ansible server, I will have to reconfig github webhook Ip address. To avoid this, I can allocate an Elastic Ip to the server.Elastic IP is only free when allocated to an EC2 instance, so remenber to release Elastic Ip once I terminate Instance.

 ## 6, STEP 4: PREPARING THE DEVELOPMENT AREA USING VSCODE
Development ‘DevOps’ means I will require to write some codes and I shall have proper tools that will make my coding and debugging comfortable. I need an Integrated development environment (IDE) or Source-code Editor. There is a plethora of different IDEs and Source-code Editors for different languages with their own advantages and drawbacks, I can choose whichever I am comfortable with. Visual Studio Code is what I will use in this project.

* After you have successfully installed VSC, I 'll configure it to connect to my newly created GitHub repository: "ansible-config-mgt".

 
 * Config to vscode via codespace in github
 <img width="960" alt="git in vscode" src="https://github.com/Gailpositive/ansible-config-mgt/assets/111061512/9883645e-fe77-47b6-8d4f-29e3559249d7">
 
 * Clone down ansible-config-mgt repo to my Jenkins-Ansible instance with the command: "git clone <ansible-config-mgt repo link>"
<img width="569" alt="git clone ansible repo" src="https://github.com/Gailpositive/ansible-config-mgt/assets/111061512/938dbe9c-7bb3-43a7-a218-c52e24ed1601">

<img width="679" alt="updated connect host" src="https://github.com/Gailpositive/ansible-config-mgt/assets/111061512/0a387391-4491-4612-8c32-6c5def8c917b">

## 7, STEP 5: BEGIN ANSIBLE DEVELOPMENT
* In my ansible-config-mgt GitHub repository, I create a new branch called "Development" which I latter change to "hotfix" , that will be used for development of a new feature.
  
* Tip: Give the branches descriptive and comprehensive names, for example, if I use Jira or Trello as a project management tool – include ticket number (e.g. PRJ-145) in the name of my branch and add a topic and a brief description what this branch is about – a bugfix, hotfix, feature, release (e.g. feature/prj-145-lvm)

<img width="468" alt="developemnt branch" src="https://github.com/Gailpositive/ansible-config-mgt/assets/111061512/ccce6bf0-fc09-4e3a-a0a2-c4d71faf9c65">

* Checkout/switch to the newly created branch "Development"
<img width="409" alt="git 3 created new branch" src="https://github.com/Gailpositive/ansible-config-mgt/assets/111061512/d4aa0861-ed4e-4f1a-8490-9019e8aa197a">
 
* Create a directory and name it "playbook"
* Create another directory and name it "inventory"
* Within the playbooks folder, create a playbook file called "common.yml"
* Within the inventory folder, create an inventory file each for the following environments :Dev, staging, uat (testing), prod respectively
* These inventory files uses .ini languages to style to config ansible host
<img width="569" alt="git 6" src="https://github.com/Gailpositive/ansible-config-mgt/assets/111061512/26e0ec32-9c3a-4375-a146-3c66e5a67f0c">
<img width="492" alt="git 8" src="https://github.com/Gailpositive/ansible-config-mgt/assets/111061512/10e300ca-ef00-4651-99c6-a36316917da4">

<img width="916" alt="git 9" src="https://github.com/Gailpositive/ansible-config-mgt/assets/111061512/a969dca0-8c26-43ba-87ea-3d0d05ae97eb">

## 8, Step 4, SET AN ANSIBLE INVENTORY
* An Ansible inventory file defines the hosts and groups of hosts upon which commands, modules, and tasks in a playbook operate. Since our intention is to execute Linux commands on remote hosts, and ensure that it is the intended configuration on a particular server that occurs. It is important to have a way to organize our hosts in such an Inventory.
* Save below inventory structure in the inventory/dev file to start configuring the development servers. Ensure to replace the IP addresses according to my own setup.
* Note: Ansible uses TCP port 22 by default, which means it needs to ssh into target servers from Jenkins-Ansible host – for this I can implement the concept of ssh-agent.

* Now I need to import my key into ssh-agent: To learn how to setup SSH agent and connect VS Code to your Jenkins-Ansible instance, please see this video:
* • ( https://www.youtube.com/watch?v=OplGrY74qog) for Windows users – ssh-agent on windows
* • ( https://www.youtube.com/watch?v=OplGrY74qog) for Linux users – ssh-agent on linux

  ## I setup my SSH agent using my windows power shell terminal using the following script:

*  Get-WindowsCapability -Online | Where-Object Name -like 'OpenSSH*'

*   Name  : OpenSSH.Client~~~~0.0.1.0
State : NotPresent

*  Name  : OpenSSH.Server~~~~0.0.1.0
State : NotPresent

* Start the sshd service
"Start-Service sshd"

* OPTIONAL but recommended:
"Set-Service -Name sshd -StartupType 'Automatic'"

* Confirm the Firewall rule is configured. It should be created automatically by setup. Run the following to verify
" (!(Get-NetFirewallRule -Name "OpenSSH-Server-In-TCP" -ErrorAction SilentlyContinue | Select-Object Name, Enabled)) {
    Write-Output "Firewall Rule 'OpenSSH-Server-In-TCP' does not exist, creating it..."
    New-NetFirewallRule -Name 'OpenSSH-Server-In-TCP' -DisplayName 'OpenSSH Server (sshd)' -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
} else {
    Write-Output "Firewall rule 'OpenSSH-Server-In-TCP' has been created and exists."
}

<img width="915" alt="powershell1" src="https://github.com/travdevops/PBL-DevOps/assets/111061512/ff78a63e-f860-4548-be48-cb172d63ed28">
<img width="960" alt="powershell2" src="https://github.com/travdevops/PBL-DevOps/assets/111061512/68e32bc7-55aa-44a0-9a2b-3b4d450a850e">
<img width="945" alt="powershell3" src="https://github.com/travdevops/PBL-DevOps/assets/111061512/c9ae04d3-3de4-4c0a-ad0c-160b22fc6988">

## Import my key into SSH agent
* eval `ssh-agent -s`
* ssh-add <path-to-private-key>
*Confirm the key has been added with the command below, you should see the name of your key:
* ssh-add -l
* Now,ssh into your Jenkins-Ansible server using ssh-agent:
* ssh -A ubuntu@public-ip
<img width="588" alt="process to loging in remotely via ssh 9" src="https://github.com/travdevops/PBL-DevOps/assets/111061512/1a5e1cb4-422d-4601-b0f5-bd1a8ef37ba1">

* From the Jenkins-Ansible Controller, ssh into all the remote servers with the same command ssh user@public-ip and install wireshark with the command:
* For ubuntu: "sudo apt install wireshark" and "wireshark --version" to check the version
* For RHEL: "sudo yum install wireshark" and "wireshark --version" to check version 
<img width="702" alt="ssh into all Nodes 7" src="https://github.com/travdevops/PBL-DevOps/assets/111061512/0d069913-043a-484c-b060-2479d6b71a17">

* I currently have a few servers  I want Ansible and ssh to talk to, to be able to run our playbooks:
* 1 Nfs server - RHEL-based
* 2 Webservers - RHEL-based
* 1 Database Server - RHEL-based
* 1 Loadbalancer Server - Ubuntu-based
  
* Note: Ubuntu-based servers user is "ubuntu" and user for RHEL-based servers is "ec2-user".

## On vscode, Update my inventory/dev.yml file with the snippet of code below:
* [nfs]
 172.31.24.221 ansible_ssh_user='ec2-user'

* [webservers]
172.31.22.59 ansible_ssh_user='ec2-user'
172.31.25.29 ansible_ssh_user='ec2-user'

* [db]
172.31.27.205 ansible_ssh_user='ec2-user'

* lb]
172.31.23.195 ansible_ssh_user='ubuntu'

<img width="814" alt="inventory file modified 8" src="https://github.com/Gailpositive/ansible-config-mgt/assets/111061512/45868885-91ab-4106-99cc-59b10167fb33">

## STEP 5: CREATE A COMMON PLAYBOOK
* It is time to start giving Ansible the instructions needed to be performed on all servers listed in inventory/dev.

* In common.yml playbook, I will write configuration for repeatable, re-usable, and multi-machine tasks that is common to systems within the infrastructure.

## Update my playbooks/common.yml file with following code:

*  ---
- name: update web, nfs and db servers
  hosts: webservers, nfs, db
  remote_user: ec2-user
  become: yes
  become_user: root
  tasks:
    - name: ensure wireshark is installed and at its latest version
      yum:
        name: wireshark
        state: latest

- name: update LB server
  hosts: lb
  remote_user: ubuntu
  become: yes
  become_user: root
  tasks:
    - name: Update apt repo
      apt: 
        update_cache: yes

    - name: ensure wireshark is installed and at its latest version
      apt:
        name: wireshark
        state: latest
<img width="884" alt="create a common playbook step 5 7" src="https://github.com/Gailpositive/ansible-config-mgt/assets/111061512/06d25d97-34bf-4980-81d7-ba008d377a16">
* Examine the code above and try to make sense out of it. This playbook is divided into two parts, each of them is intended to perform the same task: install wireshark utility (or make sure it is updated to the latest version) on my RHEL and Ubuntu servers. It uses root user to perform this task and respective package manager: yum for RHEL and apt for Ubuntu.
* Feel free to update this playbook with the following tasks:
* Create a directory and a file inside
* Create time zones on all servers
* Run some shell script

* For a better understanding of Ansible playbooks – watch this video (https://youtu.be/ZAdJ7CdN7DY) from RedHat and read this article(https://www.redhat.com/en/topics/automation/what-is-an-ansible-playbook).

## Step 6: Update git with the latest code
* Now all of my directories and files live on my machine
* git switch to the local development "hotfix" branch,  Merge them to the local main branch and push the changes made locally to the remote GitHub.
* Working with an organization, It is always advicable to make a pull request first to update changes made remotely down to the local branch before pushing the changes made on the local branch.

* Commit your code into GitHub:
* use git commands to add, commit and push your branch to GitHub.

* `git status`

* `git add <selected files>`

* `git commit -m "commit message"`


* Create a Pull request (PR)

* Act as a reviewer. If the reviewer is happy with your hotfix development, merge the code to the main/master branch.

* Head back on your terminal, checkout from the hotfix branch into the main/master, and pull down the latest changes.

<img width="782" alt="git pull 12" src="https://github.com/Gailpositive/ansible-config-mgt/assets/111061512/f86bae87-d02f-4ce2-8047-74e2e1161ced">

* Once your code changes appear in main/master branch – Jenkins will do its job and save all the files (build artifacts) to /var/lib/jenkins/jobs/ansible/builds/<build_number>/archive/ directory on Jenkins-Ansible server.

<img width="579" alt="build artifact updated" src="https://github.com/Gailpositive/ansible-config-mgt/assets/111061512/b901d826-ad93-4a1c-955a-14c5dfb357c7">

## Step 7- Run First Ansible Test
* Now, it is time to execute ansible-playbook command and verify if your playbook actually works:
* On the Jenkins-Ansible controller server,
* "cd ansible-config-mgt"
* Firstly, we will try to ping all the servers just to check if our ansible connection is setup properly with the ansible ping module
* "ansible all -i inventory/dev.yml -m ping"
*<img width="960" alt="run playbooks successfull" src="https://github.com/Gailpositive/ansible-config-mgt/assets/111061512/b4307bd3-bdb4-4710-804f-472d2f0b6c83">
* Only three Nodes were successful

* Now, I will run our playbooks.
* "ansible-playbook -i inventory/dev.yml playbooks/common.yml"


* Optional step – Repeat once again - Another Task.

* Update my ansible playbook with some new Ansible tasks

* Here, I will ask ansible to install Nginx

* Go through the full checkout -> change codes -> commit -> PR -> merge -> build -> ansible-playbook cycle again to run it.
* SEE HOW EASILY I CAN MANAGE A SERVERS FLEET OF ANY SIZE WITH JUST ONE COMMAND!


* My updated  Ansible architecture now looks like this:
![image](https://github.com/Gailpositive/ansible-config-mgt/assets/111061512/01af22b4-5fa6-46cf-a775-b33da861ab3a)
## Congratulations-On-The-Automating-Of-My-Routine-Tasks-By-Implementing-My-First-Ansible-Project)


![image](https://github.com/Gailpositive/ansible-config-mgt/assets/111061512/32f46fcf-04c7-4ebe-8df3-ce97f7df8aa9)

