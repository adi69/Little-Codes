##Virtualenv
virtualenv is a tool to create isolated Python environments. 

`virtualenv venv` will: <br>
Create a folder in the current directory which will contain the Python executable files, <br>
Create a copy of the `pip` library which you can use to install other packages. <br>
The name of the virtual environment (in this case, it was venv) can be anything. <br> 
Omitting the name will place the files in the current directory instead. <br>
This creates a copy of Python in whichever directory you ran the command in, placing it in a folder named venv.<br>

Use a Python interpreter of your choice: `$ virtualenv -p /usr/bin/python2.7 venv`

To activate virtualenv: `$ source venv/bin/activate`

The name of the current virtual environment will now appear on the left of the prompt (e.g. `(venv)Your-Computer:your_project UserName$`). <br>
From now on, any package that you install using pip will be placed in the venv folder, isolated from the global Python installation.
Install packages as usual, using pip install ... 

To deactivate virtualenv: `$ deactivate`


##Vagrant
*  Vagrant is computer software that **creates and configures virtual development environments.**
* **Vagrantfile**
	- It describe the type of machine required for a project, and how to configure these machines.
	- The syntax of Vagrantfiles is Ruby.
	- When you run any vagrant command, Vagrant climbs up the directory tree looking for the first Vagrantfile it can find, starting first in the current directory.
* **BOXES**
	- Instead of building a virtual machine from scratch, which would be a slow and tedious process, Vagrant uses a base image to quickly clone a virtual machine. These base images are known as "boxes" in Vagrant, and specifying the box to use for your Vagrant environment is always the first step after creating a new Vagrantfile.
	- `$ vagrant box add hashicorp/precise64` //This will download the box named "hashicorp/precise64" from HashiCorp's Atlas box catalog, a place where you can find and host boxes.
	- Boxes are globally stored for the current user. 
	- Boxes are broken down into two parts - the username and the box name - separated by a slash. In the example above, the username is "hashicorp", and the box is "precise64" (ubuntu 12.04).
	- Using a box: `config.vm.box = "hashicorp/precise64"`

	
* `vagrant init` or `vagrant init ubuntu/trusty64` -> `vagrant up` -> `vagrant ssh`
	
* to logout of another user `exit`
* `sudo service --status-all`
* search packages `sudo aptitude  search postgres`

* **PROVISION**
    - Vagrant has built-in support for automated provisioning. Using this feature, Vagrant will automatically install software when you vagrant up so that the guest machine can be repeatably created and ready-to-use.
    - Create the following shell script and save it as bootstrap.sh in the same directory as your Vagrantfile:
    
   	```
    #!/usr/bin/env bash

    apt-get update
    apt-get install -y apache2
    if ! [ -L /var/www ]; then
      rm -rf /var/www
      ln -fs /vagrant /var/www
    fi
    ```

    - Add to the _Vagrantfile_: 
    
    ```
    config.vm.provision :shell, path: "bootstrap.sh"
    ```

    The "provision" line is new, and tells Vagrant to use the shell provisioner to setup the machine, with the bootstrap.sh file. The file path is relative to the location of the project root (where the Vagrantfile is).

    now just vagrant up or if it's already running then `vagrant reload --provision`

* **NETWORKING**
	
	```
	config.vm.network :forwarded_port, guest: 80, host: 4567
	```
	Once the machine is running again, load http://127.0.0.1:4567 in your browser. You should see a web page that is being served from the virtual machine that was automatically setup by Vagrant.
	
	guest = vagrant; host = machine/system on which vagrant is running
	
	