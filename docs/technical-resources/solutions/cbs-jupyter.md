---
title: Use a cloud virtual machine as a Jupyter notebook server
parent: Solutions
grand_parent: Technical Resources
---

* [Ok now what is the plan?](#what-is-the-plan)
    * (and while we are it it: [How does one simplify the bash terminal display](https://robfatland.github.io/greenandblack/)?)
* [Why does this require instructions?](#why-instructions-are-necessary)
    * [Key concepts](#key-concepts)
* [Let's do it!](#walk-through)
    * [short version](#short-version)
    * [extended version](#extended-version) 

### What is the plan?

Learning about how to compute on the cloud includes understanding **machine images**:
Snapshots of the operating system including customization, code and data. This CloudBank solution
serves two purposes: It introduce machine images and also demonstrates using a 
cloud virtual machine (herein **VM**) as a traditional desktop... potentially quite a powerful one.


Since [Jupyter notebooks](https://jupyter.org) are in common use, our aim will be to operate the cloud VM
as a Jupyter notebook server. The interface will appear in our local machine's browser. 
We move information back and forth securely using an **ssh tunnel**.


### Why instructions are necessary


At a high level we are configuring a research computer. Because it is on the 
cloud there is some extra vocabulary involved and some extra steps, starting with securing 
a cloud account. There is enough complexity to merit a walk-through. Here's the narrative:


* A researcher decides to work on the public cloud; and gains access...
    * Cloud access agreements commonly are called 'accounts' or 'subscriptions'
    * CloudBank uses the term *billing account*
* Using a browser interface the researcher starts a dedicated cloud VM and logs in
    * The VM begins as a blank slate, just Ubuntu Linux
    * The walk-through given below happens to use the AWS cloud
* The researcher configures the VM as a Jupyter Lab notebook server supporting a Python 3 kernel
    * Other useful tools / libraries / packages / datasets are installed as well
    * The researcher tests the Jupyter Lab server by connecting via browser
        * This makes use of a secure connection called an *ssh tunnel*
* The researcher uses cloud management tools to create an ***image*** of the VM on the cloud
    * Unlike a container this image is specific to the cloud used; it is not transferrable between clouds
* The researcher Terminates (deletes) the VM: It is no longer available
    * The stored ***image*** persists and can be used to start up a new VM


A VM may be in two states: Started and Stopped. You pay for it by the hour only when it is Started.
Stopped is like having the power turned off: You can resume using it later without loss of data.
Terminating a VM means it no longer exists: Everything is gone. 


Starting a VM using a stored image recreates the machine in its state when the image was created.
When you do this you have a choice of which VM to use for the image. You can use a smaller
cheaper VM if you do not need a lot of computing power; say you just want to write and test some code. 
Or you can choose a powerful (more expensive) VM if you have some heavy computation to do.


Notice that you may start many *VMs* from a single *image*. Your collaborators may use an image
to start their own VMs as well. This means that customized work environments are easy to replicate
and share.

#### Key concepts

* Authentication
    * *Authentication* is "logging in", i.e. you are authenticated to use a resource on the cloud
    * On the cloud there are (at least) three authentication paths: Username/passord, login keypairs and IAM Access Keys
    * Authentication is generally **credential** use over a **secure connection**
        * The **secure connection** is built into applications like **ssh** as a protocol: It just works.
        * The **credential** may be a file containing long text strings: Guard as if they are passwords to your bank account
            * Cautionary tale: An Access Key file for your cloud account can be used to automatically run tasks
                * This means that someone with your access key can easily spend $15,000 in one hour mining bitcoin
                * ***Do not place credentials on Virtual Machines that will be used to create images per this walk-through***

* Virtual Machines (VM / VM instance / "EC2" on AWS, "Google Compute Engine" on GCP, "Azure VM" on Azure)
    * There are a wide variety of instance types to choose from.
        * Any given instance type will have associated memory, processing power and storage (see below).
        * The cost of the instance -- per hour -- varies with these characteristics.
        * On the AWS console you select the EC2 service and then Launch Instance
            * This runs a launch wizard where your first step is to choose an operating system.
            * The second step is to choose an instance *type*
                * You should have a good idea of what this <instance-type> will cost per hour of operation
                * It can be challenging to find instance cost
                * Enter `cost <instance-type> <region>` in a browser search bar; for example:
                    * `cost m5ad.4xlarge oregon` shows $1.00 per hour as the first result

* Storage: AWS-specific details
    * Disk drive version 1: Elastic Block Storage (EBS) is persistent storage, acting as a disk drive + file system
    * AWS also supports temporary disk storage through the [*instance store*](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html)
    * It is essential to distinguish between **Elastic Block Strage (EBS) volumes** and **(ephemeral) instance store volumes**
        * Instance store volumes evaporate; so they are only useful for temporary storage. 
        * If we Stop and then Start an image: The instance store file system will still be there but all files are gone. 
    * To summarize
        * Use EC2 instance store volumes for temporary storage where you do not need to keep data
        * Use EC2 EBS block storage as persistent disk space when you need to keep data
        * Use AWS EFS (Elastic File System) as storage shared by multiple instances; analogous to UNIX NFS
        * Use AWS S3 Object Storage to store any (large) amounts of data cheaply, independent of EC2 resources

### Walk-through
   
Again these use AWS as the "target cloud" but these notes will apply broadly to any cloud.
The short version describes what to do with scant attention to specifics.
The extended version is more comprehensive.
   
#### Short version
   
* Install a bash shell if needed on your Local computer (Local is a laptop, dekstop etc)
* Start a cloud VM, note three things:
   * the generic VM username (here we use `ubuntu`)
   * the ip address of the VM (here we use `12.23.34.45`)
   * download the key pair file to Local (here we use `keypair.pem`)
* Assign this VM a stable ip address
   * This does not change even when you stop and re-start the VM
   * On AWS the persistent ip address is provided via the Elastic IP service
* `chmod 400 keypair.pem` and `ssh -i keypair.pem ubuntu@12.23.34` to log in to the cloud VM from Local
* From the command line on the cloud VM install `miniconda`, `jupyter` and other Python libraries
* Use `git clone https://github.com/organization/repository` on the cloud 
   * If you need a working example you can use organization = `cloudbank-project` and repository = `ocean`

left off here
   
   
   
   
#### Extended version

* Log on to the AWS console and select Services > Compute > EC2 > Launch Instance
    * This may also be done with the Command Line Interface (CLI) to AWS or an API such as `boto3`
* Run through the launch Wizard; here are some details by example:
    * Image: 64 bit x86 Ubuntu Server (username = `ubuntu`): Bare machine image, just the OS
    * VM: `m5ad.4xlarge`, cost $1.00 / hour in Oregon at time of writing
        * This instance type includes two 300 GB instance store volumes (temporary storage)
            * Data on these volumes will vanish on stops, terminations, or hardware failures
        * ***We strongly recommend following through this tutorial to the Terminate stage!!!***
            * Failure to do so may result in you being charged for this Virtual Machine at this rate.
            * We refer to this as zombie resource charges: You may forget about it but the cloud provider will not.
    * Configure instance: Default values
    * Add storage: Default values 
        * Note
            * By selecting the `m5ad.4xlarge` we already have 2 300 GB SSD EBS drives on this instance
            * EBS = Elastic Block Storage (AWS acronym), meaning "big empty disk drives"
            * See the note below on mounting these drives to make them useful
    * Add Tags: Added key values `Name`, `Project`, `Date`, `Owner`, `URL`
    * Configure Security Group: Default values
    * Review and Launch: Created a temporary keypair file, downloaded
        * See the main page of this repo for details
    * Launch: Note resulting ip address of the VM; let's say it is `12.23.34.45`

#### From my own computer log on to the AWS EC2 instance

* Relocate the downloaded keypair file to my `bash` home directory
* `chmod 400` applied to keypair file
* `my computer> ssh -i keypair.pem ubuntu@12.23.34.45`


#### On the EC2 instance (VM) mount any added storage drives





* Assumes you are logged on with a bash shell
* This example works with temporary *instance store* volumes; will also work with EBS volumes
* Search engine: 'AWS EC2 EBS mount` gives [this instructive link](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html)
* `lsblk` produces a device listing
    * For this `m5ad.4xlarge` instance we see three devices `nvme0n1`, `nvme1n1`, `nvme2n1` of type `disk`
        * Full path is in fact `/dev/nvme0n1` and so on
            * Only the third volume `nvme2n1` has an associated partition
            * This is the root device where the operating system and ubuntu home directory resides
            * The remaining two disks `nvme0n1` and `nvme1n1` are the empty instance store volumes; must be mounted
        * Verify that there is no file system on the empty volumes
            * `sudo file -s /dev/nvme1n1` --> `/dev/nvme1n1: data` (The result `data` indicates nothing there.)
        * ***WARNING!!! The following command wipes a file system. Any data will be lost. ***
            * `sudo mkfs -t xfs /dev/nvme0n1` and likewise for `nvme1n1`
                * This should print out some confirmation stats
                * If `mkfs.xfs` is `not found`: Search on installing it using `sudo yum install xfsprogs`
            * `sudo file -s /dev/nvme0n1` should now show an XFS filesystem
            * Create a data directory for each disk
                * `sudo mkdir /data` and `sudo mkdir /data1`
                * `sudo mount /dev/nvme0n1 /data` and `sudo mount /dev/nvme1n1 /data1`
            * You will find that root owns these data directories. By default nobody else including the `ubuntu` user can write to them.
                * It is a security risk to make a data directory world-writable
                * The command to make a data directory world-writable is `sudo chmod a+rwx /data`
                * I use this without any qualms; but please be aware that it is a security-relevant choice
            * Test the data directories by `cd /data` and creating a new file

#### Set up these storage disks to auto-mount on reboot

* Backup copy: `sudo cp /etc/fstab /etc/fstab.orig`
* Get the UUID: `sudo lsblk -o +UUID` 
* Edit the `fstab` file: `sudo vim /etc/fstab`
* Add this entry to the file: `UUID=aebf131c-6957-451e-8d34-ec978d9581ae  /data  xfs  defaults,nofail  0  2`
* Test it out: 
    * `sudo umount /data`
    * `sudo mount -a`
* Refer to the link given above for more detail on this procedure. A broken `fstab` file can prevent the VM from booting.

   
#### Install the Jupyter Lab notebook serve
* Install Anaconda
    * search `install Anaconda Linux` and follow the instructions
    * Once the download path is determined I used `wget` to download the installer on the VM
        * `wget https://repo.anaconda.com/archive/Anaconda3-2020.07-Linux-x86_64.sh`
    * On Ubuntu I installed in the `/home/ubuntu` directory and needed to add the path for `conda`
        * `export PATH=~/anaconda3/bin:$PATH`
    * Once Anaconda is installed: use the `conda` package manager to install Jupyterlab
    * `conda install -c conda-forge jupyterlab`
* Test this using the `ssh tunnel` described in the main page tutorial of this repository

#### Configure the machine for research

* Install software packages
    * The following are installation commands by way of example (geoscience-oriented)
        * `conda install xarray`
        * `conda install -c conda-forge cmocean`
        * `conda install boto` was apparently already done...
        * `conda install netcdf4`
        * `conda install -c conda-forge ffmpeg`
        * `conda install networkx`
        * `pip install git+https://github.com/cormorack/yodapy.git`
        * `pip install utm`
        * `pip3 install manimlib` is Grant Sanderson's explanatory math visualization library
        * `sudo apt install nodejs` working towards widgets...
    * Import datasets, typically to data volumes
        * Example approach: Use `sftp -r` from the data directory of a source computer
    * Imported code repositories (for example from GitHub) into the ubuntu user home directory
    * Generate `requirements.txt` or `environment.yml`; look into `pip freeze` for example

#### Create an image (AMI) of this Virtual Machine
* On the AWS console: EC2 'running instances table: Locate and select this instance
    * Actions menu > Image > Create Image
    * Be sure to attach extensive metadata (typically add Tags) to the AMI to make it recognizable

## Share the AMI with other AWS accounts
    
* On the AWS console > AMI listing (see left sidebar) > Permissions editor
* Add the AWS 12-digit account of a destination account where you wish this AMI to be available

## Terminate the VM so as not to continue paying for it

* ***WARNING: This will completely delete this EC2 instance. To mitigate concern...***
    * Consider starting a new EC2 instance using the AMI you created above.
    * You can verify everything is preserved as you expect in this new instance
    * At this point the AMI has been demonstrated as correct and you can Terminate both EC2 instances
* On the AWS console > EC2 'Running Instances' table: Locate and select an instance to Terminate
* Actions menu > Instance State > Terminate and confirm


## Updating Anaconda and refreshing the machine image

* Once per month is a common Anaconda update tempo
* Consider updating the operating system as well, for example using `sudo yum update`
* Upon doing updates: Create a new AMI; and manage your AMI catalog to avoid zombie charges


## Remaining open topics

* Linking to M's content
* Do not re-start a running Jupyterlab server. Check for it with `ps -ef | grep jupyter` is helpful.
    * If the local machine disconnects it is usually sufficient to re-issue the ssh tunnel command from your computer
* Enabling widgets is a confusing detail...

```
jupyter nbextension enable --py widgetsnbextension
```

* Making mpeg videos from chart sequences
   
   
# CloudBank Solution Repo: Research Computing and Cloud Images

This tutorial introduces you, the researcher or student, to using a virtual machine ***image*** 
on the cloud as a basis for research computing. This page describes using an image
that has *already been built for you*. If you want to see how to do the preparatory
build: Look into the `creating_an_image` sub-folder. 



> We ***strongly encourage*** you to read the section on idiosyncratic cloud matters 
that follows the main 'how to'. 


You may be familiar with a *zip* or *tar* file containing an entire directory. 
A *machine image* is analogous; think of it as a zip file of the entire computer's contents
from operating system to home directory to code to data files. The idea is that once 
a cloud Virtual Machine (VM) is *configured* for use: It can be stored as a machine 
image. This image is then un-packed back onto a Virtual Machine. The image is used
to creating a working research computing environment for a scientist to use. 


Our specific example here takes advantage of the Jupyterlab notebook server. This page
is a tutorial for going from a pre-built Linux environment with Jupyterlab installed 
as an *image* and reconstituting it on the AWS cloud as a Virtual Machine. We will use
a secure tunnel called an `ssh` tunnel to enable you the researcher to connect to the
Jupyterlab server through your web browser. 


In the sub-folder called `bootstrap` we go through the process of building this machine image. 

In the sub-folder called `waterhackweek` we go from the rebuilt Jupyterlab VM to cloning a 
repository of notebooks on the VM for a particular research topic. 


## Important remark on cost management


In what follows on this page: The VM has a single disk drive (filesystem) with a fairly small 
capacity of 32 Gigabytes. However it is common practice to create images that bundle large
datasets. The sub-folder `bootstrap` tutorial spins up a Virtual Machine, for example, that has
200 GB of block storage (disk drive) volume mounted as two data filesystems. 
This allows the image *builder* to include a moderately large dataset with the image 
and provide some working space as well. 


Our point of emphasis here -- as with all cloud resource allocation actions -- is that one 
should understand and track the costs associated with cloud resources. 'Block' or 'disk' 
storage -- for example -- runs about $20 per month for 200 GB.


> Again our main admonition: The cloud is very powerful but it is
important to understand and manage the cost of using it. 


## Outline


- In what follows please customize your resources by substituting your own name for `hedylamarr`.
- Obtain credentials to log in to the cloud console: Contact cloudbank for details
- Identify the proper *image* and use it to jumpstart a Virtual Machine *instance*
- Log on to this *instance* and start up a Jupyter Lab server
- Create an **ssh** tunnel from your computer to this server
- Use your browser to connect to the Jupyter Lab and use this to explore some data


## For Windows Users


A brief interruption anticipating a possible issue: Further down we are going to build 
something called an "ssh tunnel" to use our Virtual Machine as a Jupyter notebook server. 
If you are doing this on a PC running Windows: No problem, that's perfectly feasible but
Windows does not natively make this Linux-y step trivial.  So be ready: We will introduce
the idea of installing a small Linux bash shell on the Windows PC. It is a bit of a 
"yet another step, really??" situation but on balance it can help save time and avoid frustration. 
[Instructions are here](https://ubuntu.com/tutorials/tutorial-ubuntu-on-windows#1-overview).


## Connecting to a Jupyterlab server built on a cloud Virtual Machine


Suppose we would like to visually explore some (ocean) data. This data took years to collect and 
months to bring together in one location. Hopefully it takes less than an hour to
deploy and connect to a Jupyterlab notebook server on the cloud. 


> Prerequisites: Cloudbank credentials to connect to the cloud and an available `bash` shell.


We are using in this case the AWS (Amazon Web Services) cloud. You will log on to the AWS 
console, start a Virtual Machine (called an EC2 *instance*) and on that machine start a 
Jupyterlab server. If you were starting from nothing you would be installing Python 
packages and importing datasets.  Our objective here is to avoid 
all of that by using a pre-built environment stored on the cloud as an *image*. 
Once you have identified this image you can start it on virtually any size machine; 
from a small cheap one say costing $0.04 per hour to a very powerful computer that 
might cost $2.40 / hour or more. Cloud users choose a computer based on computational needs.


Once the computer is running (with everything pre-installed) you will create 
an *ssh tunnel*. This is a secure connection that associates a local address on your
local comoputer with the Jupyterlab 
server running on the cloud VM. By connecting through this tunnel the cloud VM 
becomes the backing engine for exploring the data. 


The procedure is presented in 13-or-so steps with interspersed comments.
Upon completion you will have your own data science research environment. 


### Procedure


1. Log on to the AWS console using your credentials; and be sure to set your Region (upper right drop-down) to Oregon
    o Sidebar: You can choose Services (upper left) to see a listing of services, i.e. what you can do on the AWS cloud


2. Navigate to Amazon Machine Image choices (AMIs): Services > Compute > EC2. Then choose AMIs from the left sidebar. 


3. In the upper left drop down menu of the AMI pane select 'Private Images' (it may say 'Owned by Me' or 'Public Images' by default). When 'Private Images' is selected, you should see an AMI listed called `jupyter1-cb`. Select this AMI by clicking on it so a blue dot appears at the left edge of the table.


4. Choose Actions > Launch. Choose a VM type `c5.large` (you may have to scroll down a bit to find this). Choose Review and Launch at the bottom right.


> WARNING: If this tutorial is a class-sized activity there is a potential collision scenario. Let's 
take a moment to outline this and how to resolve it.


> The AWS EC2 Launch Wizard goes through seven steps. Step 6 involves choosing a 
Security Group. This SG is given a name by default; and a classroom full of people will get
the same default as they proceed. So the way to avoid this (which will obstruct the next 
couple of steps) is to click on the Security Group step (step 6) and give a name for the 
Security Group that is unique. As below with keypair and instance names the best choice 
for a Security Group name is simply `yourname`. In our instructions we use the name
`hedylamarr` as an example of your name. Now you can proceed to step 7 of the wizard;
which is step 10 in this procedural. 


5. Choose **Launch** at the bottom right. In the ensuing keypair dialog choose **Create new keypair** 
and name it `hedylamarr`. This will be the key to identifying your instance and logging in to
it in what follows. Download the keypair file you generated; then click **Launch Instance**
in the dialog box.
 

6. Continue in the AWS console: Click **View Instances** at the lower right. This takes 
you to a table of EC2 instances (Virtual Machines), where one instance is listed per row. 
Therefore one of the rows in this table should be your instance.


> Class strategy remark: You now have a Virtual Machine ("EC2 instance") on the public cloud. If you are in
a class with many people doing the same thing at the same time it can be difficult to identify
which instance is *your* VM. Once you identify your instance: Name it using `yourname`.


7. Locate your instance by the key name: In the table of instances scroll right to find the 
`Key name` column. Scan down this column for the keypair you used to identify the row for your EC2 instance.
Return to the left-most **Name** column of this row -- the row for your instance -- and hover your cursor to bring up 
a pencil icon in the "Name" column. Click on that and name your instance appropriately.


8. One the instance status dot changes to green (running): Note its `ip address` in the instance table.
Here we take this to be `12.23.34.45` as an example. 


> Now we are ready to log in to this VM or EC2 instance from a bash shell. Our main goal on the
instance is to start a 'quiet' Jupyterlab session. There will not initially be a browser interface; 
that comes a little further down. Once the Jupyterlab server starts it will print a *token*, a long 
string of letters and numbers used to authenticate; so be prepared to copy that to a text editor
for a few minutes. 
 

9. On your computer: Open a `bash` shell and ensure the keypair file `hedylamarr.pem` is present in your working directory.
Issue a `chmod` command to give the keypair file limited `rwx` permissions: `chmod 400 hedylamarr.pem`.


> If running **Windows** on your local computer: You may need to install or enable the native `bash` shell. 
As an alternative you can install an **Ubuntu** `bash` shell. In either case it is useful to realize that
the *home directoy* of this shell is not the same location as the Windows User home directory. 
If `hedylamarr.pem` was downloaded to `C:\\Users\hedylamarr\Downloads`:  Move it to your `bash` home 
directory, for example using this sequence of commands: 


```
bash> mv /mnt/c/Users/hedylamarr/Downloads/hedylamarr.pem ~
bash> cd ~
bash> chmod 400 hedylamarr.pem
```


> The `ssh` command insists the authentication keypair file `hedylamarr.pem` have User read-only permission,
Files in the Windows User area are not amenable to `chmod`; so we relocate this file and proceed in `bash` from `~`.
These details can very frustrating so we go to the trouble to elaborate this here.

 

10. In bash run `ssh -i hedylamarr.pem ubuntu@12.23.34.45` to connect to your EC2 instance. (Use the correct ip address.)
Respond `yes` at the prompt to complete the login. Note that your username is `ubuntu` and you have the ability to run
root commands using `sudo`. 


> If you have configured the image using (for example) the tutorial in the `bootstrap` sub-folder: You may wish 
to spend some time looking around to ensure the instance is configured as expected. For example if there are 
additional data disks you can check to see these have been mounted properly. 


11. On the command line of your EC2 instance issue `(jupyter lab --no-browser --port=8889) &`
After about one minute this command should produce multiple lines of output including a 
`token string`.

 
```
...token=ae948dc6923848982349fbc48a2938d4958f23409eea427
...token=ae948dc6923848982349fbc48a2938d4958f23409eea427
```


Copy this string to a text editor for later use. 
If you lose this token just log on to the instance where it is running and issue `jupyter notebook list` to see it.
Issue `exit` to log out of your cloud instance.


> Note: The Linux structure `(command) &` causes `command` to run as a background job. 
This allows you to log out of your instance, leaving it to function as a Jupyterlab server.


> Note: The above token is used the first time connecting to the Jupyterlab server. It may not
be used in subsequent sessions but it is worth keeping it around. If it is lost for some reason:
Re-start the Jupyterlab server on the VM and re-copy the token. 


12. In the `bash` shell issue `ssh -N -f -i hedylamarr.pem -L localhost:7005:localhost:8889 ubuntu@12.23.34.45`. 
(Make appropriate substitutions.)


> This `ssh` command creates an ssh tunnel to the EC2 instance running the Jupyterlab server. 
You associate a location on your own computer (localhost:7005) with the connection point on the EC2 instance 
(12.23.34.45:8889). The number trailing the colon, called a port, acts as a qualifier for the connection.


13. In your browser address bar type `localhost:7005`. This should change to `localhost:7005/lab?`.
When prompted for the token paste in the token string you copied earlier. You should now see a 
Jupyterlab environment. When using the instructional image for this tutorial there should be 
a notebook listed on the left called `chlorophyll.ipynb`. You may click on this notebook to open it
and explore the contents. 


## Idiosyncratic Cloud Matters

This section covers some details of the main program beyond the basics given above.  

- You may need to 
[mount storage volumes](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html)
on your EC2 VM as file systems in addition to 
your root file system. This is commonly done for managing large datasets for example. 
See the [sub-folder **bootstrap** in this repo](https://github.com/cloudbank-project/image-research-computing-tutorial/tree/master/bootstrap)
for more.
    - You may choose to attach Elastic Block Storage (EBS) volumes which can be understood as 
'additional persistent disk drives' (above the capacity of your root drive).  You may 
need to mount those drives to make them usable.
    - Your EC2 VM may also include *instance store volumes*. These may be understood as 
'addtional ephemeral (temporary) disk drives'. That is: They evaporate when the instance stops. 
It is therefore unwise to put anything on these volumes that you expect to access in the future. 
- Updating Anaconda and the machine image operating system
    - Once per month is a common Anaconda update tempo...
    - See this comment in full on the `bootstrap` README
- Locking down the ip address of the instance
    - AWS provides a (not-unlimited) number of dedicated ip addresses
        - These allow you to use the same ip address despite stopping and re-starting your instance
        - To use follow these steps
            - Use the left sidebar to navigate to the Elastic IP page of the AWS console
            - Allocate an Elastic IP address
            - Associate this IP address with your running instance

## Using Jupyterlab


There are comprehensive guides to using Jupyterlab. Here we include some minimal notes.


* A Jupyter Lab environment has a left, narrow panel and a primary, larged notebook panel
    * Notebooks are listed in the left panel
    * Double-click them to open them in the main panel
    * In our example the primary notebook of interest is called `chlorophyll.ipynb`
* A Jupyter Lab *notebook* such as `chlorophyll.ipynb` consists of a sequence of cells.
    * Each cell contains some text
    * For our purposes each cell is either a *Python code* cell or a *markdown* cell
        * Python code executes (or tries to execute) when the cell runs
        * Markdown renders as formatted readable text -- like a narrative -- when the cell is run
* How to run Jupyter Lab notebook cells
    * Notice that a cell is selected when it has a vertical blue bar at its left border
    * Select a cell by clicking on it with the cursor; or by using up/down arrows 
    * Run a cell using Shift-Enter (this advances the selected cell to the next cell down)
    * Run a cell using Ctrl-Enter (this does not advance the selected cell)
    * Run a cell using Alt-Enter (this runs the cell and opens a new empty cell below it)
* Modifying cells
    * Double-click on a cell to open it as a text editor
    * Edit the text to change what the cell does; and then run it to see the results
* The *kernel* is the program that executes the Python code
    * The kernel status is indicated by a circle at the upper right of the notebook
    * Hover over the circle with your cursor for elaboration
