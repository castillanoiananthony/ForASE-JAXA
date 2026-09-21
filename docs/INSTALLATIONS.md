# INSTALLING ROS AND GAZEBO 9 IN UBUNTU 
### ACCORDING TO THE "INT-BALL 2 TECHNOLOGY DEMONSTRATION USER PROGRAMMING PLATFORM USER'S MANUAL"

##### Download the Ubuntu iso: https://releases.ubuntu.com/18.04.6/

Hi from Ian Anthony Castillano
I wanna share my sweet struggles in installing ros and gazebo in Ubuntu.
<br><br>
According to the instruction manual the INT-BALL2 have the following operation environment:
- OS: Ubuntu 18.04
- Middleware: Robot Operating System (ROS/Melodic)
- Body Processor: Nvidia Jetson TX2, Linux for Tegra (Ubuntu Based File System)

>[!NOTE]
>I am soon gonna try and explain how to setup a virtual machine both for Windows and Linux. The problem that I have regarding this is that I do not use windows anymore so I may miss some details.
> I recommend the following Virtual Machines to be used:
> - For Windows: Oracle Virtual Box
> - For Linux: QEMO/KVM with Virt-Manager GUI
>   
> You can research tutorials anyway I do not think they are that difficult to configure.

Assuming you have already set up your Unbutu let us now have the step by step process (according to the guide book) on how to install the required middleware.

## Client URL
curl (which stands for "Client URL") is a command-line tool used to transfer data to or from a network server using one of its dozens of supported protocols (like HTTP, HTTPS, FTP, and SFTP).Essentially, it lets you interact with websites, APIs, and servers directly from your terminal without needing a web browser.

To install in ubuntu
```bash
sudo apt install curl -y
```
> [!NOTE]
> -y in used to skip the confirmation question in installations
> 
## Installing ROS and Gazebo
According to the manual the Int-Ball2 Technology Demonstration Platform uses ROS and Gazebo, and it is necessary to use a Gazebo version higher than version 9.0.9. 
So the following should be done for the installation.

**Installing Gazebo**

##### Here is the guide for Gazebo installation (currently at 2026 it is Gazebo version 11): https://classic.gazebosim.org/tutorials?tut=install_ubuntu
>[!WARNING]
> DO NOT USE the one liner installation it is deprecated.

Instead follow the step-by-step alternative installation.

1. Setup your computer to accept software from packages.osrfoundation.org.
```bash
sudo sh -c 'echo "deb http://packages.osrfoundation.org/gazebo/ubuntu-stable `lsb_release -cs` main" > /etc/apt/sources.list.d/gazebo-stable.list'
```
To confirm, run
```bash
cat /etc/apt/sources.list.d/gazebo-stable.list
```
and it should output
```bash
deb http://packages.osrfoundation.org/gazebo/ubuntu-stable bionic main
```
2. Setup keys
```bash
wget https://packages.osrfoundation.org/gazebo.key -O - | sudo apt-key add -
```
3. Install Gazebo
>[!NOTE]
> Always update debian before package installation.

To update debian, run
```bash
sudo apt update
```
Now install Gazebo
```bash
sudo apt install gazebo9
```
>[!NOTE]
>Although in the website the gazebo installation version is 11 we will be using version 9 for compatibility of the existing Int-Bot2 software.

Now you can run Gazebo
```bash
gazebo
```

**Installing ROS**
##### Here is the guide for installing ROS: https://wiki.ros.org/melodic/Installation/Ubuntu

1. Configure  to allow main, restricted, universe, and multiverse repositories.
```bash
sudo add-apt-repository main
sudo add-apt-repository universe
sudo add-apt-repository restricted
sudo add-apt-repository multiverse
```
2. 
## Installing Python in Ubuntu 

In the manual the instruction is to do
```bash
sudo apt install python3 python3-pip
```
Or like what I did you can install separately for safe install
```bash
sudo apt instal python3 -y
```
and 
```bash
sudo apt install python3-pip -y
```

> [!NOTE]
>  pip is the standard package manager for Python. It is a command-line tool that allows you to download, install, update, and manage extra libraries and dependencies that are not included in the standard Python installation.

## Installing ROS Related Packages of Python

