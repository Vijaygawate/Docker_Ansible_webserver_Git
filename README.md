https://www.youtube.com/watch?v=gvk9jZz0P6s&list=PLLu1bCv5AByECPeW_h9n_G9fikVOjz45u&index=2
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

------Creation of jenkins job----
Install "publish over ssh" plugin 
New item 
name- webapp_project3(freestyle)
git repo--- https://github.com/Vijaygawate/Docker_Ansible_webserver_Git.git
manage jenkins > configure system > add ssh 
1.Name - jenkins 
pw- Vijay22@
2.Name - Ansible 
3.Name - Web_Server 
and then check the connections if success or not 
Apply and save 

Now, configure same job i.e now whatever files we pull from github on jenkins server need to send on ansible server 
Build--Send files or execute commands over SSH
Name- jenkins 
Exec command - #rsync -avh /var/lib/jenkins/workspace/Docker_Ansible_webserver_Git/* root<private ip of ansible server>:/opt

Apply and save 
BUILD THE JOB

Now, Configure same job to building docker image from dockerfile and pushing it to dockerhub
Build--Send files or execute commands over SSH
Name- Ansible 
Exec command - #cd /opt
               #docker image build -t $JOB_NAME:v1.$BUILD_ID .
               #docker image tag $JOB_NAME:v1.$BUILD_ID vijaygawate/$JOB_NAME:v1.$BUILD_ID
               #docker image tag $JOB_NAME:v1.$BUILD_ID vijaygawate/$JOB_NAME:latest 
               #docker image push vijaygawate/$JOB_NAME:v1.$BUILD_ID
               #docker image push vijaygawate/$JOB_NAME:latest
               #docker image rmi $JOB_NAME:v1.$BUILD_ID vijaygawate/$JOB_NAME:v1.$BUILD_ID vijaygawate/$JOB_NAME:latest
