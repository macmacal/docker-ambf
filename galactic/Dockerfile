# Based on  https://github.com/osrf/docker_images/blob/master/ros/galactic/ubuntu/focal/ros1-bridge/Dockerfile
FROM ros:galactic-ros1-bridge-focal

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

# Clone AMBF
RUN cd && git clone https://github.com/WPI-AIM/ambf.git \
    && cd ambf && git checkout ambf-2.0

# Build AMBF
RUN . /opt/ros/noetic/setup.sh \
    && cd ~/ambf \
    && mkdir build \ 
    && cd build \
    && cmake .. \
    && make