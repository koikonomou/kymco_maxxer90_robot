# Build with: sudo docker build -t kymco_im <kymco_maxxer90_robot-location>/containers/Dockerfile
# Run with: sudo docker-compose up --remove-orphans

FROM ros:melodic-ros-core-bionic

LABEL maintainer="Katerina Oikonomou <koikonomou@iit.demokritos.gr>"

RUN apt-get update && apt-get install -y \
	apache2 \
    xvfb \
    python-catkin-tools \
    ros-melodic-octomap-mapping \
    ros-melodic-octomap-msgs \
    ros-melodic-octomap-ros \
    ros-melodic-octomap-server \
    ros-melodic-gazebo-ros-pkgs \
    ros-melodic-gazebo-ros-control \
    ros-melodic-move-base \
    ros-melodic-pid \
    ros-melodic-robot-state-publisher \
    ros-melodic-position-controllers \
    ros-melodic-effort-controllers \
    ros-melodic-joint-state-controller \
    ros-melodic-controller-manager \
    ros-melodic-ackermann-steering-controller

RUN mkdir -p /opt/catkin_ws/src

RUN apt-get install -y \
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
    cp -r roboskel_ros_resources /var/www/html && \
    mkdir -p ~/.gazebo/models && \
    mv gazebo_models/* ~/.gazebo/models/ && \
    rm -rf gazebo_models && \
    cp -r roboskel_ros_resources/gazebo/models/* ~/.gazebo/models && \
    rm -rf roboskel_ros_resources/graspit && \
    cd .. && \
    rosdep install --from-paths src --ignore-src -r -y && \
    /bin/bash -c "source /opt/ros/melodic/setup.bash; catkin_make -DCATKIN_WHITELIST_PACKAGES="roboticsgroup_gazebo_plugins" " && \
    # /bin/bash -c "source /opt/ros/melodic/setup.bash; catkin_make --pkg roboskel_msgs" && \
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

EXPOSE 80