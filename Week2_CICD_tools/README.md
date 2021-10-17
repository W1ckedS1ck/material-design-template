# SECOND WEEKLY ASSASEMENT PERFORMED BY VITALI KUTS



## **1.Create Jenkins VM with internet access - 1**

###	•	AWS – t2.micro should be enough 

go to aws.com, create a new account and sign in to the system



###	•	remember to limit Security group  

it can be done by 6 step in VM configuration 



###	•	Ubuntu 20.04  

it can be installed by choosing proper checkbox in VM configuration 



###	•	install openjdk-8-jdk, Git 

```bash

sudo apt-get update

sudo apt-get upgrade

sudo apt-get install openjdk-8-jre

sudo apt install git

wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -

sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > \

    /etc/apt/sources.list.d/jenkins.list'

sudo apt-get update

sudo apt-get install jenkins



Java -version

systemctl status jenkins.service

```



###	•	install Jenkins with enabling autostart on startup 

```bash

systemctl list-units -t service | grep jenkins # - it starts automatically

```



###	•	setup custom port 8081 for Jenkins 

sudo nano /etc/default/jenkins and do this way  

#port for HTTP connector (default 8080; disable with -1)  

#HTTP_PORT=8080  

HTTP_PORT=8081  

It also can be done after signing up into Jenkins startpage



###	•	plugins – select plugins, add GitHub and Role-based authorization strategy 

go to Manage Jenkins - Manage Plugins - Available and install these ones



###	•	add new user – jenkins-NAME (your fullname, jenkins-linustorvalds) 

go to Manage Jenkins - Manage Users - Create User



---



## **2.Create Agent VM - 1**

Press right click on the existing VM in the AWS EC2 console  - Images and templates - Launch more like this  

### •	openjdk-8-jre, Git 

Connect to second instatce and run

```bash

sudo apt-get update

sudo apt-get upgrade

sudo apt-get install openjdk-8-jre

sudo apt install git

```



###	•	prepare SSH keys 

It has been already provided by AWS



###	•	connect agent to master node 

go to Manage Jenkins - Manage Plugins and make sure that SSH server plugin is installed. It is if you did Jenkins installation with suggested plugins. 

If it’s not - press on Available tabs and do this now.

go to Manage Jenkins - Manage Nodes and Clouds - Permanent Agent and configure it as it should. (ip adress can be found in AWS console). ADD MORE TAGS!! They will be good in furure, believe me lol

---



## **3.Configure tools – NodeJS - 1**

###	•	Manage Jenkins -> Global tool configuration - Add NodeJS installations with version of NodeJS and global npm packages to install (uglify-js, clean-css-cli) 

go to Manage Jenkins - Manage Plugins - Available and install NodeJS plugin  

go to Manage Jenkins - Global Tool Configuration - NodeJS 

name it like "uglify-js & clean-css-cli" and in the section Global npm packages to install write down this: "clean-css-cli uglify-js"

---



## **4.Create “Multibranch Pipeline” pipeline job (work inside Lab folder) - 3**



###	•	folder name – your name in camel case (LinusTorvalds) 

```bash

mkdir -p /home/usr/material-design-template/KutsVitali

```

###	•	Git: fork https://github.com/joashp/material-design-template repo 

It has been done already in WA1

### Write Jenkinsfile which describes declarative pipeline 
It can be found here: https://github.com/W1ckedS1ck/material-design-template/blob/master/Jenkinsfile

Archive file you can find here:
ubuntu@ip-172-31-11-225:~/workspace/Pipe1ine/arc.tar.gz (slave device) 
http://3.249.115.12:8081/job/Pipe1ine/22/artifact/ (Jenkins WebInterface)

---

## **5.Setup the GitHub webhook to trigger the jobs - 2**
go here https://github.com/W1ckedS1ck/material-design-template/settings/hooks and add webhook http://3.249.115.12:8081/github-webhook/ Just the push event.
then go to the settings of your project
settings should be like:
GitHub project - https://github.com/W1ckedS1ck/material-design-template.git 
checkbox it - GitHub hook trigger for GITScm polling

The result of pushing will be 
```bash
 Build #1 (17-Oct-2021 17:19:45)
 Started by GitHub push by W1ckedS1ck
 Revision: 43dea8a65b53e029245dc2b43f33e15eef0ee078
 Repository: https://github.com/W1ckedS1ck/material-design-template.git/
 origin/master
 ```
 one more time
 ```bash
 Build #3 (17-Oct-2021 17:23:45)
	Changes
Update README.md (commit: 3578ed6) (details / githubweb)
	
Started by GitHub push by W1ckedS1ck

	Revision: 3578ed65d1eff86c40880cab9b12e2478eaed1a1
Repository: https://github.com/W1ckedS1ck/material-design-template.git/
origin/master
```
