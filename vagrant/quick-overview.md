# Vagrant Overview
-------------

```bash
# vagrant box management (self-explanatory)
# note that boxes are globally per user (~/.vagrant.d)
vagrant box add puphpet/debian75-x64
vagrant box list
vagrant box remove

# Once you have the Vagrantfile set up, time to initialize.
# This will run the VM in a headless state (as VBoxHeadless process)
vagrant up

# tells what machine is running
vagrant status

# ssh
vagrant ssh

# by default Vagrant shares the project directory
# (dir with the Vagrantfile) to /vagrant inside the virtual machine
cd /vagrant/
```

# VirtualBox Configurations
----------------------

```ruby
Vagrant.configure(2) do |config|
	config.vm.box = "ubuntu/trusty32"

	# set hostname
	config.vm.hostname = "dev.google.com"

	# postup message
	config.vm.post_up_message = "Access at http://dev.google.com"

	# virtual box configuration
	config.vm.provider "virtualbox" do |v|
		# naming your VM
		v.name = "my-fantastic-project"
		# setting RAM to 1GB
		v.memory = 1024
		# set CPUs
		v.cpus = 2
	end
end
```

# Sharing Directories
----------------

`config.vm.synced_folder "host/relative/path", "/guest/absolute/path"`

```ruby
Vagrant.configure(2) do |config|
	# note that you can have this config multiple times but
	# it should only be used for source code since there is
	# heavy performance penalty on heavy I/O such as database files

	# first path is the host's path, which can by absolute
	# or relative to project's root directory

	# second path is the guest path, and it must be absolute.
	# It will always be created if it does not exist.
	config.vm.synced_folder ".", "/vagrant"

	config.vm.synced_folder "./some/dir/one", "/one", create:true
	config.vm.synced_folder "./some/dir/two", "/two", create:true

	# note that you can also add type. NFS is the faster
	# bidirectional file syncing. In order for this to work,
	# the host machine must have `nfsd` installed. It comes
	# preinstalled on OSX and is a simple package install on Linux
	config.vm.synced_folder "." "/vagrant", type: "nfs"

	# owner/group
	config.vm.synced_folder "." "/vagrant", owner: "root", group: "root"
end

# To reconfigure the guest machine, vagrant reload must be run.
# This halts the machine and starts it up again with
# the new configuration.
# It skips the initial step to clone the box since it is
# already created.
vagrant reload
```

# Networking
-------

```ruby
Vagrant.configure(2) do |config|

	# expose guest port 80 to host 8080
	config.vm.network :forwarded_port, guest: 80, host: 8800
end
```

# Teardown
-----

```bash
# suspends the state (uses host resources still)
vagrant suspend

# shut down
vagrant halt

# destroy all traces
vagrant destroy
```

# Writing Shell Scripts for Provisioning
-----------------------------------

```ruby
# note that paths are relative the the project root
config.vm.provision "shell", path: "provision.sh"
```

# provision.sh

```bash
#! /usr/bin/env bash

# note that all these will be run as sudo
echo "Installing Apache"
apt-get update >/dev/null 2>&1
apt-get install -y apache2 >/dev/null 2>&1
rm -rf /var/www
ln -fs /vagrant /var/www
```

# Sample Vagrantfile
---------------

```ruby
Vagrant.configure(2) do |config|
	config.vm.box = "precise64"
	config.vm.network :forwarded_port, guest: 80, host: 8800
	config.vm.provision "shell", path: "provision.sh"
end
```

# Networking In-Depth
----------------

Note that it is possible to compose networking options, so long as the guest machine has room for additional network interfaces. For example:

```ruby
# forwarded ports
config.vm.network :forwarded_port, guest: 80, host: 8800
# host-only networking
config.vm.network "private_network", ip: "192.168.33.10"
```

Also, note that with VirtualBox, Vagrant requires the first network device attached to the virtual machine to be a NAT device. Therefore, any host-only or bridged networks will be added as additional network devices and exposed to the virtual machine as "eth1", "eth2" and etc. "eth0" is generally always the NAT device. Note that it is not possible to override this requirement.

## Forwarded Ports

```ruby
Vagrant.configure(2) do |config|
	config.vm.network :forwarded_port, guest: 80, host: 8800
	# or with auto-collision detection/correction
	# and limit auto-correct ports between 2200-2250
	config.vm.usable_port_range = (2200..2250)
	config.vm.network :forwarded_port, guest: 80, host: 8800, auto_correct: true
end
```

```bash
vagrant reload
vagrant ssh
```

With forwarded ports, Vagrant will set up a port on the host to forward to a port on the guest, allowing you to access services on the guest without IP.

The benefit of using this is that it is very simple to setup. You just tell Vagrant in the Vagrantfile what ports to forward where.

