# BUILDING A COMPREHENSIVE BUSINESS  WEBSITE SOLUTION AND INTEGRATION OF VARIOUS TOOLS AND TECHNOLOGIES.
## A THREE-TIER WEB APPLICATION ARCHITECTURE, A SINGLE REMOTE MYSQL DATABASE AND AN NFS SERVER AS SHARED BACKEND STORAGE FILES. 
## Solution Prerequisites: 
1 Infrastruction: AWS.
2  Three-Tier Webservers Linux: Redhat.  
3  Database Server: Ubuntu + Mysql (Preferable but I used Redhat). 
4  Storage server: Redhat + NFS Server. 
5  Programming language: PHP. 
6  Code Repository: Github. 

* On the diagram below you can see a common pattern where several stateless Web Servers share a common database and also access the same files using Network File Sytem (NFS) as a shared file storage. Even though the NFS server might be located on a completely separate hardware – for Web Servers it look like a local file system from where they can serve the same files.

  <img width="917" alt="1" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/2733229b-7cff-437e-9495-fffb5fdb353e">

<img width="873" alt="2" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/a28cf063-dbaf-436b-82ce-a19164114512">



## NOTE: Not all storage is suitable, hence it is important to know the suitable storage solution for every use case. Such as: what data will be stored, in what format, how this data will be accessed, by whom, from where, how frequently etc. Based on theses questions, I will be able to choose the right suitable storage for my solution. 

## SELF STUDY:
* Network attach storage (NAS) https://en.wikipedia.org/wiki/Network-attached_storage
* Storage area network (SAN) https://en.wikipedia.org/wiki/Storage_area_network, and related protocol like NFS(s), FTP, SMB, iSCSI.
* Block-level storage https://en.wikipedia.org/wiki/Block-level_storage and how it is used by cloud providers, and how it differs from Object Storage https://en.wikipedia.org/wiki/Object_storage.
* On the example of AWS services https://dzone.com/articles/confused-by-aws-storage-options-s3-ebs-amp-efs-explained, understand the difference between Block storage, Object storage and Network file system. 

## STEP 1: Creating a Business Website Using NFS For The Backend File Storage.
# PREPARE NFS SERVER
* Spin up AWS redhat instance
* Create security protocol and point it to "TCP 2049"
* Configure and attach 3 logical volumes (LVM) on the server: "lv-opt", "lv-apps", "lv-logs".
* Format the disk as "xfs"
* Create mount points on: "/mnt"
* Mount 'lv-opt' on "/mnt/opt"
* Mount 'lv-apps' on "/mnt/apps"
* Mount 'lv-logs' on "/mnt/logs"
* 

 * Three volumes created and attached to an NFS  server
 <img width="735" alt="three volumes attach 2" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/666f35fb-5c71-4e2b-8154-f9001c74be38">

* Running the command "lsblk" to inspect the names of  three blocks devices attached to the server :"xvdf", "xvdg", "xvdh".
<img width="530" alt="lsblk 3" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/d85bc123-8aac-47b0-bada-e0762669b0cb">
  
* Running the "ls/dev/ to view all three created blocks devices
 <img width="815" alt="ls dev 4" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/f55f4bef-8eba-49e8-922c-7b72f790ec82">

* With the "df-h" command, I can view all mounts and available space on my server
<img width="445" alt="view available space in dick 5" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/7b955231-7cb6-4f0b-bb79-74bd0058e896">

* To partition the three disks individually, I run the command 'sudo fdisk /dev/xvdf
 <img width="624" alt="partition two updated and correct" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/c73618fa-6a83-40df-b9ab-0ef3fcc33ef9">

* On the second disk,  I run the command 'sudo fdisk /dev/xvdg
<img width="583" alt="partition one updated and correct" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/743f3bd7-7d32-4d1b-82a0-15f1e49783e1">

* And on the three disk,  I run the command 'sudo fdisk /dev/xvdh
<img width="659" alt="partition three updated and correct" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/f7b4238b-8f0c-4826-aa04-311c5c568fd7">

* Executed the command "lsblk" to view a three partioned disks
<img width="522" alt="partition lsblk updated 4" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/01e6fd38-ea33-4b5d-a033-3d249539564b">

* Installing lvm2 packaging
<img width="852" alt="sudo yum install lvm2" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/83eb3122-1987-4f22-8770-22d20954ddd8">

* Checking for available partition
<img width="445" alt="sudo lvmdiskscan" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/eb05f419-8a71-45b0-a3aa-03fb9a6b247e">

