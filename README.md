# Aerospike on Mac

## Install Vagrant and Configure the Virtual Machine

"Aerospike is supported on OS X in a virtual machine managed by Vagrant." So we have to install Vagrant before using Aerospike in Mac.

Step 1: Install Vagrant
 
Download URL: https://www.vagrantup.com/downloads.html

Step 2: Install VirtualBox
 
Download URL: https://www.virtualbox.org/wiki/Downloads

Step 3: Setup Project using Vagrant

The first step in configuring any Vagrant project is to create a Vagrantfile. The purpose of the Vagrantfile is twofold:
Mark the root directory of your project. Many of the configuration options in Vagrant are relative to this root directory.
Describe the kind of machine and resources you need to run your project, as well as what software to install and how you want to access it.

Vagrant has a built-in command for initializing a directory for usage with Vagrant: vagrant init. For the purpose of this getting started guide, please follow along in your terminal:

  $ mkdir vagrant_getting_started

  $ cd vagrant_getting_started

  $ vagrant init

After that, install a box and add to this directory:

  $ vagrant box add ubuntu/trusty64

This will download the box named "ubuntu/trusty64" from HashiCorp's Atlas box catalog, a place where you can find and host boxes. 
"ubuntu/trusty64" is Official Ubuntu Server 14.04 LTS (Trusty Tahr).

How to use a box:

Now that the box has been added to Vagrant, we need to configure our project to use it as a base. Open the Vagrantfile and change the contents to the following:

Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/trusty64"
  
end

Upp and SSH:

Boost the Vagrant enviroment: 

  $ vagrant up

Then you can ssh to this machine:

  $ vagrant ssh

A more simpler way to achieve the above is to:

  $ vagrant init ubuntu/trusty64

  $ vagrant up --provider virtualbox

  $ vagrant ssh

If you have sshed to this machine and want to exit the machine:

  $ exit

If you want to detroy the machine:

  $ vagrant destroy

## Download Aerospike Server within the Virtual Machine

First we need to ssh to the VM:

  $ vagrant ssh

Since we installed ubuntu/trusty64 as our virtual machine system, so we select Ubuntu 14.04 and download.

  $ wget -O aerospike.tgz 'http://aerospike.com/download/server/latest/artifact/ubuntu14'

  $ tar -xvf aerospike.tgz

  $ cd aerospike-server-community-ubuntu14

  $ sudo ./asinstall # will install the .deb packages

## Run
* Step 1: Stop Aerospike

To halt the virtual machine and stop asd, run:

  $ vagrant halt

* Step 2: Restart Aerospike and AMC

To bring the VM back up and restart Aerospike, run:

$ vagrant up

$ vagrant ssh

$ sudo service aerospike start

