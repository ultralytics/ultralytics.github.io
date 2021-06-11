📚 This guide explains how to export a trained YOLOv5 🚀 model from PyTorch to ONNX and TorchScript formats.

## Before You Start

Clone this repo and install [requirements.txt](https://github.com/ultralytics/yolov5/blob/master/requirements.txt) dependencies, including **Python>=3.8** and **PyTorch==1.7**.

```bash
git clone https://github.com/ultralytics/yolov5  # clone repo
cd yolov5
pip install -r requirements.txt  # base requirements
pip install coremltools>=4.1 onnx>=1.9.0 scikit-learn==0.19.2  # export requirements
```

## Export a Trained YOLOv5 Model

This command exports a pretrained YOLOv5s model to ONNX, TorchScript and CoreML formats. `yolov5s.pt` is the lightest and fastest model available. Other options are `yolov5m.pt`, `yolov5l.pt` and `yolov5x.pt`, or you own checkpoint from training a custom dataset `runs/exp0/weights/best.pt`. For details on all available models please see our README [table](https://github.com/ultralytics/yolov5#pretrained-checkpoints).
```bash
python models/export.py --weights yolov5s.pt --img 640 --batch 1  # export at 640x640 with batch size 1
```

Output:
```
Namespace(batch_size=1, device='cpu', dynamic=False, half=False, img_size=[640, 640], include=['torchscript', 'onnx', 'coreml'], inplace=False, optimize=False, simplify=False, train=True, weights='./yolov5s.pt')
YOLOv5 🚀 v5.0-87-gf12cef8 torch 1.8.1+cu101 CPU

Fusing layers... 
Model Summary: 224 layers, 7266973 parameters, 0 gradients

PyTorch: starting from ./yolov5s.pt (14.8 MB)

TorchScript: starting export with torch 1.8.1+cu101...
TorchScript: export success, saved as ./yolov5s.torchscript.pt (29.4 MB)

ONNX: starting export with onnx 1.9.0...
ONNX: export success, saved as ./yolov5s.onnx (29.1 MB)

CoreML: starting export with coremltools 4.1...
CoreML: export success, saved as ./yolov5s.mlmodel (29.1 MB)

Export complete (10.40s). Visualize with https://github.com/lutzroeder/netron.
```

The 3 exported models will be saved alongside the original PyTorch model:
<p align="center"><img width="800" src="https://user-images.githubusercontent.com/26833433/112540320-53874080-8db2-11eb-9e8c-e2a8ae4c710a.jpg"></p>

[Netron Viewer](https://github.com/lutzroeder/netron) is recommended for visualizing exported models:
<p align="center"><img width="800" src="https://user-images.githubusercontent.com/26833433/112540590-a6f98e80-8db2-11eb-9381-33c5e6bc14df.jpg"></p>


## TensorRT Deployment

For deployment of YOLOv5 from PyTorch *.pt weights to [NVIDIA TensorRT](https://developer.nvidia.com/tensorrt) see https://github.com/wang-xinyu/tensorrtx. 


## Environments

YOLOv5 may be run in any of the following up-to-date verified environments (with all dependencies including [CUDA](https://developer.nvidia.com/cuda)/[CUDNN](https://developer.nvidia.com/cudnn), [Python](https://www.python.org/) and [PyTorch](https://pytorch.org/) preinstalled):

- **Google Colab and Kaggle** notebooks with free GPU: <a href="https://colab.research.google.com/github/ultralytics/yolov5/blob/master/tutorial.ipynb"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"></a> <a href="https://www.kaggle.com/ultralytics/yolov5"><img src="https://kaggle.com/static/images/open-in-kaggle.svg" alt="Open In Kaggle"></a>
- **Google Cloud** Deep Learning VM. See [GCP Quickstart Guide](https://github.com/ultralytics/yolov5/wiki/GCP-Quickstart)
- **Amazon** Deep Learning AMI. See [AWS Quickstart Guide](https://github.com/ultralytics/yolov5/wiki/AWS-Quickstart)
- **Docker Image**. See [Docker Quickstart Guide](https://github.com/ultralytics/yolov5/wiki/Docker-Quickstart) <a href="https://hub.docker.com/r/ultralytics/yolov5"><img src="https://img.shields.io/docker/pulls/ultralytics/yolov5?logo=docker" alt="Docker Pulls"></a>


## Status

![CI CPU testing](https://github.com/ultralytics/yolov5/workflows/CI%20CPU%20testing/badge.svg)

If this badge is green, all [YOLOv5 GitHub Actions](https://github.com/ultralytics/yolov5/actions) Continuous Integration (CI) tests are currently passing. CI tests verify correct operation of YOLOv5 training ([train.py](https://github.com/ultralytics/yolov5/blob/master/train.py)), testing ([test.py](https://github.com/ultralytics/yolov5/blob/master/test.py)), inference ([detect.py](https://github.com/ultralytics/yolov5/blob/master/detect.py)) and export ([export.py](https://github.com/ultralytics/yolov5/blob/master/models/export.py)) on MacOS, Windows, and Ubuntu every 24 hours and on every commit.