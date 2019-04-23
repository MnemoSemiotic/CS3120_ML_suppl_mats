## [GOOGLE SLIDES](https://docs.google.com/presentation/d/1rZEN2YBXvk--e-LliGVryP1cPCMPWOoHTbaKHMz3aak/edit?usp=sharing)

## A. Setting up the Cloud Desktop

### 1. Install a VNC viewer:
* [RealVNC Viewer](https://www.realvnc.com/en/connect/download/viewer/)

### 2. Sign Up for Google Cloud
* [https://cloud.google.com/products/compute/](https://cloud.google.com/products/compute/)
* Click "Try it Free"
* You may need to sign in
* You will need to provide billing information
    * Keep in mind that you will have $300 free credit (as of 4/23/2019)


### 3. Create a new VM instance
* Notice that you are within a project, likely titled "My First Project"
* Under the navigation "hamburger" menu in the top left
    * Under **Compute Engine**
        * Select **VM instances**
* Click **Create**
    * Give the instance a good name
    * Choose a number of CPUs/amount of memory
* Set a value for the size of the persistent disk
    * Click "Change" under the default persistent disk
        *  We'll use Ubuntu 16.04 LTS, for ease of setting up the Desktop
    * Change the size of the Disk to meet your needs
    * Click "Select"
* Under the Firewall options
    * Allow HTTP traffic
    * Allow HTTPS traffic
* Click Create and wait for the machine to spin up

### 4. Connect to the machine
* Click on the triangle dropdown next to **SSH**
    * Select "Open in Browser Window"
    * This opens a browser-run ssh terminal directly to the machine


### 5. Setting up a GUI for VNC
* Install Ubuntu desktop (may take awhile)
    * select Yes at all prompts

```bash
~$ sudo apt-get update
~$ sudo apt-get install ubuntu-desktop
```

* Install dependencies for VNC connections

```bash
~$ sudo apt-get install gnome-panel gnome-settings-daemon metacity nautilus 
~$ gnome-terminal vnc4server
```

* Enable TCP ports
```bash
~$ sudo ufw allow 5901:5910/tcp
```

* Start the vncserver in order to set the password
    * this will be the password you pass in to connect using your vnc client

```bash
~$ vncserver
```
* Verify the VNC server is running and listening on port 5901
    * hit CTRL-D to exit netcat if you see the message `RFB 003.008`
        * if you don't see this message, back up and repeat this section

```bash
~$ nc localhost 5901
```

### 6. Edit the VNCServer startup script
* This will allow a nicer desktop environment through VNC

* kill the vncserver session

```bash
~$ vncserver -kill :1
```

* open the startup script to edit (we'll use vim here)

```bash
~$ vim .vnc/xstartup
```

> * Within vim, click the "i" key in order to start "INSERT" mode, which will allow you to enter text
> * Insert this text at the bottom of the document:

```vim
gnome-panel &
gnome-settings-daemon &
metacity &
nautilus &
```

> * To save the document
>   * click "escape"
>   * type `:w` to save the file
>   * type `:q` to quit the document

* make sure the vnc startup is executable

```
~$ sudo chmod +x ~/.vnc/xstartup
```

### 7. Create a Firewall Rule to allow VNC access
* Under the main hamburger menu, scroll down and click on **VPC network**
    * Click on **Firewall rules**
* Click **CREATE FIREWALL RULE** at the top menu
    * Set the name to `vnc-server`
    * Under target tags, add `vnc-server`
    * Under Source IP ranges, add `0.0.0.0/0`
    * Under Protocols and ports
        * click the checkbox next to *tcp* and add `5901` in the textbox
* Click the **Create** button to save the new rule

### 8. Add the new firewall rule to the vm instance
* Recall that the VM instance is under the main menu --> Compute Engine --> VM instances
* You can edit the instance settings by clicking on the instance name
    * Click the **EDIT** option in the top menu
    * Under `Network tags`, add `vnc-server` and hit ENTER
* Click SAVE at the bottom

### 9. Connect to the server using your VNC client
* start the vncserver in the ssh connection
    * note that you'll need to do this every time you start up your server, unless you script it to launch when you start the machine

```bash
~$ vnserver
```

* open your VNC client (We're using RealVNC Viewer here)
    * Enter the External IP that you see associated with the VM instance





## B. Installing Python (using Anaconda)

### 1. Install Anaconda
* Open Firefox under applications
* Download the Anaconda 64-bit command line installer for Linux at [https://www.anaconda.com/distribution/](https://www.anaconda.com/distribution/)
* Open the Downloads folder (or the folder where you downloaded the .sh installer script)
    * Right click in the folder and select "Open in Terminal"
    * You should see the install script here. Let's run it. You may need to change the filename, depending.

```bash
~$ bash Anaconda3-2019.03-Linux-x86_64.sh
```

* Scroll through the endless docs, use the default location, hit enter or type yes for all prompts

### 2. Change your Python version to 3.65 (to support current TensorFlow)
* close all terminals
* open a new terminal
* install the new python version

```bash
~$ conda install python=3.6.5
```
