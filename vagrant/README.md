# Multi Machine in Vagrant

Vagrant is tools to automate development and testing environment. It helps to setup virtual machines from declarative configuration file. Sometime I go I wrote a very quick introduction about vagrant. This post will go further by explaining how to setup multiple VMs.

Why we need to bother setting up multiple VMs? In today software development world, most system is deployed as part of distributed systems. The simplest form is two tier monolith service and datastore. However, nowadays people have been breaking down the monolith into multiple microservices. Put also into the equation that some integration point is being setup asynchrously using various kind of messaging or log system. Also since data is so ubiquotous, we have big data processing infrastructures either using batch or streaming. I always excited of how the things work in that distrubuted systems setup. We can learn and investigate the characteristic of those distributed systems in our local computer. It is cheaper than if spawn the same instance in the cloud provider.

We will do step by step process of having the multi machine setup. The complete Vagrantfile configuration can be downloaded in the github.

## Step 0: Setup

We only need two applications in setting up multiple VMs in local guest operating system:
* VM provider such as VirtualBox
* Vagrant to automate the provisioning of the VMs

## Step 1: Create Vagrantfile

We can use `vagrant init` command that will generate default file with many configuration example. Or we can just create a new empty `Vagrantfile`. Let's add this first content:

```
Vagrant.configure("2") do |config|
  # setup and provisioning will be added here
end
```

Vagrantfile is written in Ruby. Underestanding Ruby will help but not mandatory, unless we want to make complex customisation or provisioning. Vagrantfile will start with the configure method. `"2"` in the configure argument is the vagrant configuration version. I don't see any reason why we want to use version other than 2. The configuration will pass the `config` object in the lambda argument.

## Step 2: Configure provider setting

```
config.vm.provider "virtualbox" do |v|
  v.memory = 256
end
```

We might want to limit the VMs hardware resource just to make sure it doesn't overwhelme the host operating system. In the configuration above, we limit the VM RAM to 256MB.

## Step 3: Create a single machine
```
config.vm define "box_1 do |box|
  box.vm.box = "ubuntu/bionic64"
  box.vm.network "private_network", ip: "192.168.51.1"
  box.vm.provision "shell", inline: "echo 'hello world'"
end
```

We are creating one VM box with that given IP and box type passed to `box.vm.box` variable. We are setting the network to private and pass the static ip address in the ip argument. In the provision part we are passing a simple shell inline script that just echoing "hello world".

## Step 4: Create multiple machines

```
box_name = "ubuntu/bionic64"

base_ip = 100
base_ip_addresses = "192.168.51"
# we will create ip address from 192.168.51.100 onward
ip_addresses = (1..number_of_machines).map{ |i| "#{base_ip_addresses}.#{base_ip + i}" }

# provisioning script
# Add all required setup here
script = <<-SCRIPT
  echo "hello world"
SCRIPT

config.vm.provider "virtualbox" do |v|
  v.memory = 256
end

(1..number_of_machines).each do |i|
  config.vm.define "box_#{i}" do |box|
    box.vm.box = box_name
    box.vm.network "private_network", ip: "#{ip_addresses[i-1]}"

    # provion the script
    box.vm.provision "shell", inline: "#{script}"
  end
end

```

The configuration of multiple machine is just the extension from the single machine setup. Fundamentally we are just looping the configuration to the number of machine that we want to create. 

In the Vagrantfile fragment above, the only dynamic configuration for each machine is we want to make sure no machine has the same IP address with others. 