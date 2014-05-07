---
layout: post
title: "Set up OpenStack on a development evironment"
date: 2014-05-07 18:19:34 +0530
comments: true
categories: 
---

To develop something, having a proper environment setup is key. This post explains how you can set up a development environment to contribute to OpenStack. I'm going to use DevStack which is a shell script that builds OpenStack completely in a developer friendly environment.
 
1. Virtual Machine
Firstly, set up a virtual machine running any of the supported distro/release of DevStack. Ubuntu 12.04, Fedora 20, CentOS/RHEL 6.5 are supported.
Any type of virtualisation software should be fine, I use Vagrant because it is easier to manage the instances.
After a VM is up and running, install a Linux distribution. I have installed Ubuntu 14.04 (it is not officially supported, but it works for me).

2. Setup user
At this point, we have a VM running a Linux OS. We need to create a user called 'stack'. For Red Hat and the like, 'stack' should be a non-root user, this does not matter in Ubuntu.  
 
3. Setup the VM
We need to install git and clone the DevStack repository
sudo apt-get update  
sudo apt-get install git  
cd ~  
git clone https://github.com/openstack-dev/devstack.git  

4. Now is the fun part, well almost
Working with OpenStack means you got to remember passwords for admin, database, rabbit etc. Your life is a lot simpler with a bit of configuration. Edit the local.conf file in the repository's root to look like this.
[[local|localrc]]  
ADMIN_PASSWORD=secrete  
DATABASE_PASSWORD=$ADMIN_PASSWORD  
RABBIT_PASSWORD=$ADMIN_PASSWORD  
SERVICE_PASSWORD=$ADMIN_PASSWORD  
SERVICE_TOKEN=some_token  
 
6. Ok, the fun part
     simply run stack.sh and keep your eyes peeled
./stack.sh   

7. Using DevStack
stack.sh takes a long time to execute. After it has completed successfully, it displays information about your newly chromed OpenStack install. You can log in to Horizon, which is the web UI dashboard to manage your VMs.
Be careful when you reboot your VM. If you ever need to reboot, you have to run unstack.sh and then reboot. After the reboot you need to run rejoin-stack.sh to get the services running again.
 
Enjoy your OpenStack and please contribute to the OpenSource community.
