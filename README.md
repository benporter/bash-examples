bash-examples
=============

My notes on easy to forget bash 


Basics

recursively change the permissions of a folder called lxc: owner/group/everyone read(4)/write(2)/execute(1)

    sudo chmod -R 744 lxc

release your ip address

    sudo dhclient -r

renew your ip address

    sudo dhclient

find a process - use this to find the PID (process id number) used in kill command

    top

stop a process - assuming it's PID is 1234

    kill -9 1234

ssh into a box using a key:  assuming you are in the same directory as your key (.pem) and your username is ubuntu on a AWS instance

    ssh -i mykey.pem ubuntu@ec2-###-###-###-###.compute-1.amazonaws.com

add a user - add a new user

    sudo adduser khaldrogo

version and codename of your OS 

    lsb_release -a


R - Statistical Programming

install an r package:  installs the sqldf package

    sudo apt-get update
    sudo apt-get install r-cran-sqldf

install R - installs R and says yes when prompted for whether you to want to download it

    sudo apt-get install -y r-base r-base-dev

TBD - install RStudio

Linux Containers (lxc)

Each of the following commands use "hadoop11" as the container name or hostname

install lxc

    sudo apt-get install lxc

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
