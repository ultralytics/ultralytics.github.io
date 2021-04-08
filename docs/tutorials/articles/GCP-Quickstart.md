This quickstart guide helps new users run YOLOv5 🚀 on a Google Cloud Platform (GCP) Deep Learning Virtual Machine (VM) ⭐. New GCP users are eligible for a [$300 free credit offer](https://cloud.google.com/free/docs/gcp-free-tier#free-trial). Other quickstart options for YOLOv5 include our [Colab Notebook](https://colab.research.google.com/github/ultralytics/yolov5/blob/master/tutorial.ipynb) <a href="https://colab.research.google.com/github/ultralytics/yolov5/blob/master/tutorial.ipynb"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"></a> <a href="https://www.kaggle.com/ultralytics/yolov5"><img src="https://kaggle.com/static/images/open-in-kaggle.svg" alt="Open In Kaggle"></a> and our Docker image at https://hub.docker.com/r/ultralytics/yolov5 ![Docker Pulls](https://img.shields.io/docker/pulls/ultralytics/yolov5?logo=docker).

## 1. Create VM
Select a **Deep Learning VM** from the [GCP marketplace](https://console.cloud.google.com/marketplace/details/click-to-deploy-images/deeplearning), select an **n1-standard-8** instance (with 8 vCPUs and 30 GB memory), add a GPU of your choice, check 'Install NVIDIA GPU driver automatically on first startup?', and select a 300 GB SSD Persistent Disk for sufficient I/O speed, then click 'Deploy'. **All dependencies are included** in the preinstalled [Anaconda](https://docs.anaconda.com/anaconda/packages/pkg-docs/) Python environment.
<img width="1000" alt="GCP Marketplace" src="https://user-images.githubusercontent.com/26833433/105811495-95863880-5f61-11eb-841d-c2f2a5aa0ffe.png">

## 2. Setup VM
Clone this repo and install [requirements.txt](https://github.com/ultralytics/yolov5/blob/master/requirements.txt) dependencies, including **Python>=3.8** and **PyTorch>=1.7**.

```bash
$ git clone https://github.com/ultralytics/yolov5  # clone repo
$ cd yolov5
$ pip install -r requirements.txt  # install dependencies
```

## 3. Run Commands
```bash
$ python train.py  # train a model
$ python test.py --weights yolov5s.pt  # test a model for Precision, Recall and mAP
$ python detect.py --weights yolov5s.pt --source path/to/images  # run inference on images and videos
```

<img width="1015" alt="GCP terminal" src="https://user-images.githubusercontent.com/26833433/105813160-47266900-5f64-11eb-9fa7-c43f45635de0.png">

## Optional Extras

Add 64GB of swap memory (to `--cache` large datasets).
```bash
sudo fallocate -l 64G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
free -h  # check memory
```

Mount local SSD
```bash
lsblk
sudo mkfs.ext4 -F /dev/nvme0n1
sudo mkdir -p /mnt/disks/nvme0n1
sudo mount /dev/nvme0n1 /mnt/disks/nvme0n1
sudo chmod a+w /mnt/disks/nvme0n1
cp -r coco /mnt/disks/nvme0n1
```