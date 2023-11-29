FROM osrf/ros:noetic-desktop-full-focal

RUN apt update \
    && apt install -y curl \
    && curl -s https://raw.githubusercontent.com/ros/rosdistro/master/ros.asc | apt-key add - \
    && apt update \
    && apt install -y ros-noetic-navigation \
    && apt install -y ros-noetic-robot-localization \
    && apt install -y ros-noetic-robot-state-publisher \
    && apt install -y git \
    && apt install -y ros-noetic-ackermann-msgs \
    && apt install -y ros-noetic-geometry2 \
    && apt install -y ros-noetic-hector-gazebo \
    && apt install -y ros-noetic-hector-models \
    && apt install -y ros-noetic-jsk-rviz-plugins \
    && apt install -y ros-noetic-ros-control \
    && apt install -y ros-noetic-ros-controllers \
    && apt install -y ros-noetic-velodyne-simulator \
    && rm -rf /var/lib/apt/lists/*

RUN apt update \
    && apt install -y software-properties-common \
    && add-apt-repository -y ppa:borglab/gtsam-release-4.0 \
    && apt update \
    && apt install -y libgtsam-dev libgtsam-unstable-dev \
    && rm -rf /var/lib/apt/lists/*

SHELL ["/bin/bash", "-c"]

RUN mkdir -p ~/catkin_ws/src \
    && cd ~/catkin_ws/src \
    && git clone https://github.com/amaxs/LIO-SAM.git \
    && cd .. \
    && source /opt/ros/noetic/setup.bash \
    && catkin_make


RUN cd ~/catkin_ws/src \
    && git clone https://gitlab.engr.illinois.edu/gemillins/POLARIS_GEM_e2.git \
    && cd POLARIS_GEM_e2/ \
    && curl https://raw.githubusercontent.com/amaxs/Docker-SLAM-Playground/main/tools/patch_simu/simu_update.patch > simu_update.patch \
    && git config --global user.email "simu_update@patch.com" \
    && git am simu_update.patch \
    && cd .. \
    && cd .. \
    && source /opt/ros/noetic/setup.bash \
    && catkin_make

RUN echo "source /opt/ros/noetic/setup.bash" >> /root/.bashrc \
    && echo "source /root/catkin_ws/devel/setup.bash" >> /root/.bashrc

WORKDIR /root/catkin_ws
