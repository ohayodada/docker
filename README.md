# Requirements

1. Install docker[https://docs.docker.com/engine/install/ubuntu/]
2. Install nvidia-dock[https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html#docker]
3. Install and setup GUI visualization
```shell
$ sudo apt-get install x11-xserver-utils
$ xhost +local:
```
# How to build
```shell
$ git clone https://github.com/ohayodada/docker
$ cd rgbdslamV2_nvidia
$ chmod +x build.sh
$ ./buils.sh
```
# How to run
```shell
$ docker pull ohayodada/rgbdslam_nvidia/:latest
$ docker run --gpus all -it --ipc=host --net=host -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=$DISPLAY ohayodada/rgbdslam_nvidia
$ roslaunch rgbdslam rgbdslam.launch
```