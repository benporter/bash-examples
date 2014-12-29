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

change password

    sudo passwd khaldrogo

version and codename of your OS 

    lsb_release -a


R - Statistical Programming

install R - installs R and says yes when prompted for whether you to want to download it

    sudo apt-get install -y r-base r-base-dev

add cran mirror:  assumes you want the revolution analytics mirror and are running the trusty flavor of linux

    echo "deb http://cran.revolutionanalytics.com/bin/linux/ubuntu trusty/" | sudo tee -a /etc/apt/sources.list
    gpg --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys E084DAB9
    gpg -a --export E084DAB9 | sudo apt-key add -

install an r package:  installs the sqldf package

    sudo apt-get update
    sudo apt-get install r-cran-sqldf

install RStudio Server - port 8787, download the appropriate file for your machine first from:  <a href="http://www.rstudio.com/products/rstudio/download-server/">http://www.rstudio.com/products/rstudio/download-server/</a>

    sudo apt-get install gdebi-core
    sudo apt-get install libapparmor1 # Required only for Ubuntu, not Debian
    gdebi rstudio-server-0.98.1091-amd64.deb

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
