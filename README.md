# Deploying remote Nginx Server which hosts a static HTML website on Amazon Linux (EC2) using Automation (Ansible)
### DESCRIPTION

#### PROJECT IDEA

Browsing through the internet I came across a DevOps project challenge to provision a web server with automation and publish a website onto it.

In this project, we want to set a web server on a remote machine. Then install a web server and configure the environments and deploy a static HTML application but all this with **Automation**.

This project will help you with these job requirements :


- Provision and configure infrastructure through automation
- Required experience in Scripting Tools (eg PowerShell, Batch, etc)
- Understanding of web application development, server deployment and upkeep, and general networking practices.

#### NEXT STEPS

I researched and found out the technologies with which it's possible and started working on the idea & believe me when you start working on a project with 3 objectives you have to complete a lot of other objectives which greatly opens up your concepts. 

###  ENVIRONMENT

**Host & Remote Nodes**: AWS Linux EC2

**Web Server**: Nginx

**Application**: Static HTML (Wishes Happy New Year)

**Automation**:  Ansible

I chose AWS for making remote machines because it spins up instances in seconds with the EC2 service & interacting with the environment becomes super easy.

Nginx because I have earlier worked with this deploying on Docker containers and it's easy to set up.

Ansible because it's been widely used for automation in companies now & also when I searched to automate the above requirements I found more resources for this. 

SSH communications is the key for deploying via Ansible. It is agent-less i.e you don't need to install it on remote machines


### CHALLENGES FACED


- Creating the network & SSH between the 2 machines (Host & Remote Node).

- Time spent to learn Ansible & use it in the project. ( It was fun)

- Facing continuous errors at most of the steps & troubleshooting them ( That's what is the most important & toughest part of the project).

### BUILDING THE PROJECT

#### SETTING UP EC2 , Establishing SSH connection between server & Node


> Note: This is a long process, I will only provide the highlights here which might help you to set up the environment. Alternatively, there are a lot of resources to set up an EC2 environment with ansible.

- Create 2 EC2 Instances. One will be used as an Ansible server and the other as a Remote Server. (Install package manager in user data as sudo user)


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641280250395/o8K77Od5Z.png)

- Use PuTTY which allows the use of SSH (Secure Shell) to access a remote computer. We will be generating a .ppk key using the EC2 instance .pem file used while creating the instances & then connecting with the Public IPs of the Instances. Log in as ec2-user

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641280327595/6y9Ooxqun.png)

- Login into Ansible server as root user & install epel ( additional enterprise-level packages). In AWS EC2 it is installed by default, figure out a way to enable it.

- Download packages necessary for the project


"yum install git python python-level python-pip openssl ansible -y"



#### Communicating Ansible server about which nodes to host.

Make a group under this and put the private IP of the host here.

"vi /etc/anible/hosts" 


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641280483245/zf76cHwrL.png)

Enable the hosts in the config file


"vi /etc/ansible/ansible.confg"

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641280606139/jXIBjVlCI.png)



####  ADDING USERS

Add user Ansible on both Server & Remote nodes and give sudo rights to them. 

You can give sudo rights by being the root user, then run the command visudo and add this configuration

![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641280890615/J17tK0zw0.png)


#### CONNECT THE SERVER WITH HOSTS 

This is done with SSH. The Secure Shell Daemon application (SSH daemon or sshd) is the daemon program for ssh. The sshd is the daemon that listens for connections from clients on port 22.

(ROOT)But first change some sshd configurations in :

" vi /etc/ssh/sshd-config"

**Changes to enable**


- 
PermitRootLogin

- Password Authentication yes

**Changes to disable/comment**

- Password Authentication no

**Restart the service after changes **


> service sshd restart

**Connect with Remote server**
Login as Ansible in both servers


"ssh@IP"
 
 
Boom you will be into your remote node now, Try creating one file there and then see in your remote node those files will be there.

**NOTE:** If you don't want to enter a password every time you ssh, There is a way to do this. Try finding this out & then you can directly enter into the remote machine without ssh password.


***The environment setup is done here, now all you need is to create files that will be used by Ansible to automate your server provisioning & host HTML site on it***

####  HOSTS

The default Ansible hosts file contains groups of hosts. However, the default inventory file is applied globally across the system and often requires admin permissions.

Instead, to make things simpler, we're going to make our own inventory file here in user ansible of Ansible Server.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641282122635/DRQCMlKn8.png)

#### Connection test 

To test the connection , run command:

"ansible -i hosts demo -m ping -u ansible"


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641282258550/Iw9WLpFcu.png)

#### TEMPLATE FILE

Create a template file called index.html.j2. The static HTML application we will deploy.


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641282386980/RVxwWQp_b.png)

I am using variable {{ MyMesage }} which will be mentioned in the playbook.

#### PLAYBOOK.YAML

Here is the playbook which Ansible requires as an argument to automate things. It has Modules & Tasks in which one can describe the requirements.

Here's my playbook



![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641282644966/ksNCqak_0.png)

#### Running the playbook

Once you have completed the playbook run this command

" ansible-playbook -i hosts  -u ansible playbook.yaml "
-i : inventory
-u : user


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641283695893/H-OzHld3p.png)

Try accessing the app on remote server IP or DNS. 


![image.png](https://cdn.hashnode.com/res/hashnode/image/upload/v1641283810500/1CIhRJ-UF.png)

**THANKS!**





