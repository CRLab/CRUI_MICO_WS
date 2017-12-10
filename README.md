# CRUI_MICO_WS
The new MICO workspace that utilizes robowebtools

## Dependencies
In order to install this system, you'll need a series of dependencies on your system.
1. gitman (https://github.com/jacebrowning/gitman)[https://github.com/jacebrowning/gitman]
2. cuda (https://developer.nvidia.com/cuda-downloads)[https://developer.nvidia.com/cuda-downloads]
3. libfreenect2 (https://github.com/OpenKinect/libfreenect2)[https://github.com/OpenKinect/libfreenect2]
4. node and npm (https://nodejs.org/en/)[https://nodejs.org/en/]
5. graspit-simulator (https://github.com/graspit-simulator/graspit.git)[https://github.com/graspit-simulator/graspit.git]
6. ipdb (https://pypi.python.org/pypi/ipdb)[https://pypi.python.org/pypi/ipdb]
7. pyassimp (https://github.com/assimp/assimp.git)[https://github.com/assimp/assimp.git]
8. ROS (http://www.ros.org/)[http://www.ros.org/]

Plus a series of apt dependencies:
```bash
libqt4-dev libqt4-opengl-dev libqt4-sql-psql libcoin80-dev libsoqt4-dev libblas-dev liblapack-dev libqhull-dev libeigen3-dev # Graspit

build-essential libturbojpeg libtool autoconf libudev-dev cmake mesa-common-dev freeglut3-dev libxrandr-dev doxygen libxi-dev libjpeg-turbo8-dev pkg-config beignet-dev libglfw3-dev   

libusb-1.0-0-dev libva-dev libjpeg-dev libopenni2-dev ros-kinetic-desktop-full ros-kinetic-moveit ros-kinetic-ar-track-alvar ros-kinetic-manipulation-msgs ros-kinetic-pcl-ros ocl-icd-libopencl1 libqt4-dev libqt4-opengl-dev libqt4-sql-psql libcoin80-dev libsoqt4-dev libblas-dev liblapack-dev libqhull-dev libeigen3-dev ros-kinetic-trac-ik* ros-kinetic-rosbridge-*

cuda
```

For simplicity, here is a series of commands to install the entire project on a fresh Ubuntu 16.04 installation:
```bash
sudo apt install python3-pip
sudo apt install python-pip

cd
pip3 install gitman --user

sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
sudo apt-key adv --keyserver hkp://ha.pool.sks-keyservers.net:80 --recv-key 421C365BD9FF1F717815A3895523BAEEB01FA116

wget https://developer.nvidia.com/compute/cuda/9.0/Prod/local_installers/cuda-repo-ubuntu1604-9-0-local_9.0.176-1_amd64-deb
mv cuda-repo-ubuntu1604-9-0-local_9.0.176-1_amd64-deb cuda-repo-ubuntu1604-9-0-local_9.0.176-1_amd64.deb
sudo dpkg -i cuda-repo-ubuntu1604-9-0-local_9.0.176-1_amd64.deb
sudo apt-key add /var/cuda-repo-9-0-local/7fa2af80.pub
sudo apt-get update

sudo apt install cuda

sudo apt install build-essential libturbojpeg libtool autoconf libudev-dev cmake mesa-common-dev freeglut3-dev libxrandr-dev doxygen libxi-dev libjpeg-turbo8-dev pkg-config beignet-dev libglfw3-dev   libusb-1.0-0-dev libva-dev libjpeg-dev libopenni2-dev ros-kinetic-desktop-full ros-kinetic-moveit ros-kinetic-ar-track-alvar ros-kinetic-manipulation-msgs ros-kinetic-pcl-ros ocl-icd-libopencl1 libqt4-dev libqt4-opengl-dev libqt4-sql-psql libcoin80-dev libsoqt4-dev libblas-dev liblapack-dev libqhull-dev libeigen3-dev ros-kinetic-trac-ik*

pip2 install ipdb --user

curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.0/install.sh | bash
nvm install node

sudo rosdep init
rosdep update

git clone https://github.com/OpenKinect/libfreenect2.git
cd libfreenect2
mkdir build
cd build
cmake .. -DCMAKE_INSTALL_PREFIX=$HOME/freenect2
sudo make -j$(nproc) install

cd

mkdir ros
cd ros
git clone git@github.com:CRLab/CRUI_MICO_WS.git
cd CRUI_MICO_WS

mkdir dependencies
cd dependencies
git clone https://github.com/assimp/assimp.git
cd assimp
mkdir build
cd build
cmake ..
make -j$(nproc)
sudo make install
cd port/PyAssimp
python setup.py install --user
sudo rm -rf /usr/lib/python2.7/dist-packages/pyassimp
cd ../../../..

gitman install
catkin build
cd src/assistive_grasping_ui
npm install
```


## Running
All of the necessary configuration files are in src/launcher which contain parameters for moveit_commander, planners, environment configurations, and many other parameters. Any nodes that need to be brought up can be found in 'src/launcher/launch/everything.launch'. Because the launcher package is part of this workspace git repo make sure you don't mistakenly delete it when using gitman uninstall. gitman version 1.5b1 has a keep (-k) flag when using uninstall so the src directory will not be deleted when cleaning your workspace. 

