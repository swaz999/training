# Day 1 Notes

## Github Repo: 

* created a repo named caterpillar and learnt how to create a repository, Create a branch, Pull request and merge to master - I have seen a document below :
* https://guides.github.com/activities/hello-world/
```
  54 git status
   55  git add .
   56  git status
   57  git commit
   58  git commit -m "Added day1 task list"
   59  git config --global user.email "you@example.com"
   60  git config --global user.email "swaz@something.com"
   61   git config --global user.name "Swaz999"
   62  git commit -m "Added day1 task list"
   63  git push origin master
   64  mkdir day1
   65  cd day1
   66  touch summary.md
   67  open summary.md
 ```


##Vagrant :

* Created a directory *vagrantcaterpillar* in home directory
* vagrant is a tool used to build VM's on top of Virtualbox. So, Virtual box installation is a prior step. 
* vagrant init command creates a Vagrantfile that has default settings. 
* Config is the obejct that holds the configurations and thre will be a variable called config.vm.box that defines the flavor of the box(Centos/7, etc.,)
* vagrant up command creates a VM with default memory and cpu. 
* If we change any configurations at any point of time, we need to use the command vagrant reload.
**Note** 
To set an alias for a command need to provide the alias in ~/.bash_profile that loads in to each session of the shell.  
In ~/.vimrc we can write settings for vim (For example : syntx on) that lets the feature enabled in each shell session. 

Here is the code:

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "centos/7"

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  # config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
  #   # Customize the amount of memory on the VM:
  #   vb.memory = "1024"
  # end
  #
  # View the documentation for the provider you are using for more
  # information on available options.

  # Enable provisioning with a shell script. Additional provisioners such as
  # Puppet, Chef, Ansible, Salt, and Docker are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision "shell", inline: <<-SHELL
  #   apt-get update
  #   apt-get install -y apache2
  # SHELL
end
```
##Create Multi Vagrant Machines:

* To create multi VM's with in the same script - we need to create config of new machines in the existing cionfiguration of a VM.
* We leverage the config object that pre loads and creates our own object also uses the other features mentioned for VM's. 
* We need to create IP addresses for each VM and will be on same network with private IP addresses. 
* To set hostname we can use cofig.vm.hostname (here cocoon is the object name).
* Use vagrant up to get all the machines running. Or use vagrant up cocoon or the oether VM. 
* use vagrant halt to suspend the VM
**Notes** Need to add details about the hostnames and IP address in /etc/hosts file in the macbook account and on the machine  cocoon aswell as below:

```
##
# Host Database
#
# localhost is used to configure the loopback interface
# when the system is booting.  Do not change this entry.
##
127.0.0.1       localhost
255.255.255.255 broadcasthost
::1             localhost

192.168.33.10   cocoon.multibox.com
192.168.33.11   bamboo.multibox.com
192.168.33.12   thread.multibox.com
```


Here is the code: 

```
# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = "bento/centos-7.6"


  config.vm.define "cocoon", primary: true do |cocoon|
    cocoon.vm.network "private_network", ip: "192.168.33.10"
      cocoon.vm.hostname = "cocoon.mutlibox.com"
  end

  config.vm.define "bamboo" do |bamboo|
    bamboo.vm.network "private_network", ip: "192.168.33.11"
      bamboo.vm.hostname = "bamboo.multibox.com"
  end

  config.vm.define "thread" do |thread|
    thread.vm.network "private_network", ip: "192.168.33.12"
      thread.vm.hostname = "thread.multibox.com"
  end

```



* Created a password less ssh from Mac(sushmareddy) to vagrant account in cocoon, bamboo and thread. 
* Use command ssh-keygen to generate a .ssh file that has authorized_keys, id_rsa.pub and id_rsa. 
* sometimes authorized_keys will not be generated at that point of time we create that folder and make sure that the permissions are 700 on authorized_keys folder. 
* Create .ssh file on Macbook and copy id_rsa.pub to authorized_keys folder in cocoon, bamboo and thread's vagrant account. 
* Created a password less ssh from macbook account to root account on all three machines. (id_rsa.pub from macbook account to authorized_keys of root accounts on all VM's).

**Note** Vagrant account has permissions in /etc/sudoers.d folder. Also, vagrant account has direct access to root in a VM when created with vagrant tool.

#Ansible:

* Ansible is the script that lets you deploy on multiple machines at the same time. 
* Install Ansible on Macbook using brew install ansible. We need to install Xcode prior to that installation.
* In macbook we might need to create /etc/ansible folder manually as brew installs it some place else. In CentOs we can find the folder in the same place. 
* Need to set host names under /etc/ansible/hosts
* In the file we can have groups creates for testing and all the servers under the same group as below

```
[test]
cocoon.multibox.com

[mboxes]
cocoon.multibox.com
bamboo.multibox.com
thread.multibox.com

```

* Ad-hoc commands can be used to run the commands on remote machines at the same time. 

Here are the commands : 

```
ansible all -m yum -a "name=vim"
ansible all -m yum -a "name=vim state=absent"
ansible all -m yum -a "name=vim state=present"
```

* In the above ad-hoc commands all(is the group name) -m(module such as yum if it's a installtion etc., see the ansible documentation for further details) -a (pass commands to module) state ( present to install and absent to uninstall).
* We can write scripts to have all the configurations at the same place. 
 




