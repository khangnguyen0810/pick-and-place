FROM ros:noetic-ros-base

# Install necessary packages
RUN apt-get update && \
    apt-get install -y \
    vim iputils-ping net-tools python3-pip dos2unix \
    ros-noetic-robot-state-publisher ros-noetic-moveit \
    ros-noetic-rosbridge-suite ros-noetic-joy \
    ros-noetic-ros-control ros-noetic-ros-controllers \
    ros-noetic-tf2-web-republisher python3-catkin-tools && \
    rm -rf /var/lib/apt/lists/*

# Create a symlink for python to point to python3
RUN ln -s /usr/bin/python3 /usr/bin/python

# Install Python dependencies
RUN pip3 install rospkg jsonpickle

# Set up ROS workspace
ENV ROS_WORKSPACE=/catkin_ws
COPY ./ROS/src/moveit_msgs $ROS_WORKSPACE/src/moveit_msgs
COPY ./ROS/src/niryo_moveit $ROS_WORKSPACE/src/niryo_moveit
COPY ./ROS/src/niryo_one_ros $ROS_WORKSPACE/src/niryo_one_ros
COPY ./ROS/src/niryo_one_urdf $ROS_WORKSPACE/src/niryo_one_urdf
COPY ./ROS/src/ros_tcp_endpoint $ROS_WORKSPACE/src/ros_tcp_endpoint

# Copy setup and tutorial files
COPY ./docker/set-up-workspace /setup.sh
COPY docker/tutorial /

RUN /bin/bash -c "find $ROS_WORKSPACE -type f -print0 | xargs -0 dos2unix"

RUN dos2unix /tutorial && dos2unix /setup.sh && chmod +x /setup.sh && /setup.sh && rm /setup.sh

# Change working directory
WORKDIR $ROS_WORKSPACE

# Make Python scripts executable
RUN chmod +x src/niryo_moveit/scripts/*.py

# Set entrypoint
ENTRYPOINT ["/tutorial"]