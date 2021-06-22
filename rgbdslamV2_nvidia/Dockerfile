FROM nvidia/cudagl:10.0-devel-ubuntu16.04

RUN apt-get update;

# ############################################################
# ##################### Install ROS ##########################
# ############################################################
RUN /bin/bash -c " \
   # Install ROS
      echo \"deb http://packages.ros.org/ros/ubuntu xenial main\" >> /etc/apt/sources.list.d/ros-latest.list; \
      apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654; \
      apt-get update; \
      apt-get install -y ros-kinetic-desktop-full"

RUN /bin/bash -c " \
   # Rosdep init and update
      rosdep init; \
      rosdep update; \
      echo \source /opt/ros/kinetic/setup.bash\ >> ~/.bashrc; \
      source ~/.bashrc;"
   
# ############################################################
# ########### Install rgbdslam_v2 Dependencies ###############
# ############################################################

RUN /bin/bash -c " \
    # Install g2o dependencies
       sudo apt-get install -y wget git libsuitesparse-dev ros-kinetic-openni-launch libglew-dev libdevil1c2 libdevil-dev; \
    # Install Eigen headers
       cd ~; \
       mkdir deps; \
       cd deps; \
       wget https://gitlab.com/libeigen/eigen/-/archive/3.2.10/eigen-3.2.10.tar.bz2; \
       mkdir eigen; \
       tar -xvjf eigen-3.2.10.tar.bz2 -C eigen --strip-components 1; \
    # Clone g2o fork
       cd ~/deps; \
       git clone https://github.com/felixendres/g2o.git; \
       cd ~/deps/g2o; \
       mkdir build; \
       cd build; \
       sed -i '/SET(G2O_EIGEN3_INCLUDE\ \${EIGEN3_INCLUDE_DIR}/c\SET(G2O_EIGEN3_INCLUDE\ \"\/root\/deps\/eigen\"\ CACHE\ PATH\ \"Directory\ of\ Eigen3\")' /root/deps/g2o/CMakeLists.txt; \
       cmake ../; \
       make -j8; \
       sudo make install; \
       sudo ldconfig; \
    # Install PCL 1.8
       cd ~/deps; \
       wget https://github.com/PointCloudLibrary/pcl/archive/pcl-1.8.0.tar.gz; \
       tar -xvzf pcl-1.8.0.tar.gz; \
       cd ~/deps/pcl-pcl-1.8.0; \
       sed -i '146iSET(CMAKE_CXX_FLAGS\ \"\${CMAKE_CXX_FLAGS}\ -std=c++11\")' CMakeLists.txt; \
       mkdir build; \
       cd build; \
       cmake ../; \
       make -j8; \
       sudo make install; \
       sudo ldconfig"

# ############################################################
# ################# Install RGBDSLAMv2 #######################
# ############################################################

RUN /bin/bash -c " \
      cd ~; \
      mkdir -p catkin_ws/src; \
      cd catkin_ws/src; \
      git clone https://github.com/felixendres/rgbdslam_v2; \
      sed -i 's/1.7/1.8/g' rgbdslam_v2/CMakeLists.txt; \
      cd ..; \
      catkin_make -j8; \
      echo \source /root/catkin_ws/devel/setup.bash\ >> ~/.bashrc; \
      source ~/.bashrc"