But the simplicity comes at a cost. First you have the be explicit on the ports you want to forward. For basic web services, this is easy. But once you start handling databases and etc, it will become tedious very quickly. In addition, it will become accessible outside your own computer.

Finally, with VirtualBox, Vagrant can't forward to ports less than 1024 on the host system due to the operating system not allowing this for processes without admin privileges such as Vagrant. So, if you want to test SSL, it is not a good way to use port forwarding.

By default, forwarded ports only work with TCP connections. If you need to forward UDP packets as well, you have to configure an additional forwarded port with UDP port forwarding, ie: `config.vm.network :forwarded_port, guest: 80, host: 8800, protocol: "udp"`

## Host-Only Networking (Private Network)

With private network, you can do the following:

- avoid typing the configuration of each forwarded port separately
- avoid using port numbers such as 8080 for well-known services
- use same port numbers in different projects
- communicate between multiple guests
- use NFS for shared directories

```ruby
Vagrant.configure(2) do |config|
	# must enter static IP (any is fine)
	config.vm.network "private_network", ip: "192.168.33.10"
end
```

```bash
vagrant reload
vagrant ssh
ping 192.168.33.10
# inside guest vm, you can access host too (x.x.x.1)
ping 192.168.33.1
```

This creates a network that is private to your host and the guest machines on that host. Because it is a new custom network, it has its own IP address space, so guest machines with host-only networking get their own IP.

Vagrant supports host-only networks by specifiying a static IP for the machine. Vagrant will handle creating the host-only network and configuring the guest machine to get the specified IP.

This creates a secure network since only the host and guest can communicate to each other. Outside computers have no way of accessing network services you may be running.

A benefit of this however is that multiple virtual machines can communicate with each other by being part of the same network. With forwarded ports, a virtual machine can't talk to another virtual machine.

*You can separate services onto multiple virtual machines to more accurately mimic production, such as web servers and database servers.*

In addition, *the virutal machine can also communicate with the host itself.*

# Running Multiple Virtual Machines
------------------------------

This brings up two virtual machines. Note that you must destroy existing virtual machines from this project first before you do initialize this new Vagrantfile.

Note that the moment multiple machines are introduced into a Vagrant environment, the behavior of vagrant commands change a little bit. Most commands such as `up` `destroy` and `relaod` now take an argument with the name of the machine to affect. For example, `vagrant reload web`. If you specify no arguments, Vagrant assumes you want to take action on every machine - in this case, `vagrant reload` will reload both VMs. You can also add multiple arguments or even regular expressions, ie: `vagrant reload node1 node2 node3` or `vagrant reload /node\d/`

The ideal network when communicating between two guest machines is by using *host-only networking*

```ruby
Vagrant.configure(2) do |config|
	config.vm.box = "precise64"

	config.vm.define "web" do |web|
		web.vm.forwarded_port 80, 8080
		web.vm.provision :shell, path: "provision.sh"
		web.vm.provision :shell, inline: "apt-get -y install mysql-client"
		web.vm.network "private_network", "192.168.33.10"
	end

	config.vm.define "db" do |db|
		db.vm.provision :shell, path: "db_provision.sh"
		db.vm.network "private_network", "192.168.33.11"
	end
end
```

```bash
# db_provision.sh

# this makes it so that when installing MySQL server, it
# does not ask us questions for a root password and all that
export DEBIAN_FRONTEND=noninteractive
apt-get update
apt-get install -y mysql-server
# replace the bind address from loopback to 0.0.0.0 which
# means all interfaces. This is necessary so that remote
# machiens can connect to the server.
sed -i -e 's/127.0.0.1/0.0.0.0/' /etc/mysql/my.cnf
restart mysql
mysql -u root mysql <<< "GRANT ALL ON*.* TO 'root'@'%'; FLUSH PRIVILEGES;"
```

Remember to run `vagrant destroy` to start with a clean slate. Now, run `vagrant ssh web`

# Packaging Boxes
------------

`vagrant package`

The easiest way to create new boxes is using an existing Vagrant environment as a starting point.

This method is used to create new boxes that have more preinstalled software than their previous base. Therefore, if you want to support a new operating system, or a new bare-bones image, you’ll want to read the next section which instead creates new boxes from scratch.

Creating new boxes from an existing Vagrant environment is useful to preinstall and configure software in the base image so that vagrant up can be faster.

This is common practice for larger organizations where provisioning runs can take upward of an hour or more. This sort of provisioning time drastically reduces the disposability of Vagrant environments, harming a critical productivity boost of the tool. To build a box from an existing environment, you’ll first need an existing environment, so vagrant up! After doing this, install any software you’d like on the machine.
