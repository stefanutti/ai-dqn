# This page contains notes about machine learning experiments done with Tensorflow.

## These are the experiments that I replicated:

- https://github.com/googlecodelabs/tensorflow-for-poets-2
- https://github.com/devsisters/DQN-tensorflow

## Base image:
- docker pull nvidia/cuda:8.0-cudnn5-runtime-ubuntu16.04

## Note:
- If you don't have a GPU graphic card, you can start from the ubuntu 16.04 image and use docker instead of nvidia-docker. Not much else has to be changed

## Start a container:
- nvidia-docker run -it --name ai-dqn -v /tmp/.X11-unix:/tmp/.X11-unix -v /docker-mounts:/docker-mounts -e DISPLAY=unix$DISPLAY --device /dev/dri --device /dev/snd nvidia/cuda:8.0-cudnn5-runtime-ubuntu16.04 bash

## Install - utilities
- apt-get update
- apt-get install vim
- apt-get install git
- apt-get upgrade

## Install - Tensorflow
- apt-get install python-pip python-dev
- apt-get install libcupti-dev (only for GPU: https://www.tensorflow.org/install/install_linux)
- pip install --upgrade pip
- pip install tensorflow or pip install tensorflow-gpu (for GPU)

## Install - DQN
- Read the installation info from here if you need additional info: https://github.com/devsisters/DQN-tensorflow. Next are the commands I have executed
  - cd
  - mkdir experiments
  - cd experiments 
  - git clone https://github.com/devsisters/DQN-tensorflow.git
  - pip install tqdm gym[all] --> ERROR
    - https://github.com/openai/gym/issues/218
      - apt-get install xvfb libav-tools xorg-dev libsdl2-dev swig cmake
      - re-run: pip install tqdm gym[all] // Just to be sure
  - execution of "python main.py --env_name=Breakout-v0 --is_train=True --display=False"
    - If error: GPU
      - Disable GPU in main.py
        - flags.DEFINE_boolean('use_gpu', False, 'Whether to use gpu or not')
    - If error: Memory
      - Change memory usage in config.py
    - If error: AttributeError: 'TimeLimit' object has no attribute 'ale'
      - pip install gym==0.7.3 (Note: downgrade)
    - Now the command runs OK
  - execution of "python main.py --env_name=Breakout-v0 --is_train=True --display=True"
    - If error: pyglet.canvas.xlib.NoSuchDisplayException: Cannot connect to "None"
      - Set these params to docker
        - "docker run ... -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=unix$DISPLAY --device /dev/dri --device /dev/snd ..."
    - If error: Visualization problem - https://github.com/devsisters/DQN-tensorflow/issues/35
      - pip install atari-py==0.0.21 (Note: downgrade)
    - Now the command runs OK

## Save an image of this container:
- nvidia-docker commit ai-dqn stefanutti/cuda:8.0-cudnn5-runtime-ubuntu16.04-tf-dqn-1.2
 nvidia-docker rm ai-dqn
- nvidia-docker run -it --name ai-dqn -v /tmp/.X11-unix:/tmp/.X11-unix -v /docker-mounts:/docker-mounts -e DISPLAY=unix$DISPLAY --device /dev/dri --device /dev/snd stefanutti/cuda:8.0-cudnn5-runtime-ubuntu16.04-tf-dqn-1.2 bash

## Other useful commands:
- nvidia-docker start ai-dqn
- nvidia-docker exec -it ai-dqn bash
- nvidia-docker stop ai-dqn

Bye