* Employing the "pvcreate" tool to designate each of the three disks as physical volumes for utilization by LVM
<img width="488" alt="pvcreate" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/33f822ef-982e-43ea-891f-8c85f3d1e8ef">

* PV has been created
 <img width="378" alt="sudo pvs" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/b04cd710-681e-4b1c-9ba3-38b9840d8ead">

* Use "pvcreate" to add all three PV to one  volumes group called "webdata-vg"
<img width="544" alt="df -h sudo mount" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/2fa10a0a-b72f-4281-90c0-b4653df943d1">

* "VG successfully created
<img width="579" alt="webdata-vg created" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/8d03247d-99f0-4fce-9a76-097155f65c3b">

 
* Three logical volumes created: lv-apps stores website data. lv-logs stores log data, lv-opt stores ..... data.
<img width="493" alt="sudo lvcreate" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/c9ade003-6bd5-409e-b514-de3606ba8bcf">

* Logical volumes created and running.
<img width="600" alt="sudo lvs" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/70174253-5a51-4084-8ea6-96e1a78823e1">

* Verifying the entire set up with the command "sudo vgdisplay -v #view complete setup - VG, PV, and LV"
<img width="743" alt="verify entire set up 1" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/c180afc5-bb70-4931-8aa7-5f61b36fee21">

<img width="772" alt="verify entire set up 2" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/59a10e02-128c-498d-b8a2-8d095c1f1b2e">

<img width="868" alt="verify entire set up 3 n4" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/3c808ee8-5a71-471f-aef3-0b9e74631b32">

<img width="675" alt="verify entire set up 5" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/1f83b924-f4f9-4017-a980-e59075484d59">

* "sudo lsbik"
<img width="578" alt="lsbik comes after mounting LV directories" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/f7cae3a0-b0ca-488a-863c-70755d63af96">

* Format logical volumes with "ext4" filesystem
 <img width="637" alt="sudo mkfs -t xfs devwebdata-vglv-apps" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/33a51f25-af82-4157-87b1-0ea2e660b270">

* Create mount point "/mnt"
<img width="486" alt="create mounts point" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/26fae9a5-72e4-48df-a81a-15b664d51226">

* Create directory to store web files and recovery log to back up log data
* Can also use "sudo mkdir -p /var/www/html
<img width="420" alt="create mnt directory to store to store website file and create recovry files" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/e034fa96-7741-4b0a-9917-216af30e0d9b">

* Mount on all three LV on "/mnt"
*  and use the rsyn utility tool to backup all files in the log directory "/var/log" into the "/home/recovery/log"
*  Run "sudo rsync -av /home/recovery/logs/. /var/log" to restore log files back into "/var/log directory
<img width="666" alt="This process comes immediately before mounting" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/46bcb384-57fd-4471-ad21-4508982d9854">

* "lsblk"
<img width="666" alt="This process comes immediately before mounting" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/bce52bee-3a97-4b41-a2b2-324a123ee51c">

* "sudo blkid"
<img width="806" alt="sudo blkid," src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/2fb0f9cb-cc64-4827-8133-03c6110b08cf">

* Update "/etc/fstab" using my UUID
* "sudo vi /etc/fstab"
<img width="681" alt="sudo vi etc fstab" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/d0c6ede9-0a36-4bca-9cb3-ae09380af1c6">

* Test the config and reload deamon
<img width="513" alt="sudo mount to test config and reload deamon 2" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/8779efe7-b7f9-42b4-86e3-d2deb828a355">

* Setup running
<img width="572" alt="verify set running df -h" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/f0bbd966-b821-40b9-867c-3ba8857552a0">


## STEP 2: INSTALLING AND CONFIGURE NFS Server TO START ON REBOOTH AND ENABLED
* Run the following script:
* sudo yum -y update
* sudo yum install nfs-utils -y
* sudo systemctl start nfs-server.service
* sudo systemctl enable nfs-server.service
* sudo systemctl status nfs-server.service
<img width="761" alt="step 4 nfs server installation" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/969ee11e-9255-4615-bc9c-f783440747ad">

* Export the mount for webservers "subnet cidr" to conect as clients.
* Install all three webserevrs inside the same subnet
* In my NFS server, I locate "Networking" and open a sub link as seen in the two images below
* ## NOTE: But in production setup, I will seperate each tier inside each own subnet cidr for high level of security.
<img width="912" alt="step 4 nfs 2" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/9e80374a-3a0a-4423-a238-ae9f2d15f373">

