# ubuntu22-ros2-cudnn



dockerfile如下

```dockerfile
FROM nvidia/cuda:12.4.1-cudnn-devel-ubuntu22.04
RUN locale
run apt-get update
RUN apt-get install locales -y
RUN locale-gen en_US en_US.UTF-8
RUN update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
RUN export LANG=en_US.UTF-8
# 定义时区参数
ENV TZ=Asia/Shanghai
# 设置时区
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo '$TZ' > /etc/timezone
RUN apt-get install software-properties-common -y
RUN add-apt-repository universe
RUN apt-get update 
RUN apt-get install wget -y
RUN wget https://raw.githubusercontent.com/ros/rosdistro/master/ros.key 
RUN apt-key add ros.key
# 安装位置位于: /etc/apt/trusted.gpg
RUN echo "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/trusted.gpg] http://packages.ros.org/ros2/ubuntu $(. /etc/os-release && echo $UBUNTU_CODENAME) main" |  tee /etc/apt/sources.list.d/ros2.list > /dev/null
RUN apt-get update 
RUN apt-get install ros-dev-tools -y
RUN apt-get upgrade -y
RUN apt-get install ros-iron-desktop -y
RUN echo "source /opt/ros/iron/setup.bash" >> ~/.bashrc
RUN source ~/.bashrc
```


# ubuntu 18 ros docker file
```dockerfile
FROM nvidia/cuda:11.6.1-cudnn8-devel-ubuntu20.04

ARG DEBIAN_FRONTEND=noninteractive
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo '$TZ' > /etc/timezone

RUN apt update 
RUN apt install locales
RUN locale-gen en_US en_US.UTF-8
RUN update-locale LC_ALL=en_US.UTF-8 LANG=en_US.UTF-8
ENV LANG=en_US.UTF-8

# install ros
RUN apt-get install wget -y
RUN wget http://packages.ros.org/ros.key
RUN apt-key add ros.key
RUN apt-get install -y lsb-release
RUN sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
RUN apt update
RUN apt install ros-noetic-desktop -y
RUN echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
RUN /bin/bash -c "source ~/.bashrc"

# install pcl-ros
RUN apt-get install ros-noetic-pcl-ros -y

#install glog gflag
RUN apt-get install libgoogle-glog-dev libgflags-dev -y
```

# start docker
```bash
USER_ID=$(id -u)
GRP=$(id -g -n)
GRP_ID=$(id -g)
LOCAL_HOST=`hostname`
DOCKER_HOME="/home/$USER"

if [ "$USER" == "root" ];then
    DOCKER_HOME="/root"
fi
if [ ! -d "$HOME/.cache" ];then
    mkdir "$HOME/.cache"
fi

IMG="ubuntu20:latest"

CONTAINER_NAME=ubuntu20_ros

docker pull $IMG || true

docker run -it \
        -d \
		--privileged \
		--name $CONTAINER_NAME \
		-e DOCKER_USER=$USER \
		-e USER=$USER \
		-e DOCKER_USER_ID=$USER_ID \
		-e DOCKER_GRP=$GRP \
		-e DOCKER_GRP_ID=$GRP_ID \
		--env ROS_DOMAIN_ID=$(date +%N) \
		-v /tmp/.X11-unix:/tmp/.X11-unix:rw \
		-e DISPLAY=unix$DISPLAY \
		-v /media:/media \
		-v $HOME/.cache:${DOCKER_HOME}/.cache \
		-v /etc/localtime:/etc/localtime:ro \
		-v /home/$USER/idea_slam:/work/share \
		-v /dev:/dev \
		--net host \
		--shm-size 512M \
		-w /work/share \
		$IMG \
		/bin/bash
```

# into docker

```bash
xhost +local:root 1>/dev/null 2>&1

CONTAINER_NAME=ubuntu20_ros

docker exec -u root -it $CONTAINER_NAME /bin/bash

xhost -local:root 1>/dev/null 2>&1
```

# stop docker

```bash
#docker kill $(docker ps -a -q); docker rm $(docker ps -a -q )

CONTAINER_NAME=ubuntu20_ros

docker stop $CONTAINER_NAME
docker rm $CONTAINER_NAME

```