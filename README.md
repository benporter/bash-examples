bash-examples
=============

My notes on easy to forget bash 

Linux Containers (lxc)

Each of the following commands use "hadoop11" as the container name or hostname

install lxc

    apt-get install lxc

create a container

    sudo lxc-create -t ubuntu -n hadoop11

start a container

    sudo lxc-start -d -n hadoop11

list containers and their status

    sudo lxc-ls --fancy

inspect one of the containers deeper

    sudo lxc-info -name hadoop11
  
enter the console of the container

    sudo lxc-attach -n hadoop11

stop a container

    sudo lxc-stop -n hadoop11

destory a container

    sudo lxc-destroy -n hadoop11
