bash-examples
=============

My notes on easy to forget bash 

See also: <a href="http://worldoflinux.org/learn-linux.php">http://worldoflinux.org/learn-linux.php</a>

Basics

make a file executeable

    sudo chmod +x myfilename

recursively change the permissions of a folder called lxc: owner/group/everyone read(4)/write(2)/execute(1)

    sudo chmod -R 744 lxc

release your ip address

    sudo dhclient -r

renew your ip address

    sudo dhclient

print all of the processes that are attached to a port

    sudo netstat -nlp

print the directories in your path, replacing colons with new line characters and sorted.  edit ~/.bashrc or .profile to add new path search directories

    echo $PATH | tr : '\n' | sort

find a process - use this to find the PID (process id number) used in kill command

    top

find all processes for a program, e.g. all instances of google chrome

    pgrep chrome

stop a process - assuming it's PID is 1234

    kill -9 1234

kill all instances of a program

    pkill chrome

<a href="http://hisham.hm/htop/index.php?page=comparison">an enhanced version of top</a>, but you can click to sort by columns and kill a process without leaving htop.  it requires separate installation though.

    sudo apt-get install htop
    htop

ssh into a box using a key:  assuming you are in the same directory as your key (.pem) and your username is ubuntu on a AWS instance

    ssh -i mykey.pem ubuntu@ec2-###-###-###-###.compute-1.amazonaws.com

copy all *.mp4 files from local to remote

    scp /home/user/videos/*.mp4 remoteusername@192.168.1.1:/server/remote/folder/videos

copy from remote to local

    scp remoteusername@192.168.1.1:/server/remote/folder/videos/*.mp4 scp /home/user/videos/

download all files from an ftp server using the -r recursive option

    wget -r ftp://#.#.#.#:3721/DCIM/Camera/

add a user - add a new user

    sudo adduser khaldrogo
    
add a user to the sudo list

    sudo adduser khaldrogo sudo

add a user to the sudo list (alternate method)

    # switch to root 
    su -
    # install sudo if necessary
    apt-get install sudo
    #edit the sudoers file with this, and not another editors
    visudo
    #add this line below the similiar root line
    root	ALL=(ALL:ALL) ALL
    khaldrago	ALL=(ALL:ALL) ALL

switch users

    sudo su - khaldrago

change password

    sudo passwd khaldrogo

version and codename of your OS 

    lsb_release -a

count the number of files in the current directory.  note:  this is the #1, not letter l in the ls command

    ls -1 | wc -l

untar and unzip a file

    tar -xzf hadoop-2.6.0.tar.gz

search for a file (dot searches from the current folder, recursively.  -iname rather than just -name makes it case insensitive)

    find . -iname "myfilename.txt"
    
Find “some text within a file” recursively

    find "text within a file" . -R

sed:  if the first character is X, then delete it.  This returns "yz"

    echo "xyz" | sed 's/x//' 

sed:  if the first character is X, then replace it with something  This returns "somethingyz"

    echo "xyz" | sed 's/x/something/' 

disk usage:  -h for human readable, and the next argument is the mount point to analyze

    df -h /

Mounting a Network Drive

    sudo apt-get install cifs-utils
    mkdir /mnt/usb125MB
    sudo mount -t cifs //192.168.1.1/USB_Storage /mnt/usb125MB -o guest

Find the name of the usb by running this, then plugging in the usb, and running it again.  This one is called  <i>sdc</i>.

    dmesg | tail
    <sample output>[63089.935649] sd 8:0:0:0: [sdc] Attached SCSI removable disk

Format a usb drive (assumed to be sdc) to FAT32.

    sudo mkfs.vfat -I -F 32 /dev/sdc

Run the unetbootin app as root (fixes the blank gray screen issue)

    sudo QT_X11_NO_MITSHM=1 /usr/bin/unetbootin

Backup to AWS Glacier - assumes AWS CLI is install and "aws configure" has been run

    #tar and zip all of the files in the directory to backup
    tar -zcvf 2015-12-11-backup.tar.gz /home/ben/Pictures/Phone/*
    #upload files to glacier, in the vaut named my-vault
    aws glacier upload-archive --account-id - --vault-name my-vault --body 2015-12-11-backup.tar.gz


R - Statistical Programming

install R - installs R and says yes when prompted for whether you to want to download it

    sudo apt-get install -y r-base r-base-dev

install <a href="#http://mran.revolutionanalytics.com/documents/rro/installation/#revorinst-lin">Revolution R Open</a> instead

    sudo apt-get update
    sudo dpkg -l make gcc gfortran g++ # to see if you need to do the next 4
    sudo apt-get install make -y
    sudo apt-get install gcc -y
    sudo apt-get install gfortran -y
    sudo apt-get install g++ -y
    
    wget http://mran.revolutionanalytics.com/install/RRO-8.0.1-Beta3-Ubuntu-14.04.x86_64.tar.gz
    tar -xzf RRO-8.0.1-Beta3-Ubuntu-14.04.x86_64.tar.gz
    sudo ./install.sh


add cran mirror:  assumes you want the revolution analytics mirror and are running the trusty flavor of linux.  the "tee" part prints the result to the console and appends (the -a option) to sources.list.

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

For Fedora 22, see <a href="http://www.kianmeng.org/2015/05/linux-containers-lxc-in-fedora-22.html">this link</a> for networking bridge workarounds.

Config file location

    /var/lib/lxc/<container name>/config
    /var/lib/lxc/hadoop11/config

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

installing the <a href="http://lxc-webpanel.github.io/index.html">LXC Web Panel</a> for GUI admin of the containers.  accessible via localhost:5000, default username and password of admin/admin.

    wget http://lxc-webpanel.github.io/tools/update.sh -O - | bash
    
share a folder between the host and a container.  first create /mnt/share within the container and exit the container.  then edit the containers config file, and add a mount point where both folders are relative to the host. this example links the hosts Downloads folder to /mnt/share in the container.

    cd mnt
    mkdir share
    exit
    sudo gedit /var/lib/lxc/hadoop11/config
    add this line:  lxc.mount.entry = /home/ben/Downloads /var/lib/lxc/hadoop11/rootfs/mnt/share none bind 0 0


Vagrant

start a vagrant VM, first navigate to the directory where the VAGRANT file is located

    cd ~/Documents/edx/spark/mooc-setup-master
    vagrant up
    
shut down a vagrant VM

    vagrant halt
    
delete the vagrant VM

    vagrant destory
    
share files between the VM and the host.  move the files on the VM to /vagrant and the files will be in the same folder as the Vagrantfile on the host

    cp /myfolder/filesToShare /vagrant

