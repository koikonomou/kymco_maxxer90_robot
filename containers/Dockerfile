# Build with: sudo docker build -t kymco_im <kymco_maxxer90_robot-location>/containers/Dockerfile
# Run with: sudo docker-compose up --remove-orphans

FROM ros:melodic-ros-core-bionic

LABEL maintainer="Katerina Oikonomou <koikonomou@iit.demokritos.gr>"


RUN apt-get update && apt-get install -y \
	apache2 \
    xvfb \
    python-catkin-tools \
    python-genpy \
    libignition-msgs-dev \
    ros-melodic-rosmsg \
    ros-melodic-ros-control \
    ros-melodic-ros-controllers \
    ros-melodic-hector-gazebo-plugins \
    ros-melodic-gazebo-ros \
    ros-melodic-simulators \
    ros-melodic-gazebo-plugins \
    ros-melodic-gazebo-ros-pkgs \
    ros-melodic-gazebo-ros-control \
    ros-melodic-move-base \
    ros-melodic-rosbridge-server \
    ros-melodic-tf2-web-republisher \
    ros-melodic-robot-state-publisher \
    ros-melodic-position-controllers \
    ros-melodic-effort-controllers \
    ros-melodic-joint-state-controller \
    ros-melodic-controller-manager \
    ros-melodic-ackermann-steering-controller && \
    rm -rf /var/lib/apt-get/lists/*

RUN mkdir -p /opt/catkin_ws/src


RUN apt-get -y upgrade &&\
	apt-get install -y \
        build-essential \
        git \
        python-rosdep && \
    rosdep init && \
    rosdep update && \
    cd /opt/catkin_ws/src && \
    git clone https://github.com/roboskel/roboskel_msgs && \
    git clone https://github.com/osrf/gazebo_models && \
    git clone https://github.com/roboskel/roboskel_ros_resources && \
    git clone https://github.com/roboticsgroup/roboticsgroup_gazebo_plugins && \
    git clone https://github.com/koikonomou/kymco_maxxer90_robot && \
    mkdir -p ~/.gazebo/models && \
    mv gazebo_models/* ~/.gazebo/models/ && \
    rm -rf gazebo_models && \
    cp -r roboskel_ros_resources/gazebo/models/* ~/.gazebo/models && \
    rm -rf roboskel_ros_resources/graspit && \
    cd .. && \
    rosdep install --from-paths src --ignore-src -r -y && \
    /bin/bash -c "source /opt/ros/melodic/setup.bash; catkin_make -DCATKIN_WHITELIST_PACKAGES="roboticsgroup_gazebo_plugins" " && \
    /bin/bash -c "source /opt/ros/melodic/setup.bash; source /opt/catkin_ws/devel/setup.bash; catkin_make" && \
	apt-get purge -y \
	    build-essential \
	    git && \
	apt-get autoremove -y && \
	rm -rf /var/lib/apt-get/lists/*


RUN sed --in-place --expression \
    '$isource "/opt/catkin_ws/devel/setup.bash"' \
    /ros_entrypoint.sh

RUN /bin/bash -c ". /opt/ros/melodic/setup.bash && . /opt/catkin_ws/devel/setup.bash "


ENV DISPLAY :1

EXPOSE 80

# EXPOSE 5920