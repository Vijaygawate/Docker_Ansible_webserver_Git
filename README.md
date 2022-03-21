Description: Code i.e ansible playbook and docker file will be in github repo , once commit happens jenkins will build the dockerfile and create docker image and push it to the docker hub then webserver will pull the image from dockerhub and create conatiner on webserver 
Prereuisites::::::: Jenkins, ansible, webserver , gitrepo 
Docker should be installed on both ansible and webserver 

--------Establishing pw less connection between ansible server and web server----------
Do below steps on both webserver and ansible server 
#vi /etc/ssh/sshd_config
uncomment permmitrootlogin
and below no to yes
#systemctl restart sshd
@@@@On ansible server set pw for root :::::
#passwd root 
#Vijay22@  
@@@@@On web server set pw for root:::::::
#passwd root 
#Vijay22@
#ssh-keygen
#ssh-copy-id -i root@<private ip of webserver>

----------Establishing pw less connection between Jenkins server to ansible server and web server----------
On jenkins server 
#passwd root 
#Vijay22@
#vi /etc/ssh/sshd_config
uncomment permmitrootlogin
and below no to yes
#systemctl restart sshd
#ssh-keygen
#ssh-copy-id -i root@<private ip of ansible server>
#ssh-copy-id -i root@<private ip of web server>

