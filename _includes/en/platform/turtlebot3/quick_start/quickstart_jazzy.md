{::options parse_block_html="true" /}
# Quick Start Guide

<iframe width="560" height="315" src="https://www.youtube.com/embed/2I_29m_Z3WA" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## [PC Setup](#pc-setup)

**WARNING**: The content in this chapter is for the initialization of the `Remote PC` (your desktop or laptop PC) which will be used to control the TurtleBot3. Do not complete these instructions on the TurtleBot3 platform itself.
{: .notice--danger}

{% capture warning_01 %}
**Compatibility WARNING**  
- The `Jetson Nano` does not support native Ubuntu 20.04. Please refer to the [NVIDIA developer forum]{: .blank} for more details.

[NVIDIA developer forum]: https://forums.developer.nvidia.com/t/when-will-jetpack-move-to-ubuntu-20-04/142517
{% endcapture %}
<div class="notice--danger">{{ warning_01 | markdownify }}</div>

**NOTE**: This instruction was tested on the `Ubuntu 24.04` linux distribution running `ROS 2 Jazzy Jalisco`.
{: .notice--info}

### [Download and Install Ubuntu on Remote PC](#download-and-install-ubuntu-on-pc)

1. Download the `Ubuntu 24.04 LTS Desktop` image for your PC from the link below.
  - [Ubuntu 24.04 LTS Desktop image (64-bit)](https://releases.ubuntu.com/noble/){: .blank}

2. Follow the instructions below to install Ubuntu.
  - [Install Ubuntu desktop](https://ubuntu.com/tutorials/install-ubuntu-desktop#1-overview)


### [Install ROS 2 on Remote PC](#install-ros-2-on-remote-pc)

Please follow [the official ROS 2 documentation](https://docs.ros.org/en/jazzy/Installation.html) to install ROS 2 Jazzy.  
  
For most Linux users, the [Debian package installation method](https://docs.ros.org/en/jazzy/Installation/Ubuntu-Install-Debians.html) is strongly recommended.  
  

<details>
<summary>

![](/assets/images/icon_unfold.png) **Click here to expand more details about How to install ROS 2.**
</summary>
  
1. Visit the [Debian package installation](https://docs.ros.org/en/jazzy/Installation/Ubuntu-Install-Debs.html) page.  
  
2. Copy the CLI commands located in the green box and paste into your terminal with `ctrl` + `shift` + `v`  
  ![](/assets/images/platform/turtlebot3/quick_start/quick_start/ros2_install1-.png)    
  
3. Generally, ros-jazzy-desktop is recommended for the `Remote PC`  
  ![](/assets/images/platform/turtlebot3/quick_start/quick_start/ros2_install2-.png)  
  
4. Add a line sourcing your environment to your bashrc.
<br>
**[Remote PC]**  
```bash
echo "source /opt/ros/jazzy/setup.bash" >> ~/.bashrc  
source ~/.bashrc  
```
  
</details>  




### [Install Dependent ROS 2 Packages](#install-dependent-ros-2-packages)

1. Open the terminal with `Ctrl`+`Alt`+`T` on the **Remote PC**.
2. Install Gazebo Sim  
**[Remote PC]**  
  ```bash
$ sudo apt-get update
$ sudo apt-get install curl lsb-release gnupg
$ sudo curl https://packages.osrfoundation.org/gazebo.gpg --output /usr/share/keyrings/pkgs-osrf-archive-keyring.gpg
$ echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/pkgs-osrf-archive-keyring.gpg] http://packages.osrfoundation.org/gazebo/ubuntu-stable $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/gazebo-stable.list > /dev/null
$ sudo apt-get update
$ sudo apt-get install gz-harmonic
  ```
3. Install Cartographer  
**[Remote PC]**  
  ```bash
$ sudo apt install ros-jazzy-cartographer
$ sudo apt install ros-jazzy-cartographer-ros
  ```
4. Install Navigation2  
**[Remote PC]**  
  ```bash
$ sudo apt install ros-jazzy-navigation2
$ sudo apt install ros-jazzy-nav2-bringup
  ```

### [Install TurtleBot3 Packages](#install-turtlebot3-packages)

Install the required TurtleBot3 Packages.  

**[Remote PC]**  
```bash  
$ source /opt/ros/jazzy/setup.bash
$ mkdir -p ~/turtlebot3_ws/src
$ cd ~/turtlebot3_ws/src/
$ git clone -b jazzy https://github.com/ROBOTIS-GIT/DynamixelSDK.git
$ git clone -b jazzy https://github.com/ROBOTIS-GIT/turtlebot3_msgs.git
$ git clone -b jazzy https://github.com/ROBOTIS-GIT/turtlebot3.git
$ sudo apt install python3-colcon-common-extensions
$ cd ~/turtlebot3_ws
$ colcon build --symlink-install
$ echo 'source ~/turtlebot3_ws/install/setup.bash' >> ~/.bashrc
$ source ~/.bashrc
```

### [Environment Configuration](#environment-configuration)

1. Setup your ROS environment for the Remote PC.  
**[Remote PC]**  
  ```bash
$ echo 'export ROS_DOMAIN_ID=30 #TURTLEBOT3' >> ~/.bashrc
$ echo 'source /opt/ros/jazzy/setup.bash' >> ~/.bashrc
$ source ~/.bashrc
  ```
