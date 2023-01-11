# Based on  https://github.com/osrf/docker_images/blob/master/ros/galactic/ubuntu/focal/ros1-bridge/Dockerfile
FROM ros:galactic-ros1-bridge-focal

ENV AMBF_VERSION_TAG=ambf-2.0 

# Install OpenGL & X11 dependencies
RUN apt update && apt install -y \
        freeglut3-dev \
        libao-dev \
        libglew-dev \
        libglfw3-dev \ 
        libglm-dev \ 
        libglu1-mesa-dev \
        libmpg123-dev \
        librosconsole-dev \
        libusb-1.0-0-dev \
        libxcursor-dev \
        libxinerama-dev \
        libxmlrpcpp-dev \
        mesa-common-dev \
        mesa-utils \
    && rm -rf /var/lib/apt/lists/*

# Install ROS & AMBF dependencies
RUN apt update && apt install -y \
        git \ 
        libasound2-dev \
        pip \
        python3-pykdl \
        ros-galactic-cv-bridge \ 
        ros-galactic-image-transport \
        ros-noetic-cv-bridge \ 
        ros-noetic-image-transport \
        ros-noetic-tf \
    && rm -rf /var/lib/apt/lists/*

# Clone ros1_brdige
RUN apt remove -y ros-galactic-ros1-bridge \
    && cd && mkdir -p bridge_ws/src && cd bridge_ws/src \
    && git clone -b galactic --single-branch https://github.com/ros2/ros1_bridge.git

# Clone AMBF, ROS2 ambf_msgs
RUN cd && mkdir -p ros1_ws/src && cd ros1_ws/src \
    && git clone -b $AMBF_VERSION_TAG --single-branch https://github.com/WPI-AIM/ambf.git \
    && cd && mkdir -p ros2_ws/src && cd ros2_ws/src \
    && git clone -b master --single-branch https://github.com/macmacal/ambf_ros2_msgs.git

# Build AMBF
RUN . /opt/ros/noetic/setup.sh \
    && cd /root/ros1_ws/src/ambf \
    && mkdir build \ 
    && cd build \
    && cmake .. \
    && make 

# Build ROS1 ambf_msgs
RUN . /opt/ros/noetic/setup.sh \
    && cd /root/ros1_ws \
    && catkin_make_isolated

# Build ROS2 ambf_msgs
RUN . /opt/ros/galactic/setup.sh \ 
    && cd /root/ros2_ws \
    && colcon build --symlink-install

# Build & source ros1_bridge
RUN exec bash \
    && . /opt/ros/noetic/setup.sh \
    && . /opt/ros/galactic/setup.sh \ 
    && . /root/ros1_ws/install_isolated/setup.sh \
    && . /root/ros2_ws/install/setup.bash \
    && cd /root/bridge_ws \
    && colcon build --symlink-install \
    && source install/local_setup.bash
