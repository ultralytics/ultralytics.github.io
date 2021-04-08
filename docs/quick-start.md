# Getting Started

## Requirements
---
You will need Python >= 3.8 and PIP in order to follow this guide.  

The rest of the requirements are listed in './requirements.txt'

*_If you have mutliple versions of python installed, ensure you are using the correct one_

## Installation
---

Clone the repository
```bash
$ git clone https://github.com/ultralytics/yolov5.git
```

Enter the repository root directory
```bash
$ cd yolov5
```

Install the required packages from your cloned repository root directory
```bash
$ pip install -r requirements.txt
```

## Packaged Environments
---

For a quick and hassle free setup YOLOv5 has been packaged with all dependencies* for the following environments  

_*including [CUDA](https://developer.nvidia.com/cuda)/[CUDNN](https://developer.nvidia.com/cudnn), [Python](https://www.python.org/) and [PyTorch](https://pytorch.org/)_

- **Google Colab and Kaggle** notebooks with free GPU: <a href="https://colab.research.google.com/github/ultralytics/yolov5/blob/master/tutorial.ipynb"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"></a> <a href="https://www.kaggle.com/ultralytics/yolov5"><img src="https://kaggle.com/static/images/open-in-kaggle.svg" alt="Open In Kaggle"></a>
- **Google Cloud** Deep Learning VM. See [GCP Quickstart Guide](tutorials/articles/GCP-Quickstart.md)
- **Amazon** Deep Learning AMI. See [AWS Quickstart Guide](tutorials/articles/AWS-Quickstart.md)
- **Docker Image**. See [Docker Quickstart Guide](tutorials/articles/Docker-Quickstart.md) <a href="https://hub.docker.com/r/ultralytics/yolov5"><img src="https://img.shields.io/docker/pulls/ultralytics/yolov5?logo=docker" alt="Docker Pulls"></a>

## Inference - Detect Objects
---

### From Your Cloned Repository

To get started with object detection using the [latest YOLO models](https://github.com/ultralytics/yolov5/releases), run this command from your repository root directory. Results are saved to './runs/detect'
```bash
$ python detect.py --source OPTION
```
Replace OPTION with your selection, to detect from:  

* **Webcam** : (OPTION = 0) _For live object detection from your connected webcam_
* **Image** : (OPTION = filename.jpg) _Create a copy of the image with an object detection overlay_
* **Video** : (OPTION = filename.mp4) _Create a copy of the video with an object detection overlay_
* **Directory** : (OPTION = directory_name/) _Create a copy of all file with an object detection overlay_
* **Global File Type** (OPTION = directory_name/*.jpg) _Create a copy of all file with an object detection  overlay_
* **RTSP stream** : (OPTION = rtsp://170.93.143.139/rtplive/470011e600ef003a004ee33696235daa) _For live object  detection from a stream_
* **RTMP stream** : (OPTION = rtmp://192.168.1.105/live/test) _For live object detection from a stream_
* **HTTP stream** : (OPTION =  http://112.50.243.8/PLTV/88888888/224/3221225900/1.m3u8) _For live object detection from a stream_

The following file formats are currently supported:

* **Images:** bmp, jpg, jpeg, png, tif, tiff, dng, webp, mpo
* **Videos:** mov, avi, mp4, mpg, mpeg, m4v, wmv, mkv

### From PyTorch Hub

Inference can be run directly from PyTorch Hub without cloning the repository. The necesary files will be downloaded into your temporary directory.

Here is an example script that uses the [latest YOLOv5s model](https://github.com/ultralytics/yolov5/releases) and the repositories example images.

```python
import torch

# Model
model = torch.hub.load('ultralytics/yolov5', 'yolov5s')

# Images
dir = 'https://github.com/ultralytics/yolov5/raw/master/data/images/'
imgs = [dir + f for f in ('zidane.jpg', 'bus.jpg')]  # batch of images

# Inference
results = model(imgs)
results.print()  # or .show(), .save()
```

## Training Models
---
Check out the [tutorial section](tutorials/tutorials.md) for guides on training custom models