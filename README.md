## Welcome to **stackinabox.io**

### Introduction

**stackinabox.io** is a Vagrant project that configures a single Ubuntu 14.04 VirtualBox VM with OpenStack and Docker. The project pulls Docker images for [UrbanCode Deploy](https://hub.docker.com/r/stackinabox/urbancode-deploy/), the UrbanCode Deploy [agent](https://hub.docker.com/r/stackinabox/urbancode-deploy-agent/) and [relay](https://hub.docker.com/r/stackinabox/urbancode-deploy-agent-relay/), the [UrbanCode Deploy Blueprint Designer](https://hub.docker.com/r/stackinabox/urbancode-patterns-designer/), and the [Blueprint Designer Engine](https://hub.docker.com/r/stackinabox/urbancode-patterns-engine/).  Using this Vagrant project, users are able to develop OpenStack Heat-based automation to deploy complex, multiplatform applications to the embedded [OpenStack](https://www.blueboxcloud.com/) instance or to any other cloud provider supported by the [Blueprint Designer](https://developer.ibm.com/urbancode/products/urbancode-deploy/features/blueprint-designer/), including [Amazon Web Services](https://aws.amazon.com/), [SoftLayer](http://www.softlayer.com/), [Azure](https://azure.microsoft.com/), or even your on-premise [VMware vCenter](https://www.vmware.com/products/vcenter-server).

Using **stackinabox.io**, our hope is that you will share the automation you develop with the larger community.  For an example, check out the [JKE Banking Application](https://github.com/stackinabox/jke).

### Future Integrations

The plan is to add further Docker images to this Vagrant environment to support many other deployment automation tools such as:

  - [UrbanCode Build](https://developer.ibm.com/urbancode/products/urbancode-build/) (not yet implemented)
  - [UrbanCode Release](https://developer.ibm.com/urbancode/products/urbancode-release/) (not yet implemented)
  - [Chef Server](https://www.chef.io/chef/) (not yet implemented)
  - [Salt Stack](https://saltstack.com/) (not yet implemented)
  - [Puppet](https://puppet.com/) (not yet implemented)

### Shortcuts
- [Setup](https://github.com/stackinabox/stackinabox.io#setup-instructions)
- [Halt, Resume, or Destroy](https://github.com/stackinabox/stackinabox.io#halt-resume-or-destroy-the-vagrant-machine)
- [Access Information](https://github.com/stackinabox/stackinabox.io#access-information)
- [Additional Steps](https://github.com/stackinabox/stackinabox.io#additional-steps)
- [Troubleshooting](https://github.com/stackinabox/stackinabox.io#troubleshooting)

___
### Setup Instructions

#### Download and install these tools  

  - [Oracle VirtualBox](https://www.virtualbox.org/wiki/Downloads)  
  - [Vagrant](https://www.vagrantup.com/downloads.html)
  - [Git](https://git-scm.com/) 

#### Install required Vagrant plugins  
````
vagrant plugin install vagrant-docker-compose
vagrant plugin install vagrant-multi-hostsupdater
```

#### Run `vagrant up`
Verify that VirtualBox, Vagrant, and Git are installed and running by typing `vboxmanage help`, `vagrant help`, and `git help` at the command shell.  

Execute these commands:
````
git clone https://github.com/stackinabox/stackinabox.io.git 
cd stackinabox.io/vagrant
vagrant up
```

The `vagrant up` command will take a while to complete.  The project will download the OPDK (Open Patterns Development Kit) VirtualBox VM from atlas.hashicorp.com. Once downloaded, Vagrant will launch the VM in VirtualBox in "headless" mode (no GUI).  When the VM is up, Docker Compose is used to start the UrbanCode products in multiple containers.  The Vagrant project will also set several entries in your machine's `hosts` file, and this step may prompt for a password.  You will see this output at the end of the process coming from a `docker-compose up` command on the VM:
````
...
==> opdk: Creating ucddb
==> opdk: Creating heatengine
==> opdk: Creating blueprintdb
==> opdk: Creating ucd
==> opdk: Creating blueprintdesigner
==> opdk: Creating agent
==> opdk: Creating agent-relay
...
```

When the Vagrant environment is up, you can open your local web browser to the [Blueprint Designer](http://designer.stackinabox.io:9080/lanscaper) and login with `demo`/`labstack`.  The demo user is intended to be the primary user for building your automation.  The demo user belongs to a 'demo' team in the Blueprint Designer and has it's own tenant in the embedded [OpenStack](http://bluebox.stackinabox.io).  Additional login information is provided below.

___
### Halt, Resume, or Destroy the Vagrant Machine
You can run `vagrant global-status` to see a list of running Vagrant machines and their IDs.

#### Halt
To halt/suspend a machine, naviagte to the directory where you cloned the stackinabox.io repository and execute `vagrant halt`:
````
cd /path/to/stackinabox.io/repo
vagrant halt
```

#### Resume
To resume a halted machine and restore all previous work/data:
````
cd /path/to/stackinabox.io/repo
vagrant resume
```

#### Destroy
To destroy a Vagrant machine and restart with a clean slate:
````
cd /path/to/stackinabox.io/repo
vagrant destroy <vagrant-env-id>
vagrant up
```
___
### Access Information
[OPDK Terminal](http://192.168.27.100:4200/) - Available at http://192.168.27.100:4200/
- Login with `vagrant`/`vagrant`


[OpenStack](http://openstack.stackinabox.io) - Available at http://openstack.stackinabox.io 
- Login with `demo`/`labstack` or `admin`/`labstack`

	 
[UrbanCode Deploy Server](http://ucd.stackinabox.io:8080) - Available at http://ucd.stackinabox.io:8080
- Login with `admin`/`admin`


[Blueprint Designer](http://designer.stackinabox.io:9080/landscaper) (Heat Template Designer) - Available at http://designer.stackinabox.io:9080/landscaper
- Login with `demo`/`labstack` or `ucdpadmin`/`ucdpadmin`


[UrbanCode Deploy Heat Engine](http://heat.stackinabox.io:8004) - Verify at http://heat.stackinabox.io:8004

___
### Additional Steps

#### Connect the Blueprint Designer to AWS
Open your browser to the [OPDK web terminal](http://192.168.27.100:4200/), login, and execute `~/aws-setup.sh`.  Then follow the instructions at the end of the script execution.

#### Install the JKE Sample
Open your browser to the [OPDK web terminal](http://192.168.27.100:4200/).  Login and execute the following commands:
````
git clone https://github.com/stackinabox/jke.git /vagrant/patterns/jke
cd /vagrant/patterns/jke
./init.sh
```

Open your browser to the [JKE Tutorial](http://designer.stackinabox.io:9080/landscaper/view/tutorial) and login with `demo`/`labstack`.  You will see a "Guided Tour" frame on the right side of the browser window.  Follow the instructions which will guide you on how to deploy the JKE sample using UrbanCode Deploy and Blueprint Designer.

___
### Troubleshooting

#### Failure while downloading OPDK
You may have timeout errors or see the following while downloading OPDK after running `vagrant up`:
```
==> opdk: Adding box 'stackinabox/opdk' (v0.9.4) for provider: virtualbox
    opdk: Downloading: https://atlas.hashicorp.com/stackinabox/boxes/opdk/versions/0.9.4/providers/virtualbox.box
==> opdk: Box download is resuming from prior download progress
An error occurred while downloading the remote file. The error
message, if any, is reproduced below. Please fix this error and try
again.

SSL read: error:00000000:lib(0):func(0):reason(0), errno 60
```

In this case you should download the OPDK VM directly via your browser (preferably using a download manager plugin).  Once you have the VirtalBox VM downloaded locally (`.box` file), you can add the VM to your Vagrant setup using a `vagrant box add` command:
````
$ vagrant box add /tmp/Downloads/opdk.box --name stackinabox/opdk
==> box: Box file was not detected as metadata. Adding it directly...
==> box: Adding box 'stackinabox/opdk' (v0) for provider: 
    box: Unpacking necessary files from: file:///tmp/Downloads/opdk.box
==> box: Successfully added box 'stackinabox/opdk' (v0) for 'virtualbox'!
```

Next, you must modify your Vagrantfile by adding a `config.vm.box.url` line and commenting out the `opdk.vm.box_version` line:
````
...
config.vm.box_url = ["file:///tmp/Downloads/opdk.box"]
config.vm.define "opdk" do |opdk|

	opdk.vm.box = "stackinabox/opdk"
      # opdk.vm.box_version = "= 0.9.4"
...
```

Retrying the `vagrant up` command should now produce the following output to start:
````
Bringing machine 'opdk' up with 'virtualbox' provider...
==> opdk: Importing base box 'stackinabox/opdk'...
```

#### `ttyname failed` and `Inappropriate ioctl for device` messages
See this [issue](https://github.com/stackinabox/stackinabox.io/issues/1).