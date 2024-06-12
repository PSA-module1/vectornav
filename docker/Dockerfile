# Use the noetic-ros-base image as the base image
FROM ros:noetic-ros-base

ARG DEBIAN_FRONTEND=noninteractive

# Setup timezone
RUN echo 'Etc/UTC' > /etc/timezone && \
    rm /etc/localtime && \
    ln -s /usr/share/zoneinfo/Asia/Singapore /etc/localtime && \
    apt-get update && \
    apt-get install -q -y --no-install-recommends tzdata

# Install basic apt tools and dependencies
RUN apt update && apt upgrade -y && apt install -y \
    vim wget tmux git build-essential cmake \
    ros-${ROS_DISTRO}-tf2-ros \
    ros-${ROS_DISTRO}-tf2-geometry-msgs \
    && rm -rf /var/lib/apt/lists/*

# Setup environment
ENV LANG C.UTF-8
ENV LC_ALL C.UTF-8
ENV ROS_DISTRO noetic

# Source ROS setup scripts
RUN . /opt/ros/${ROS_DISTRO}/setup.sh \
    && echo "source /opt/ros/${ROS_DISTRO}/setup.bash" >> ~/.bashrc

# Create and build the catkin workspace
RUN echo "Running catkin_make..." \
    && . /opt/ros/${ROS_DISTRO}/setup.sh \
    && mkdir -p /root/catkin_ws/src && cd /root/catkin_ws/src \
    && git clone https://github.com/PSA-module1/vectornav.git \
    && cd /root/catkin_ws \
    && catkin_make --cmake-args -DCMAKE_BUILD_TYPE=Release \
    && echo "source /root/catkin_ws/devel/setup.bash" >> ~/.bashrc

# Set the working directory
WORKDIR /root/catkin_ws

CMD ["bash"]
