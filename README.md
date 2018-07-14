
[link](https://google.com/)


Copy Azure VHD (virtual hard drive) between diffrent storage account
=================
- Copy a single blob from one storage account to another storage account
- Run: AzCopy /Source:https://sourceaccount.blob.core.windows.net/mycontainer1 /Dest:https://destaccount.blob.core.windows.net/mycontainer2 /SourceKey:key1 /DestKey:key2 /Pattern:abc.txt
- SourceKey - should be the storage acount access key (appears in azure portal overview), or secondary key
- DestKey - should be the storage acount access key (appears in azure portal overview), or secondary key
- Pattern: the file name or wildcard pattern of file names

Github - Create user for jenkins
=================
- Create new github user (dedicated for jenkins access)
- Login with the new user
- Account -> Settings -> Developer Settings -> Personal access token 
- Click "Generate new token", descirption: jenkins-github, scopes: select all or only spesific
- Click "Generate token", copy the generate token string
- Login to jenkins server as admin -> Manage jenkins -> Configure System
- Github section -> GitHub Server: Name: "github", API URL: "https://api.github.com"
- Credentials -> Add -> Jenkins -> Kind: "Secret Text", copy the token string from previous step
- the Id should be a name for this token
- Then, select this token (from the combo box) in the github credentials, click save

Mount Data Disk
================
- Check all exists disks by running: sudo fdisk -l
- Run: sudo lsblk -o NAME,FSTYPE,SIZE,MOUNTPOINT,LABEL
- create datadrive, run: sudo mkdir /datadrive
- Run: sudo -i blkid
- To one time hard drive mount Run: sudo mount /dev/sdc1 /datadrive -a (select the disk and partition you want to mount)
- To force mount on every boot, run ```sudo nano /etc/fstab```
- Add the following line to the end of the /etc/fstab file (use the uuid from the command blkid):
UUID=33333333-3b3b-3c3c-3d3d-3e3e3e3e3e3e   /datadrive   ext4   defaults,nofail   1   2
- Reboot the server and ensure datadrive has the right file and directories


Install New Ubuntu Server
===============
- Prefered version is ubuntu 16.0.4 lts
- Make sure to open ports 8080 and 22 to the server
- Once installtion is complete by the cloud vendor (azure/aws) connect to the server via ssh
- run: ssh znkadmin@zinkerzci.westeurope.cloudapp.azure.com (port 22), enter the password
- Install java, run: sudo add-apt-repository ppa:webupd8team/java
- Then run: sudo apt update; sudo apt install oracle-java8-installer
- Check java is installed by running ```javac -version```
- Install nodejs: curl -sL https://deb.nodesource.com/setup_6.x | sudo -E bash -
- Run: sudo apt-get install -y nodejs
- Install git: apt-get install git
- Install typescript: npm install -g typescript 
- Install gulp: npm install -g gulp 
- Install vncserver: apt-get install vnc4server fluxbox
- Install build essentials: sudo apt-get install build-essential
- Start vnc for the first time (generate new password is needed once)
- First, login as jenkins user, run: sudo su -s /bin/bash jenkins
- Then run: vncserver and set empty password
- Install bower: sudo npm install -g bower
- Install chrome, run: sudo nano /etc/apt/sources.list
- Use the down arrow key to scroll to the bottom of this file. 
- Copy the following APT line and paste it at the end of the file:
- deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main
- Run: wget https://dl.google.com/linux/linux_signing_key.pub
- Run: sudo apt-key add linux_signing_key.pub
- Run: sudo apt update
- Run: sudo apt install google-chrome-stable

Changing Jenkins Home directory
============
- On the jenkins server
- Run: nano /etc/default/jenkins
- And changing the $JENKINS_HOME variable (around line 23) to JENKINS_HOME=/newhome/jenkins
- Restart jenkins: /etc/init.d/jenkins start
- Set Jenkins new url in "Manage Jenkins"
- "Manage Jenkins" > "Configure Global Security" and select "Prevent Cross Site Request Forgery exploits." 

Setup Claudia on Jenkins
============
- Run as jenkins user: sudo su -s /bin/bash jenkins
- go to jenkins user home by: cd ~
- mkdir .aws
- cd .aws
- nano credentials
- copy the following lines (add the correct access and secrets keys)
[default]
aws_access_key_id = XXXXXXXXXXXX
aws_secret_access_key = XXXXXXXXXXX
- save the file
- nano config
- copy the following lines
[default]
region = eu-west-1
- save the file






Restart Jenkins Service
============
Select one of the three: 
- Run: /etc/init.d/jenkins start
- Run: sudo service start jenkins
- Run: sudo systemctl restart jenkins

Signin to server as jenkins user
============
- As and admin user run: sudo su -s /bin/bash jenkins

links
=======
- https://stackoverflow.com/questions/25577254/how-do-i-jenkins-permission-on-a-per-job-basis
- http://www.tothenew.com/blog/jenkins-implementing-project-based-matrix-authorization-strategy/
- http://blog.dahanne.net/2011/07/18/run-ui-tests-on-a-headless-jenkins-hudson-continuous-integration-server-running-ubuntu/
- java install http://tipsonubuntu.com/2016/07/31/install-oracle-java-8-9-ubuntu-16-04-linux-mint-18/
- jenkins home dir: http://blog.code4hire.com/2011/09/changing-the-jenkins-home-directory-on-ubuntu-take-2/
- chrome: https://github.com/karma-runner/karma-chrome-launcher/issues/73
- bower: https://tecadmin.net/install-bower-on-ubuntu/
- nodejs: https://nodesource.com/blog/installing-node-js-tutorial-ubuntu/
- jenkins user: https://stackoverflow.com/questions/18068358/cant-su-to-user-jenkins-after-installing-jenkins
- github jenkins: https://www.thegeekstuff.com/2016/10/jenkins-git-setup
- git install: https://www.vultr.com/docs/setting-up-git-on-ubuntu-14-04
- github personal access token: https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/
- network ports security https://docs.microsoft.com/en-us/azure/virtual-machines/windows/nsg-quickstart-portal
- ssh connection to azure https://docs.microsoft.com/en-us/azure/virtual-machines/linux/troubleshoot-ssh-connection
- azure https://docs.microsoft.com/en-us/azure/virtual-machines/linux/troubleshoot-ssh-connection
- ssh-keygen https://www.ssh.com/ssh/keygen/
- ubuntu mount https://askubuntu.com/questions/182446/how-do-i-view-all-available-hdds-partitions
- ubuntu mount https://docs.microsoft.com/en-us/azure/virtual-machines/linux/add-disk
- azure ubuntu add disk https://docs.microsoft.com/en-us/azure/virtual-machines/linux/add-disk
- mount drive https://chrismckee.co.uk/creating-mounting-new-drives-in-ubuntu-azure/
- vhd to vm https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-specialized-vhd
- azure azcopy https://docs.microsoft.com/en-us/azure/storage/common/storage-use-azcopy?toc=%2fazure%2fstorage%2fblobs%2ftoc.json
- chrome ubuntu https://www.linuxbabe.com/ubuntu/install-google-chrome-ubuntu-16-04-lts
- install chrome https://askubuntu.com/questions/79280/how-to-install-chrome-browser-properly-via-command-line
- github ssh https://mohitgoyal.co/2017/02/27/configuring-ssh-authentication-between-github-and-jenkins/
- github https://medium.com/@MaciejNajbar/setup-jenkins-for-private-repository-9060f54eeac9
- claudia credentials https://claudiajs.com/tutorials/installing.html
