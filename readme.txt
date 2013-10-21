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

3) Open up a command prompt and run:
    cd c:\openmrs
    vagrant up 
    (MacOS users: if you see an error about network interfaces here, try 'sudo vagrant up')

4) The OpenMRS web app is now viewable at http://192.168.33.10:8080/openmrs/. 
    You can login with username: admin, password: Admin123.
    
At this point you will have a running instance of OpenMRS with an out-of-date version of the CPM installed.

=========================================================================================
Continue in order to pull/push updates, build, package, install and test the CPM locally:
=========================================================================================

5) Fork the openmrs-cpm repository at https://github.com/OpenMRS-Australia/openmrs-cpm (button at top right of the page).

6) Open an SSH session to the VM:
    On OSX: "vagrant ssh"
    On Windows: install PuTTY from Windows/ on the USB key, and use it to connect to localhost:2222 (u: vagrant, p: vagrant). 

7) Now on the VM, clone the source code from your newly forked repository:
    $ cd /vagrant/code
    $ git clone https://github.com/<your-username>/openmrs-cpm.git

8) From the same directory, add the upstream repo so that you can pull changes:
    $ git add upstream https://github.com/OpenMRS-Australia/openmrs-cpm

The VM directory /vagrant/code/openmrs-cpm is shared with your host machine at C:\openmrs\code\openmrs-cpm. 
Feel free to code in the IDE of your choice or use VIM.

To compile the package, run on the VM:
    $ cd /vagrant/code/openmrs-cpm
    $ ./go

This generates a module file at /vagrant/code/openmrs-cpm/build/libs/*.omod. 
For testing, you can login to the web app and manually upload the module via 
Administration -> Manage Modules -> Add or Upgrade Modules.

9) When you want to shutdown the VM, use the following command:
    $ vagrant halt

=========================================================================================
Troubleshooting
=========================================================================================
1. VM is not connected to the internet to clone the code.
---> Probably you have started VM before connecting to the internet, easiest way to solve 
     the problem is to shutdown the VM and starting it up again:
     $ vagrant halt
     $ vagrant up     
     
2. I get the following error when trying to run "vagrant":
    VBoxManage.exe: error: Could not rename the directory 'C:\Users\...\VirtualBox VMs\openmrs-dev_1' to
    'C:\Users\...\VirtualBox VMs\openmrs-dev' to save the settings file (VERR_ALREADY_EXISTS)
---> You need to delete the following folders:
    i.  "C:\Users\...\VirtualBox VMs\openmrs-dev_1"
    ii. "C:\Users\...\VirtualBox VMs\openmrs-dev"
    iii. in current openMRS folder, the hidden ".vagrant" folder. (You can see it with "dir /a" command in
    windows or "ls -a" in Unix based systems)
   > Run the following command:
   $ vagrant destroy
   > Start vagrant again
   $ vagrant up
   > If you get further errors, run vagrant in debug mode for further troubleshooting
   $ VAGRANT_LOG=DEBUG vagrant up
    
3. I get the following error when trying to run "vagrant":
   failed to untar the box file.
---> You have the older version of vagrant on your system. Uninstall it and install the latest version.

4. When starting to run "./go" script, I get the following error:
   ERROR: JAVA_HOME is set to an invalid directory: /usr/
---> run the following command from your ssh command prompt:
   $ export JAVA_HOME="/usr/"
