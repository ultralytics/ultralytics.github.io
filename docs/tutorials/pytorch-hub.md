📚 This guide explains how to load YOLOv5 🚀 from PyTorch Hub https://pytorch.org/hub/ultralytics_yolov5

## Before You Start

Start from a **Python>=3.8** environment with **PyTorch>=1.7** installed, as well as `pyyaml>=5.3` for reading YOLOv5 configuration files. To install PyTorch see [https://pytorch.org/get-started/locally](https://pytorch.org/get-started/locally). To install YOLOv5 [requirements](https://github.com/ultralytics/yolov5/blob/master/requirements.txt):
```bash
$ pip install -r https://raw.githubusercontent.com/ultralytics/yolov5/master/requirements.txt
```

Cloning the [https://github.com/ultralytics/yolov5](https://github.com/ultralytics/yolov5) repository is **not** required 😃.

## Load YOLOv5 with PyTorch Hub

### Simple Example

This example loads a pretrained YOLOv5s model from PyTorch Hub as `model` and passes an image for inference. `'yolov5s'` is the lightest and fastest YOLOv5 model. For details on all available models please see the [README](https://github.com/ultralytics/yolov5#pretrained-checkpoints).
```python
import torch

# Model
model = torch.hub.load('ultralytics/yolov5', 'yolov5s')

# Image
img = 'https://ultralytics.com/images/zidane.jpg'

# Inference
results = model(img)
```

### Detailed Example

This example shows **batched inference** with **PIL** and **OpenCV** image sources. `results` can be **printed** to console, **saved** to `runs/hub`, **showed** to screen on supported environments, and returned as **tensors** or **pandas** dataframes.
```python
import cv2
import torch
from PIL import Image

# Model
model = torch.hub.load('ultralytics/yolov5', 'yolov5s')

# Images
for f in ['zidane.jpg', 'bus.jpg']:
    torch.hub.download_url_to_file('https://ultralytics.com/images/' + f, f)  # download 2 images
img1 = Image.open('zidane.jpg')  # PIL image
img2 = cv2.imread('bus.jpg')[:, :, ::-1]  # OpenCV image (BGR to RGB)
imgs = [img1, img2]  # batch of images

# Inference
results = model(imgs, size=640)  # includes NMS

# Results
results.print()  
results.save()  # or .show()

results.xyxy[0]  # img1 predictions (tensor)
results.pandas().xyxy[0]  # img1 predictions (pandas)
#      xmin    ymin    xmax   ymax  confidence  class    name
# 0  749.50   43.50  1148.0  704.5    0.874023      0  person
# 1  433.50  433.50   517.5  714.5    0.687988     27     tie
# 2  114.75  195.75  1095.0  708.0    0.624512      0  person
# 3  986.00  304.00  1028.0  420.0    0.286865     27     tie
```
<img src="https://user-images.githubusercontent.com/26833433/114868289-08cb7800-9df6-11eb-85ca-58545cf45a83.png" width="500">  <img src="https://user-images.githubusercontent.com/26833433/114868505-47613280-9df6-11eb-9cb0-c383358d37d2.png" width="300">

For all inference options see YOLOv5 `autoShape()` forward method:
https://github.com/ultralytics/yolov5/blob/3551b072b366989b82b3777c63ea485a99e0bf90/models/common.py#L182-L191

### Inference Settings
Inference settings such as **confidence threshold**, NMS **IoU threshold**, and **classes** filter are model attributes, and can be modified by:
```python
model.conf = 0.25  # confidence threshold (0-1)
model.iou = 0.45  # NMS IoU threshold (0-1)
model.classes = None  # (optional list) filter by class, i.e. = [0, 15, 16] for persons, cats and dogs

results = model(imgs, size=320)  # custom inference size
```

### Input Channels
To load a pretrained YOLOv5s model with 4 input channels rather than the default 3:
```python
model = torch.hub.load('ultralytics/yolov5', 'yolov5s', channels=4)
```
In this case the model will be composed of pretrained weights **except for** the very first input layer, which is no longer the same shape as the pretrained input layer. The input layer will remain initialized by random weights.

### Number of Classes
To load a pretrained YOLOv5s model with 10 output classes rather than the default 80:
```python
model = torch.hub.load('ultralytics/yolov5', 'yolov5s', classes=10)
```
In this case the model will be composed of pretrained weights **except for** the output layers, which are no longer the same shape as the pretrained output layers. The output layers will remain initialized by random weights.

### Force Reload
If you run into problems with the above steps, setting `force_reload=True` may help by discarding the existing cache and force a fresh download of the latest YOLOv5 version from PyTorch Hub.
```python
model = torch.hub.load('ultralytics/yolov5', 'yolov5s', force_reload=True)  # force reload
```

### Training
To load a YOLOv5 model for training rather than inference, set `autoshape=False`. To load a model with randomly initialized weights (to train from scratch) use `pretrained=False`.
```python
model = torch.hub.load('ultralytics/yolov5', 'yolov5s', autoshape=False)  # load pretrained
model = torch.hub.load('ultralytics/yolov5', 'yolov5s', autoshape=False, pretrained=False)  # load scratch
```

### Base64 Results
For use with API services. See https://github.com/ultralytics/yolov5/pull/2291 and [Flask REST API](https://github.com/ultralytics/yolov5/tree/master/utils/flask_rest_api) example for details.
```python
results = model(imgs)  # inference

results.imgs # array of original images (as np array) passed to model for inference
results.render()  # updates results.imgs with boxes and labels
for img in results.imgs:
    buffered = BytesIO()
    img_base64 = Image.fromarray(img)
    img_base64.save(buffered, format="JPEG")
    print(base64.b64encode(buffered.getvalue()).decode('utf-8'))  # base64 encoded image with results
```


### JSON Results
Results can be returned in JSON format once converted to `.pandas()` dataframes using the `.to_json()` method. The JSON format can be modified using the `orient` argument. See pandas `.to_json()` [documentation](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.to_json.html) for details.
```python
results = model(imgs)  # inference

results.pandas().xyxy[0].to_json(orient="records")  # JSON img1 predictions
```
<details>
  <summary>JSON Output (click to expand)</summary>

  ```json
[
{"xmin":749.5,"ymin":43.5,"xmax":1148.0,"ymax":704.5,"confidence":0.8740234375,"class":0,"name":"person"},
{"xmin":433.5,"ymin":433.5,"xmax":517.5,"ymax":714.5,"confidence":0.6879882812,"class":27,"name":"tie"},
{"xmin":115.25,"ymin":195.75,"xmax":1096.0,"ymax":708.0,"confidence":0.6254882812,"class":0,"name":"person"},
{"xmin":986.0,"ymin":304.0,"xmax":1028.0,"ymax":420.0,"confidence":0.2873535156,"class":27,"name":"tie"}
]
  ```
</details>

## Custom Models
This example loads a custom 20-class [VOC](https://github.com/ultralytics/yolov5/blob/master/data/voc.yaml)-trained YOLOv5s model `'best.pt'` with PyTorch Hub.
```python
model = torch.hub.load('ultralytics/yolov5', 'custom', path='path/to/best.pt')  # default
model = torch.hub.load('path/to/yolov5', 'custom', path='path/to/best.pt', source='local')  # local repo
```


## Environments

YOLOv5 may be run in any of the following up-to-date verified environments (with all dependencies including [CUDA](https://developer.nvidia.com/cuda)/[CUDNN](https://developer.nvidia.com/cudnn), [Python](https://www.python.org/) and [PyTorch](https://pytorch.org/) preinstalled):

- **Google Colab and Kaggle** notebooks with free GPU: <a href="https://colab.research.google.com/github/ultralytics/yolov5/blob/master/tutorial.ipynb"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"></a> <a href="https://www.kaggle.com/ultralytics/yolov5"><img src="https://kaggle.com/static/images/open-in-kaggle.svg" alt="Open In Kaggle"></a>
- **Google Cloud** Deep Learning VM. See [GCP Quickstart Guide](https://github.com/ultralytics/yolov5/wiki/GCP-Quickstart)
- **Amazon** Deep Learning AMI. See [AWS Quickstart Guide](https://github.com/ultralytics/yolov5/wiki/AWS-Quickstart)
- **Docker Image**. See [Docker Quickstart Guide](https://github.com/ultralytics/yolov5/wiki/Docker-Quickstart) <a href="https://hub.docker.com/r/ultralytics/yolov5"><img src="https://img.shields.io/docker/pulls/ultralytics/yolov5?logo=docker" alt="Docker Pulls"></a>


## Status

![CI CPU testing](https://github.com/ultralytics/yolov5/workflows/CI%20CPU%20testing/badge.svg)

If this badge is green, all [YOLOv5 GitHub Actions](https://github.com/ultralytics/yolov5/actions) Continuous Integration (CI) tests are currently passing. CI tests verify correct operation of YOLOv5 training ([train.py](https://github.com/ultralytics/yolov5/blob/master/train.py)), testing ([test.py](https://github.com/ultralytics/yolov5/blob/master/test.py)), inference ([detect.py](https://github.com/ultralytics/yolov5/blob/master/detect.py)) and export ([export.py](https://github.com/ultralytics/yolov5/blob/master/models/export.py)) on MacOS, Windows, and Ubuntu every 24 hours and on every commit.