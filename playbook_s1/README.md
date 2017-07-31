This file explains all the components of the task to be submitted to Pythian. The task is as explained from the README file on the project/task.
An introduction:
To me ansible is the simplest automation tool out there for implementing DevOps projects. Yet it has the ability to implement almost all tasks the others out there such as SALT, Puppet etc, can do. 
Assuming you have installed successfully ansible, virtual box and vagrant on your local computer please refer to the following link to achive all these http://docs.ansible.com/ansible/latest/intro_installation.html
The Vagrantfile:
The Vagrant file of this task simple:
   The first line selects the Vagrant machine to be Ubuntu/trust64
   The next maps port 8080 and 8443 respectively of your local computer in my case Macbook to port 80 and 443 of the Vagrant machine. 
   The next line configures this Vagrant machine with a local network of IP "192.168.33.10
   
  If you are in the playbook directory on your terminal you can use the following:
  $ vagrant up   # this launches the vagrant machine and 
  $ vagrant reload # should in case you modify the vagrant file and want to relaunch with your modification
  $ vagrant provision # This is used to commision the project 
 
The Ansible.cfg
   To reduce configution setup and coding in the inventory file ansible has a number of ways to achieve this. I prefer the ansible.cfg file. 
   this file can be placed in so many places :i)   ./ansible.cfg #this is putting the config file in the current directory which is my preference, because you can easily make correction to it and go on with your work!
   					      ii)  ~/.ansible.cfg # This is placing it in your home directory
				              iii) /etc/ansible/ansible.cfg

   In the cfg file I stoped the pop uo warning, defined what name my hostfile would be and stored the private key file in the VM for ease in ssh 		 


The Hosts
  The Host file is simple I named the server pythian_vm and wrote its IP address which it can be reached at
  to test its reachability simply type:
  $ ansible pythian_vm -m ping
  you could also determine how long the server has been up this is sometime useful for network admins
  $ ansible pythian_vm -a uptime

The Playbook
  This is the main config file for project I called it pythian.yml. It is written in YAML.
  The playbook is just according to task required for submission.

  Tasks: i)   Launch a VM: the vm to be launched, its details are all explained above
         ii)  Ping: this is to test the VM is active/up and can be reached
         iii) Apt: "since the VM is Ubuntu and not CentOS" This is used to install the packages git, apache2, mysql respectively,followed by a check if the packages is latest and then cache is updated(patched).  After each installation is completed, the admin is notified with confirmation OK from the server.
         iv)  Directory code: This directory /opt/code/ is created on the Vm to store the cloned git repository to be downloaded and given a Chown of 0750 (drwxr-x----0), It is grouped and named in the process pythian, (Unfortunately, I could not succefully change the owner name to pythian, cosit would not allow. I have not figured out why.)
         v)   CLoning repo: I cloned a simple repo from github, Into the new directory code just created.
         vi)  Group naming: Although this is not required. I just created a group pythian      
   
  Run:  Simply type $ ansible-playbook pythian.yml on the terminal and the playbook will be set into action
        NB: if you want to enter vagrant provision, you may have to delete the file pythian4.yml
      
            


references: 
1) http://docs.ansible.com/ansible/latest/intro_installation.html
2) Ansible up and Running look to linux-com (www.it-ebooks.com)  
