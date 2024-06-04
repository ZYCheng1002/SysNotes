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

