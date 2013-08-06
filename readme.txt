====================================================================
NB: THE LATEST VERSION OF THIS FILE IS AVAILABLE AT:
https://github.com/rdoh/cpm-go/blob/master/readme.txt. 
Please correct as necessary.
====================================================================

====================================================================
Setting up the OpenMRS Concept Proposal Module (CPM) Virtual Machine
====================================================================

Following these instructions will start a Linux virtual machine running the OpenMRS web app.

1) Copy the contents of the USB key to a directory on your hard drive (assuming C:\openmrs for the remainder of this 
read me). 

2) Install VirtualBox and Vagrant (using the installers in the Windows and MacOS directories on the USB key). 
Don't download the latest VirtualBox - it doesn't work - use the provided 4.2.12.

3) Open up a command prompt and run:
    cd c:\openmrs
    vagrant box add openmrs-dev ./openmrs-dev.box
    vagrant init
    vagrant up

4) The OpenMRS web app is now viewable at http://192.168.33.10:8080/openmrs/.
Login with username "admin" and password "Admin123" and follow the prompts there to finish installation. 
When prompted, set the MySQL root password to "OpenMRS".


==================================
Developers only: in order to code:
==================================

4) Open an SSH session to the VM (username: vagrant, password: vagrant):
    On OSX: "vagrant ssh"
    On Windows: install PuTTY from Windows/ on the USB key, and use it to connect to localhost:2222 

5) Now on the VM, check out the source code:
    cd /vagrant/code
    git clone https://github.com/OpenMRS-Australia/openmrs-cpm.git

The VM directory /vagrant/code/openmrs-cpm is shared with your host machine at C:\vagrant-openmrs\code\openmrs-cpm. 
Feel free to code in the IDE of your choice or use VIM.
                         
To compile the package, run on the VM:
    ./go package

This generates a module file at /vagrant/code/openmrs-cpm/omod/*.omod. For testing, you can login to the web app and
manually upload the module via the Administration page.
