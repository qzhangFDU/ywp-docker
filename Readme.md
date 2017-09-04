
Inspired by [floydhub/dl-docker](https://github.com/floydhub/dl-docker), this docker includes my personal setting for Deep Learning development.

This dockerfile includes:
- Ubuntu 16.04 + Nvidia Cuda 8.0 + CuDNN 6
- Tensorflow 1.3.0 gpu
- Theano 0.9.0
- Keras 1.2.0
- Torch
- Caffe
- Caffe-SSD
- Lasange 0.1
- OpenCV 3
- Jupyter

(personal preference)
- tmux
- vim
- zsh

* Welcome to grab this, but note that I am new to Docker and this dockerfile is also for practice. 

## Install docker
https://docs.docker.com/engine/installation/linux/docker-ce/ubuntu/

```bash
# post-installation
sudo groupadd docker
sudo usermod -aG docker $USER
(log out and in)
docker run hello-world
```

## Install nvidia-docker
https://github.com/NVIDIA/nvidia-docker

## Move docker directory (in case of the lack of the root space)
https://linuxconfig.org/how-to-move-docker-s-default-var-lib-docker-to-another-directory-on-ubuntu-debian-linux
```bash
systemctl stop docker                       # (or) docker service stop
ps aux | grep -i docker | grep -v grep      # make sure nothing 
mkdir /new/path/docker
sudo rsync -axP --info=progress2 /var/lib/docker/ /new/path/docker
sudo subl lib/systemd/system/docker.service
    # (change) ExecStart=/usr/bin/dockerd -g /new/path/docker -H fd://
systemctl daemon-reload
systemctl start docker
```

## Some useful commands

```bash
# build
docker build -t deepaul -f Dockerfile.gpu .

# run a certain python through docker
docker run -it -v "$PWD":/app -w /app -e PYTHONPATH=/root/caffe-ssd/python deepaul python xxx.py

# run a certain python through docker
nvidia-docker run -rm -it \
    -p 8888:8888 -p 6006:6006 \
    -v /sharedfolder:/root/sharedfolder \
    deepaul zsh

# if GUI is needed
nvidia-docker run -it --rm \
    --env="PYTHONPATH=/root/caffe-ssd/python" \
    --env="DISPLAY" \
    --workdir=/app \
    --volume="$PWD":/app \
    --volume="/tmp/.X11-unix:/tmp/.X11-unix" \
    deepaul python

# nvidia-docker run --rm -it \
#    --user=$(id -u) \
#    --env="PYTHONPATH=/root/caffe-ssd/python" \
#    --env="DISPLAY" \
#    --workdir=/app \
#    --volume="$PWD":/app \
#    --volume="/etc/group:/etc/group:ro" \
#    --volume="/etc/passwd:/etc/passwd:ro" \
#    --volume="/etc/shadow:/etc/shadow:ro" \
#    --volume="/etc/sudoers.d:/etc/sudoers.d:ro" \
#    --volume="/tmp/.X11-unix:/tmp/.X11-unix:rw" \
#    deepaul python test.py

# docker size 
docker system df
# (however) real size in the disk is
sudo du -hd 1 /var/lib/docker/aufs/diff | sort -hrk 1 | head -20
```
