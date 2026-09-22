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
>If given enough energy I am going to try to explain how to setup a virtual machine both for Windows and Linux. The problem that I have regarding this is that I do not use windows anymore so I may miss some details.
> I recommend the following Virtual Machines to be used:
> - For Windows: Oracle Virtual Box
> - For Linux: QEMO/KVM with Virt-Manager GUI
>   
> You can research tutorials anyway I do not think they are that difficult to configure.

Assuming you have already set up your Unbutu let us now have the step by step process (according to the guide book) on how to install the required middleware.

## Client URL
curl (which stands for "Client URL") is a command-line tool used to transfer data to or from a network server using one of its dozens of supported protocols (like HTTP, HTTPS, FTP, and SFTP). Essentially, it lets you interact with websites, APIs, and servers directly from your terminal without needing a web browser.

To install in Ubuntu
```bash
sudo apt install curl -y
```
> [!NOTE]
> -y is used to skip the confirmation question in installations
> 
## Installing ROS and Gazebo
According to the manual the Int-Ball2 Technology Demonstration Platform uses ROS and Gazebo, and it is necessary to use a Gazebo version higher than version 9.0.0. 
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
And it should output
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

Run Gazebo
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

2. Setup sources.list
```bash
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```

3. Setup Keys
```bash
curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | sudo apt-key add -
```

4. Install ROS

Update debian
```bash
sudo apt update
```

There are 3 main installs but it is recommended to use the full desktop for a more comfortable configuration.
```bash
sudo apt install ros-melodic-desktop-full
```
>[!NOTE]
>Explore the given website link to explore the other 2.

5. Add ROS environment variables to bash session every time a new shell is launched
```bash
echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```
>[!NOTE]
> If you are using more than one ROS distribution, changing the environment, or want  to change to zsh instead of bash script check the given website link. As of now we will be using the default for our set up.

6. Additional Dependencies for Building Packages
```bash
sudo apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential
```

Now you need to initialize rosdep.

First install rosdep
```bash
sudo apt install python-rosdep
```
Initialize rosdep
```bash
sudo rosdep init
rosdep update
```
Check if ROS is working 
```bash
source /opt/ros/melodic/setup.bash
roscore
```

If it outputs
```bash
started core service [/rosout]
```
That means ros is working now you can Ctrl+C to stop it.

**Installing ROS related packages in collaboration with Gazebo**
```bash
sudo apt install ros-melodic-gazebo-*
```

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
We will follow what the manual has instructed.

```bash
pip3 install rospkg -y
```
and 
```bash
pip3 install empy -y
```
>[!NOTE]
>In the manual it was stated that empy had a bug where you need to specify the version to properly install it. I have tried it in the terminal and I believe that the bug has been fixed, thus we do not need to specify the version here.

## Python3 Version for ROS Package (catkin)
Navigate to 
```bash
/opt/ros/melodic/etc/catkin/profile.d/
```
Use any code editor (I recommend neovim for linux) 

In the manual to edit the shell script
```bash
sudo vi 1.ros_python_version.sh
```
When using nvim 
```bash
sudo nvim 1.ros_python_version.sh
```
If you are unfamiliar with these text editors you can use `nano`
```bash
sudo nano 1.ros_python_version.sh
```

In the script you will see

```shell
# generated from ros_environment/env-hooks/1.ros_python_version.sh.in

export ROS_PYTHON_VERSION=2
```

Change it to


```shell
# generated from ros_environment/env-hooks/1.ros_python_version.sh.in

export ROS_PYTHON_VERSION=3
```
## Ground Support Equipment
The Int-Ball2 Ground Support Equipment shall be built and installed through the following.
### Newtide Assembler (NASM)

Netwide Assembler (NASM) is a popular open-source software tool that translates assembly language code into machine code for Intel x86 and x86-64 microprocessors

1. Download `nasm` Source File

Navigate to `/usr/local/src`
```bash
cd /usr/local/src
```
```bash
sudo wget https://www.nasm.us/pub/nasm/releasebuilds/2.15.05/nasm-2.15.05.tar.gz
```
and extract
```bash
sudo tar zxvf nasm-2.15.05.tar.gz
```

2. Configure NASM

Navigate to `nasm-2.15.05`
```bash
cd nasm-2.15.05
```
Begin configuration
```bash
sudo ./configure
```

3. Build and Install the Source File
```bash
sudo make install
```

### Video Reception Environment

##### The download links from this section is from https://trans-it.net/blog/centos7-ffmpeg43-h264-fdkaac/ note that this blog is in Japanese please use translation for better comprehension of each step.

Here we will be installing `x264`

1. Download `x264-master` Source File
Navigate back to `/usr/local/src`
```bash
cd /usr/local/src
```

