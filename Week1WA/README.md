# FIRST WEEKLY ASSASEMENT PERFORMED BY VITALI KUTS  

Lab: simple automatic deployment  

## •      Spin up a VM  - DONE (nuff said)  

## •      Install Nginx web server and git and fork GitHub repo https://github.com/joashp/material-design-template  
##################################################################  
  
cd~  
apt-get update  
apt-get install -y git  
apt-get install -y nginx  
sudo nginx -t  
git --version  
  
                        GIT CONFIGURATION.  
git config --global user.name "Vitali Kuts"  
git config --global user.email "VitaliK@playtika.com"   
git config --global --list  
git clone git@github.com:W1ckedS1ck/material-design-template.git  
  
                        NGINX CONGIGURATION.   
nano /etc/nginx/sites-available/default  
delete root /var/www/html;  
add    /home/usr/material-design-template/www;  
  
nano /etc/nginx/nginx.conf  
delete user www-data;  
add    user usr; ( to prevent "/home/usr/material-design-template/www/" failed (13: Permission denied) )  
systemctl restart nginx  
##################################################################  
  
## •      Setup a cron job for a regular (every 1 minute) checkout from main branch  
##################################################################  
crontab -e  

* * * * * cd /home/usr/material-design-template ; git pull origin master  
* * * * * cd /home/usr/Desktop ; date >> file.log  
  
crontab -l  
##################################################################  
  
## •      Update index.html from your machine, push changes to Git and confirm updated content on web page  
##################################################################  
--This VM has been attached to the github account before  
  
git clone git@github.com:W1ckedS1ck/material-design-template.git  
cd /home/usr/material-design-template/www  
******* #changed index.html  
git add .  
git commit -m " _________ "  

Cronlog can be reviewed here - https://github.com/W1ckedS1ck/material-design-template/blob/master/additions2assasement/cronlog.txt  
##################################################################  

## •      Configure Github hook instead of cron. If VM is not accessible from the Internet, use local git hooks for validation of incoming commits (reject commits if there is a “shit” string in the files)  
##################################################################  
--Because of my VM is beyong NAT and it could't be reached from the Internet I'm going to use local webhook  
  
cd /home/usr/material-design-template/.git/hooks  
cp pre-commit.sample pre-commit  
 > pre-commit  
nano pre-commit  
  
#!/bin/bash  
#git diff --cached > /home/usr/Desktop/hook.log  
git diff --cached > hook.log  
if cat hook.log | grep shit  
then  
echo "Shit-word isn't allowed to commit"  
rm hook.log  
exit 1  
fi  
##################################################################  
  
## •      Configure Nginx to Proxy Websockets and cache static content within 1 hour  
##################################################################  
  
##################################################################  
  
## •      Merge feature branch with main, rebase git merge commit, squash all commits  
##################################################################  
cd /home/usr/material-design-template  
git checkout -b feature  
-- Do some changes and commit(s)  
git checkout -b feature  
mkdir feature  
cd feature  
git add .  
git commit -m "feature is done"  
git checkout master  
git merge checkout  
git log --graph  
git rebase -i --autosquash HEAD~2 # just for show as it should for 2 commits. Not for all of them.  
  
  
photo will be here in a proper way https://github.com/W1ckedS1ck/material-design-template/tree/master/additions2assasement  