<img width="907" alt="step 4 nfs 3" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/00aac8a6-8ffe-4c56-9a0b-f4ab91e4dac4">

* Set up permmision to allow to allow all three webservers to write, read and execute files on NFS
<img width="614" alt="step 4 nfs 4" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/d584e9e2-b223-4718-aa54-5d66218cee87">

* Configure access to NFS for clients within same subnet(example of Subnet CIDR -172.31.32.0/20
* First,  "sudo vi /etc/exports" the following 
* /mnt/apps 172.31.32.0/20(rw,sync,no_all_squash,no_root_squash)
* /mnt/logs 172.31.32.0/20(rw,sync,no_all_squash,no_root_squash)
* /mnt/opt 172.31.32.0/20(rw,sync,no_all_squash,no_root_squash)
<img width="621" alt="step 4 nfs 5 sudo vi etc exports" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/183f6ba2-4082-4621-819a-f7fdc6eb2fc4">

* sudo exportfs -arv
<img width="396" alt="step 4 nfs 6" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/e07d5980-5450-4117-97ea-de1ed1ce55e1">

*  Check the NFS port , open and add inbound rules
*  Run this command "rpcinfo -p | grep nfs"
<img width="384" alt="step 4 7 check which nfs ports openning" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/97511502-0932-45eb-99ed-da49a757a285">

* In order for NFS to be accessible from my client, I must also open the three webservers sercurity group and create the three inbound rules:"TCP 111", "UDP 111" and "UDP 2049"
<img width="709" alt="step 4 nfs 8 theree security port openned" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/435f5742-c375-4680-9618-625ebffeb121">

## STEP THREE: CONFIGURE BACKEND MYSQL-DATABASE AS PART OF THREE TIER ARCHITECTURE   
* Install Mysql
* Create a database and name it "tooling"
* Create a database user and name it "webaccess"
* Create permission to "webaccess" user on "tooling" database to do anything only from the webservers "subnet cidr"
* Mysqld installation script on redhat:
*  sudo yum install mysql-server -y
* sudo mysql
* If it refused to connect due to socket error them do this:
* sudo service mysqld start( This will display redirecting to /bin/systemctl start mysqld.service)
* sudo systemctl status mysqld
<img width="731" alt="mysql 1" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/5cc43dc5-e61f-435c-9635-4993c3346a50">

* Database "tooling" created
<img width="641" alt="mysql 2 tooling" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/785dd097-f297-4eb9-b0e9-3163beca1654">
<img width="541" alt="mysql 4 database" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/ffedcfc2-917f-49b3-a4aa-8a59c01c477b">

* Database security group created and set at:Mysql/Aurora, Port range:3306, CICR Block:NFS IP address  
<img width="919" alt="mysql 3 security tcp" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/47800ee4-7f2d-4969-9e4c-1fe9aa67327b">


## STEP 3: PREPARING THE WEBSERVERS
* Next, I will be configuing NFS client on all three web servers, this is compulsory
* Deploying a "tooling" application to our webservers into a shared "NFS" folder
* Config all three web servers to work with one database 
* ## NOTE: Ensure all three webservers can server the same content from the shared storage solution, in this case: Mysql database and NFS server
* ## The databse can be accessed by multiple clients for "read" and "write"
* ## To save files that webservers will use, I will utilize "NFS" and mount previously created logical volumes"lv-apps"to the folder where Apache stores files to be serve to the user "/var/www" .
* ## This approach will make the webservers stateless, this means I will be able to add new ones or remove them whenever needed, and the integrity of the data in the both the database and NFS will be preserve.

1,  Launch a redhat EC2 instance
2, Install NFS client with the command:
* "sudo yum install nfs-utils nfs4-acl-tools -y"
<img width="859" alt="client server 1 installation" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/265ff166-8a10-4034-88da-ad064df2ba2b">


3, To Mount "/var/www" and target NFS export for "Apps", I execute the command:
* sudo mkdir /var/www
* sudo mount -t nfs -o rw,nosuid <NFS-Server-Private-IP-Address>:/mnt/apps /var/www
4, Verify NFS is mounted correctly running the command "dh-f"
* Ensure changes persist after rebooting
<img width="544" alt="df -h sudo mount" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/d9580ecb-639f-4f30-ae4b-85113125a4e9">


* sudo vi /etc/fstab
<img width="810" alt="step 4 8 preparing the webserver, just before forking darey repo" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/19f75b29-000c-4eb6-a1b5-4481d94c01b8">
 


* I run the command "sudo systemctl status httpd" to check mysql status and it is dead/inactive
* I run ti the following command to start it "sudo systemctl start status"
* And "sudo systemctl status httpd" to check status, this time is it active and running
<img width="675" alt="step 4 18 php and dependencies installed, active and running" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/065ffaec-d502-4792-b847-147580ec675f">

* Webserver is active on browser
<img width="869" alt="step 4 12 webserver active" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/d54daac0-69d3-4e30-905e-c1747f7682a5">

## 5, STEP 4:INSTALL REMI'S REPO , APACHE AND PHP
* sudo yum install httpd -y
* sudo dnf install https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm
* sudo dnf install dnf-utils http://rpms.remirepo.net/enterprise/remi-release-9.rpm
* sudo dnf module reset php
* sudo dnf module enable php:remi-7.4
* sudo dnf install php php-opcache php-gd php-curl php-mysqlnd
* sudo systemctl start php-fpm
* sudo systemctl enable php-fpm
* setsebool -P httpd_execmem 1

## 6, Repeat same steps for other two server
* Verify Apach files and directories are available in the webserver in /var/www
* And also on the NFS server in /mnt/apps
* If I see same file, it means NFS is properly mounted
* create a touch test.txt from one webserver and check that the same file is accessible from another webserver

7,  Create log folder for apache on the webserver and mount it to NFS server export's for log. Verify it was mounted succefuly with the command "df -h", reboot to ensure it is properly mounted.

8,  Fork tooling code source from"Darey.io guthub account" to my github account
* First install git
* use the "git clone" command to clone the github "http repo"
* "ls"  command  "tooling"
<img width="305" alt="step 4 9 tooling" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/7721b781-a267-4af6-b10a-4f33a5b0736c">


9, Deploy the tooling website code to the webserver and ensure "html" from the reposotory is is deployed to /var/www/html
* "cd tooling"
* sudo cp -R html/. /var/www/html
* If I created the "text.txt" file earlier, it will display here along the html
<img width="633" alt="step 4 10 cp html file to var www html" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/0b4d53f2-0836-458a-bcad-8187e6064025">

* Open security group port 80
  
*Next,  disables sentenforce
* "sudo sentenforce 0"
* sudo vi /etc/sysconfig/selinux
* Set selinux Disabled
<img width="930" alt="new1" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/b580ec94-ebdf-4ef6-93e1-7d1c0486da0e">


* Make sure mysql-y is installed
* cd "tooling"
* "mysql-h < private db server ip -u webaccess -p tooling < tooling -db.sql" ( injecting the tooling-db-sql schema to the database server that I provided the details for (IP address, password and the user)
* Open the security port to: "MYSQL/AURORA" 
 <img width="742" alt="new6 security port" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/0720eb71-f69e-47a7-b33d-ac7f7de8f491">
<img width="871" alt="new7 mysql ip adress webaccess" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/8a8ce8a6-625e-42fa-b290-8a7793126973">

* "cd tooling"
* "mysql -h 172.31.25.99 -u webaccess -p tooling < tooling-db.sql"
* Enter "mypassword"
<img width="663" alt="new8 tooling mypassword" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/26f7dcf8-2963-411b-a0a3-3e319b3518ca">

* sudo mysql
* Follow the image
 <img width="797" alt="new9 show database" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/83de3fc1-0e64-4782-ae1c-ec0921c49ab3">



* Quickly go the database, and do some config to make sure of the connection
* sudo vi /etc/my.cnf
* Change the binding address  to "0.0.0.0"
* Restart the system, "sudo systemctl restart mysqld"
 <img width="491" alt="step 4 15 redhat mysql conf file" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/c85d4fb8-e0f6-4ef3-a439-33d1debb2cf1">


 9,  Back on the webserver 1, 
* Update the website configuration to to connect to the database in /var/www/html/functions.php
* sudo vi /var/www/html/functions.php

<img width="906" alt="new5 database php" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/c5ca0a20-e72f-43e9-8ca2-59be7ba9b957">

10, Website openned in broswer
<img width="941" alt="new10 admin admin as both user name and passwird in the last page" src="https://github.com/Gailpositive/DevOps-Projects-1-10/assets/111061512/fbbd0d5e-a645-4f04-9aa2-ca0217ce73a0">

# "THE END'