Download the file
```bash
sudo wget https://code.videolan.org/videolan/x264/-/archive/master/x264-master.tar.gz
```

Decompress the file
```bash
sudo tar xvf x264-master.tar.gz
```

2. Configure x246-master

Navigate to `x264-master`
```bash
cd x264-master
```

Begin configuration
```bash
 ./configure \
--disable-asm \
--enable-shared \
--enable-static \
--enable-pic 
```

3. Build and Install the Source File
```bash
sudo make install
```

### Installing FFMPEG

1. Download `ffmpeg` Source File
   
Navigate back to `/usr/local/src`
```bash
cd /usr/local/src
```

Download the file
```bash
sudo wget https://ffmpeg.org/releases/ffmpeg-4.1.3.tar.gz
```

>[!NOTE]
>In the website link for the downloads the version for ffmpeg is at 4.3 but to follow the manual we will change it to version 4.1.3.

Decompress the file
```bash
sudo tar xvzf ffmpeg-4.3.tar.gz
```

2. Configure the File

Navigate to `ffmpeg-4.1.3`
```bash
cd ffmpeg-4.1.3
```

Configure the File
```bash
sudo ./configure \
--extra-cflags="-I/usr/local/include" \
--extra-ldflags="-L/usr/local/lib" \
--extra-libs="-lpthread -lm -ldl -lpng" \
--enable-pic \
--disable-programs \
--enable-shared \
--enable-gpl \
--enable-libx264 \
--enable-encoder=png \
--enable-version3
```

3. Build and Install the Source File
```bash
sudo make install
```

>[!NOTE]
> When checking for the ffmpeg it will result in `Command 'ffmpeg' not found` this is okay since we are only using ffmpeg libraries not the ffmpeg command itself.

### Install VLC Media Player

1. Install Dependencies
   
Navigate back to `/usr/local/src`
```bash
cd /usr/local/src
```
Install the dependency packages
```bash
sudo apt install libasound2-dev libxcb-shm0-dev libxcb-xv0-dev \
libxcb-keysyms1-dev libxcb-randr0-dev libxcb-composite0-dev \
lua5.2 lua5.2-dev protobuf-compiler bison libdvbpsi-dev libpulse-dev
```

2. Download `VLC Media Player` Source File

Install Source
```bash
sudo wget https://download.videolan.org/pub/videolan/vlc/3.0.7.1/vlc-3.0.7.1.tar.xz
```

>[!NOTE]
> The manual did not provided any source for `VLC Media Player` source file, I HATE IT, so I found one online.

Decompress the File
```bash
sudo tar Jxvf vlc-3.0.7.1.tar.xz
```

2. Configure the File

Navigate to `vlc-3.0.7.1`
```bash
cd vlc-3.0.7.1
```
Configure with
```bash
CFLAGS="-I/usr/local/include" \
LDFLAGS="-L/usr/local/lib" \
X264_CFLAGS="-L/usr/local/lib -I/usr/local/include" \
X264_LIBS="-lx264" \
X26410b_CFLAGS="-L/usr/local/lib -I/usr/local/include" \
X26410b_LIBS="-lx264" \
AVCODEC_CFLAGS="-L/usr/local/lib -I/usr/local/include" \
AVCODEC_LIBS="-lavformat -lavcodec -lavutil" \
AVFORMAT_CFLAGS="-L/usr/local/lib -I/usr/local/include" \
AVFORMAT_LIBS="-lavformat -lavcodec -lavutil" \
sudo ./configure \
--disable-a52 \
--enable-merge-ffmpeg \
--enable-x264 \
--enable-x26410b \
--enable-dvbpsi
```

3. Build and Install the Source File
```bash
sudo make install
```

>[!IMPORTANT]
>When confirming `vlc` with `vlc -version` I encountered a problem here
>```bash
>ian@ian-Standard-PC-Q35-ICH9-2009:/usr/local/src/vlc-3.0.7.1$ vlc -version
>vlc: error while loading shared libraries: libvlc.so.5: cannot open shared object file: No such file or directory
>```
>This means that `vlc` is installed, proven by using `whicg vlc`
>```bash
>ian@ian-Standard-PC-Q35-ICH9-2009:/usr/local/src/vlc-3.0.7.1$ which vlc
>/usr/local/bin/vlc
>```
>But Ubuntu doesn't know where to find its shared libraries.
>
>To fix this we need to tell Ubuntu where to find the `vlc` files
>
>Check where `libvlc.so.5` is
>```bash
>find /usr/local -name "libvlc.so*"
>```
>Result should be something like
>```bash
>/usr/local/lib/libvlc.so
>/usr/local/lib/libvlc.so.5.6.0
>/usr/local/lib/libvlc.so.5
>/usr/local/src/vlc-3.0.7.1/lib/.libs/libvlc.so
>/usr/local/src/vlc-3.0.7.1/lib/.libs/libvlc.so.5.6.0
>/usr/local/src/vlc-3.0.7.1/lib/.libs/libvlc.so.5.6.0T
>/usr/local/src/vlc-3.0.7.1/lib/.libs/libvlc.so.5
>```
>
>Tell Ubuntu where `/usr/lacal/lib` is
>```bash
>echo "/usr/local/lib" | sudo tee /etc/ld.so.conf.d/local.conf
>```
>and 
>```bash
>sudo ldconfig
>```
>Now confirm it with
>```bash
>vlc -version
>```
>I have fixed my problem with this. You can research online if this does not fix your problem.

