FROM ubuntu:xenial

# install packages
RUN apt-get update && apt-get install -y --no-install-recommends \
    dirmngr \
    gnupg2 \
    && rm -rf /var/lib/apt/lists/*

# setup keys
RUN apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 421C365BD9FF1F717815A3895523BAEEB01FA116

# setup sources.list
RUN echo "deb http://packages.ros.org/ros/ubuntu xenial main" > /etc/apt/sources.list.d/ros-latest.list

# install bootstrap tools
RUN apt-get update && apt-get install --no-install-recommends -y \
    python-rosdep \
    python-rosinstall \
    python-vcstools \
    && rm -rf /var/lib/apt/lists/*

# setup environment
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8

# bootstrap rosdep
RUN rosdep init \
    && rosdep update

# install ros packages
ENV ROS_DISTRO kinetic
RUN apt-get update && apt-get install -y \
    ros-kinetic-ros-core=1.3.1-0* \
    && rm -rf /var/lib/apt/lists/*

RUN apt-get update

RUN apt-get install python3-pip -y
RUN apt-get install python-pip -y

RUN pip3 install gitman --user

RUN apt-get install -y build-essential libturbojpeg libtool autoconf libudev-dev cmake mesa-common-dev freeglut3-dev libxrandr-dev doxygen libxi-dev libjpeg-turbo8-dev pkg-config beignet-dev libglfw3-dev libusb-1.0-0-dev libva-dev libjpeg-dev libopenni2-dev ros-kinetic-desktop-full ros-kinetic-moveit ros-kinetic-ar-track-alvar ros-kinetic-manipulation-msgs ros-kinetic-pcl-ros ocl-icd-libopencl1 libqt4-dev libqt4-opengl-dev libqt4-sql-psql libcoin80-dev libsoqt4-dev libblas-dev liblapack-dev libqhull-dev libeigen3-dev ros-kinetic-rosbridge-* ros-kinetic-trac-ik*

RUN pip2 install ipdb --user
RUN pip2 install pygame --user

RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.0/install.sh | bash
RUN nvm install node

RUN cd $HOME
RUN git clone https://github.com/OpenKinect/libfreenect2.git
RUN cd libfreenect2
RUN mkdir build
RUN cd build
RUN cmake .. -DCMAKE_INSTALL_PREFIX=$HOME/freenect2
RUN sudo make -j$(nproc) install

RUN cd $HOME

RUN mkdir dependencies
RUN cd dependencies
RUN git clone https://github.com/graspit-simulator/graspit.git
RUN cd graspit
RUN mkdir build
RUN cd build
RUN cmake ..
RUN sudo make -j$(nproc) install

RUN cd $HOME

RUN mkdir ros
RUN cd ros
RUN git clone git@github.com:CRLab/CRUI_MICO_WS.git
RUN cd CRUI_MICO_WS

RUN mkdir dependencies
RUN cd dependencies
RUN git clone https://github.com/assimp/assimp.git
RUN cd assimp
RUN mkdir build
RUN cd build
RUN cmake ..
RUN make -j$(nproc)
RUN sudo make install
RUN cd ../port/PyAssimp
RUN python setup.py install --user
RUN sudo rm -rf /usr/lib/python2.7/dist-packages/pyassimp
RUN cd ../../../..

RUN gitman install
RUN catkin build

# setup entrypoint
COPY ./ros_entrypoint.sh /

ENTRYPOINT ["/ros_entrypoint.sh"]
CMD ["bash"]