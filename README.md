# ai-dqn
Nvidia ai-dqn experiments

Base image from docker store:
- docker pull nvidia/cuda:8.0-cudnn5-runtime-ubuntu16.04

Start a container:
- nvidia-docker run -it --name ai-dqn -v /tmp/.X11-unix:/tmp/.X11-unix -v /docker-mounts:/docker-mounts -e DISPLAY=unix$DISPLAY --device /dev/dri --device /dev/snd nvidia/cuda:8.0-cudnn5-runtime-ubuntu16.04 bash

Packages to install:
- apt-get update
- apt-get install vim
- apt-get install git
- apt-get upgrade
- apt-get install python-pip python-dev
- apt-get install libcupti-dev
- pip install --upgrade pip
- pip install tensorflow-gpu 

Install dqn:
- cd
- mkdir experiments
- cd experiments 
- git clone https://github.com/devsisters/DQN-tensorflow.git
- pip install tqdm gym[all] --> ERROR
  - apt-get install xvfb libav-tools xorg-dev libsdl2-dev swig cmake
    - pip install tqdm gym[all] // Just to be sure
- python main.py --env_name=Breakout-v0 --is_train=True --display=True --> ERROR 
  WITH GRAPHIC
  - pip install atari-py==0.0.21
    - python main.py --env_name=Breakout-v0 --is_train=True --display=True

Save an image of this container:
- nvidia-docker commit ai-dqn stefanutti/cuda:8.0-cudnn5-runtime-ubuntu16.04-tf-dqn-1.2
 nvidia-docker rm ai-dqn
- nvidia-docker run -it --name ai-dqn -v /tmp/.X11-unix:/tmp/.X11-unix -v /docker-mounts:/docker-mounts -e DISPLAY=unix$DISPLAY --device /dev/dri --device /dev/snd stefanutti/cuda:8.0-cudnn5-runtime-ubuntu16.04-tf-dqn-1.2 bash

Other useful commands:
- nvidia-docker start ai-dqn
- nvidia-docker exec -it ai-dqn bash
- nvidia-docker stop ai-dqn
