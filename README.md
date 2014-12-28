bash-examples
=============

My notes on easy to forget bash 

Linux Containers (lxc)

Each of the following commands use "hadoop11" as the container name or hostname

install lxc

    apt-get install lxc

create a container - takes a few minutes on the first run

    sudo lxc-create -t ubuntu -n hadoop11

start a container

    sudo lxc-start -d -n hadoop11

list containers and their status

    sudo lxc-ls --fancy

inspect one of the containers deeper

    sudo lxc-info -n hadoop11
  
enter the console of the container - like ssh into the box

    sudo lxc-attach -n hadoop11

stop a container

    sudo lxc-stop -n hadoop11

destory a container

    sudo lxc-destroy -n hadoop11

clone a container - create a new container hadoop12 from the existing hadoop11

    sudo lxc-clone -o hadoop11 -n hadoop12
    
lxc networking - delete the dhcp assigned ip addresses (all containers must be stopped)

    sudo service lxc-net stop
    sudo rm /var/lib/misc/dnsmasq.lxcbr0.leases 
    sudo service lxc-net start
