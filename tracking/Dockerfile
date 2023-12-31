
########################
# Base for ROS install - helps with development

ARG ROS_DISTRO=noetic
FROM ros:$ROS_DISTRO-ros-base as rmit_panther_rosbase
# FROM husarion/panther:$ROS_DISTRO as rmit_panther_rosbase


# Use bash instead of sh
SHELL ["/bin/bash", "-c"]

# Update Ubuntu Software repository and initialise ROS workspace
RUN apt update && \
    source /opt/ros/$ROS_DISTRO/setup.bash && \
    apt install -y libopencv-dev && \
    apt install -y ros-noetic-pcl-ros && \
    apt install -y ros-noetic-octomap-server && \
    apt install -y ros-noetic-gmapping && \
    apt install -y ros-noetic-navigation && \
    apt install -y python3-numpy && \
    apt install -y libegl1 && \
    apt install -y libgl1 && \
    apt install -y libgomp1 && \
    apt install -y python3-pip \
    && rm -rf /var/lib/apt/lists/*

# Install Open3D
RUN python3 -m pip install --no-cache-dir --upgrade pip && \
    python3 -m pip install --no-cache-dir --upgrade --ignore-installed open3d

########################
# Build software for RMIT Panther
ARG ROS_DISTRO=noetic
FROM rmit_panther_rosbase

# Use bash instead of sh
SHELL ["/bin/bash", "-c"]

WORKDIR /ros_ws
COPY ./noetic_workspace/src/first_pkg ./src/first_pkg
COPY ./noetic_workspace/src/multiple-object-tracking-lidar ./src/multiple-object-tracking-lidar
COPY ./noetic_workspace/src/zed-ros-interfaces ./src/zed-ros-interfaces

RUN source /opt/ros/$ROS_DISTRO/setup.bash && \
    catkin_make --cmake-args -DCMAKE_BUILD_TYPE=Release

# This should not be required - replaced with above
# RUN apt update && \
#     source /opt/ros/$ROS_DISTRO/setup.bash && \
#     rosdep update --rosdistro $ROS_DISTRO && \
#     rosdep install -i --from-path src --rosdistro $ROS_DISTRO -y && \
#     catkin_make && \
#     apt autoremove -y && \
#     apt clean && \
#     rm -rf /var/lib/apt/lists/*


COPY ./ros_entrypoint.sh /
ENTRYPOINT ["/ros_entrypoint.sh"]

