# Devstack on Ubuntu 18.04

Devstack Requirements:
  -  Fresh installation of Ubuntu 18.04
  -  Minimum memory of 4 GB
  - At least 2 vCPU's
  - Storage capacity of at least 10 GB
  - Internet connection
  - User with admin pribileges
  
Run:
```sh
$ sudo apt update
$ sudo apt -y upgrade
$ sudo apt -y dist-upgrade
$ init 6
```

After the system reboots run:
```sh
$ sudo useradd -s /bin/bash -d /opt/stack -m stack
$ sudo -i
$ echo "stack ALL=(ALL) NOPASSWD: ALL" | sudo tee /etc/sudoers.d/stack
$ su - stack
$ sudo apt -y install git
$ git clone https://opendev.org/openstack-dev/devstack -b master
$ cd devstack
```

The next step is to get the IP address of your Server/VM. Some commands that could help you are `ip a`, `ip addr` and `ifconfig`.

Now you must create/edit the configuration file of Devstack:
```sh
$ nano local.conf
```

Paste:

```sh
[[local|localrc]]
ADMIN_PASSWORD=StrongAdminSecret
DATABASE_PASSWORD=$ADMIN_PASSWORD
RABBIT_PASSWORD=$ADMIN_PASSWORD
SERVICE_PASSWORD=$ADMIN_PASSWORD
HOST_IP=$YOUR_HOST_IP
```

If you want to enable some Openstack services you must add them in the local.conf file. For example, for Heat add these lines:

```sh
enable_service h-eng h-api h-api-cfn h-api-cw
enable_plugin heat https://opendev.org/openstack/heat master
```
Change the ownership:

```sh
sudo chown stack: /opt/stack/*
```

Last step is to eliminate some conflicts. Run:

```sh
$ sudo apt-get remove python3-simplejson -y
$ sudo apt-get remove --auto-remove python3-simplejson -y
```

Execute the Devstack installation script:
```sh
$ ./stack.sh
```