4. Prepare the Symbolic Link to the Downloaded File Source
```bash
sudo ln -s /usr/local/src/vlc-3.0.7.1 /usr/local/src/vlc
```
Confirm with
```bash
ls -l /usr/local/src/vlc
```

### Installing Qt

1. Prepare the Installing Directory
Navigate to `/opt/`
```bash
cd /opt
```
Create the installing directory
```bash
mkdir Qt
```
2. Download the `Qt Installer`
Navigate to the downloads folder
```bash
cd ~/Downloads
```
Download the source file
```bash
sudo wget https://download.qt.io/archive/qt/5.12/5.12.8/qt-opensource-linux-x64-5.12.8.run
```
Make it executable
```bash
sudo chmod +x qt-opensource-linux-x64-5.12.8.run
```
Run the installer
```bash
./qt-opensource-linux-x64-5.12.8.run
```
3. Configuring the installer

<br>1. After running the installer you will see the following window<br>

<img width="1072" height="658" alt="image" src="https://github.com/user-attachments/assets/6d0671cb-be97-41d4-93fd-e3e0bccbc543" />

<br>2. Login or Sign-up an account<br>

<img width="1070" height="656" alt="image" src="https://github.com/user-attachments/assets/a8a72480-b070-4d2a-83a3-f580aed8b051" />

<br>3. It will ask for email verification<br>

<img width="1065" height="654" alt="image" src="https://github.com/user-attachments/assets/ec99febb-1237-43a4-8099-76df24241b6e" />


<br>4. Verify your email<br>


<img width="1448" height="780" alt="image" src="https://github.com/user-attachments/assets/c854ed88-283b-4877-a909-050c71bd1117" />

<img width="628" height="780" alt="image" src="https://github.com/user-attachments/assets/a7cbf108-fd5a-4b05-9093-aa47ab3f1253" />

<br>5. Check the box of "I have read and approved the obligation of using Open Source Qt," <br>

<img width="1064" height="653" alt="image" src="https://github.com/user-attachments/assets/365b8392-ae26-442d-8684-016f6990333a" />


<br>6. Now you will begin the setup<br>
<img width="1073" height="661" alt="image" src="https://github.com/user-attachments/assets/02a1eed0-54ed-4af2-8964-2a2cf321c06c" />

<br>7. Set the directory as `/opt/Qt`
<img width="1069" height="654" alt="image" src="https://github.com/user-attachments/assets/c9b9936e-6a9f-479f-b62d-949e0933474a" />

<br>8. Select `5.12.3 Desktop gcc 64-bit` as the targeted version for installation<br>
<img width="1062" height="658" alt="image" src="https://github.com/user-attachments/assets/cf53b7b3-c850-470e-8aca-8cc771029209" />

<br>9. Agree to the terms and consitions<br>
<img width="1064" height="653" alt="image" src="https://github.com/user-attachments/assets/0cbac92f-3d1c-42b5-a94a-10a2f4166fad" />

<br> 10. Install <br>
<img width="1254" height="645" alt="image" src="https://github.com/user-attachments/assets/21539d51-87aa-4f4a-b8a9-4e97a20b5dcc" />

>[!IMPORTANT]
> I experienced an issue here where the installer said that the device does not have enough disk size. I solved this problem by increasing the partition used by the virtual machine.

<br>11. You have now successfully installed Qt<br>
<img width="1260" height="652" alt="image" src="https://github.com/user-attachments/assets/014ac15a-4adf-4ff6-bd48-e4c3e2057169" />

4. One line Execution
To execute `Qt` with `qtcreator` run
```bash
sudo ln -s /opt/Qt/Tools/QtCreator/bin/qtcreator /usr/local/bin/qtcreator
```
Now try
```bash
qtcreator
```

5. Prepare the Symbolic Link
```bash
sudo ln -s /opt/Qt/5.12.3 /opt/Qt/5
```

6. Setup Font






































































































































































































