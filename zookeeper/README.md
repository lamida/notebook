# Using Vagrant to Setup Multiple Cluster of Zookeeper

Vagrant is used to automate environment creation. In a shorter description, I like to call it as virtual machine manager. We write a file configuration and then we run a vagrant command line that will setup virtual machine based on the configuration. We should use vagrant only for development and testing purpose but not for production.

With that feature, vagrant is an excellent tool to simulate distributed systems that we can run on top of a single PC. In this blogpost we will simulate the clustering setup of zookeeper using multiple virtual machines.

Requirement:

* A host machine that ideally run linux or mac os operating system
* Vagrant binary that can be downloaded from vagrant website
* VirtualBox as VM provider for Vagrant
* A zookeeper installer that also can be downloaded from zookeeper website

## Out of Scope

This blog post wouldn't go into detail of explaining the installation process because most of the time it is very simple. The post also wouldn't go into detail what is zookeeper is. I will try to share it in another post and assume anyone who come to this page must have already been familiar to what zookeeper is doing. Personally I like to call it as a mother of distributed systems. It is a very popular system to maintain distributed coordination and consensus. To make it reliable, zookeeper is always deployed as clusters that contain at least 3 replicas.

## Setup

On a host machine we need to install vagrant and then virtualbox if they are not already installed. After that we should download a zookeeper installer. At the time I write this post, I am using version zookeeper version 3.5.5, vagrant version 2.2.5 and virtualbox headless version 5.2.18. Hence if you run some different versions that what I have now, some adjustment in command or configuration might be required.

We will simulate deploying multiple zookeeper instances in different servers. The different servers here are represented by multiple virtual machines that run on your laptop.

## Generating Vagrantfile

To initiate managed virtual machines setup, we will ask vagrant to generate `Vagrantfile` which is the main vagrant configuration.

```
vagrant init
```

Once we run it, we can get a default configuration file which is heavily commented with examples.

## Walking through the config file
vagrant up
printing the status
testing leader and follower
kill the leader
## 



WIP
