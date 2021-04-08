To get started with YOLOv5 🚀 in a Docker image follow the instructions below. Other quickstart options for YOLOv5 include our [Colab Notebook](https://colab.research.google.com/github/ultralytics/yolov5/blob/master/tutorial.ipynb) <a href="https://colab.research.google.com/github/ultralytics/yolov5/blob/master/tutorial.ipynb"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"></a> <a href="https://www.kaggle.com/ultralytics/yolov5"><img src="https://kaggle.com/static/images/open-in-kaggle.svg" alt="Open In Kaggle"></a> and a [GCP Deep Learning VM](https://github.com/ultralytics/yolov5/wiki/GCP-Quickstart). 

## 1. Install Docker and Nvidia-Docker

Docker images come with all dependencies preinstalled, however Docker itself requires installation, and relies of nvidia driver installations in order to interact properly with local GPU resources. The requirements are: 
- Nvidia Driver >= 455.23 https://www.nvidia.com/Download/index.aspx
- Nvidia-Docker https://github.com/NVIDIA/nvidia-docker
- Docker Engine - CE >= 19.03 https://docs.docker.com/install/

## 2. Pull Image
The Ultralytics YOLOv5 DockerHub is https://hub.docker.com/r/ultralytics/yolov5 ![Docker Pulls](https://img.shields.io/docker/pulls/ultralytics/yolov5?logo=docker). [Docker Autobuild](https://docs.docker.com/docker-hub/builds/) is used to automatically build images from the latest repository commits, so the `ultralytics/yolov5:latest` image hosted on the DockerHub **will always be in sync with the most recent repository commit**. To pull this image:
```bash
sudo docker pull ultralytics/yolov5:latest
```

## 3. Run Container
Run an interactive instance of this image (called a "container") using `-it`:
```bash
sudo docker run --ipc=host -it ultralytics/yolov5:latest
```

Run a container with **local file access** (like COCO training data in `/coco`) using `-v`:
```bash
sudo docker run --ipc=host -it -v "$(pwd)"/coco:/usr/src/coco ultralytics/yolov5:latest
```

Run a container with **GPU access** using `--gpus all`:
```bash
sudo docker run --ipc=host --gpus all -it ultralytics/yolov5:latest
```

<p align="center"><img width="900" src="https://user-images.githubusercontent.com/26833433/112548245-35264280-8dbc-11eb-990e-2aa89cd999ba.jpg"></p>

## 4. Run Commands
Run commands from within the running Docker container, i.e.:
```bash
$ python train.py  # train a model
$ python test.py --weights yolov5s.pt  # test a model for Precision, Recall and mAP
$ python detect.py --weights yolov5s.pt --source path/to/images  # run inference on images and videos
```
