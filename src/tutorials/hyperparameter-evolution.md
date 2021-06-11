📚  This guide explains **hyperparameter evolution** for YOLOv5 🚀. Hyperparameter evolution is a method of [Hyperparameter Optimization](https://en.wikipedia.org/wiki/Hyperparameter_optimization) using a [Genetic Algorithm](https://en.wikipedia.org/wiki/Genetic_algorithm) (GA) for optimization. 

Hyperparameters in ML control various aspects of training, and finding optimal values for them can be a challenge. Traditional methods like grid searches can quickly become intractable due to 1) the high dimensional search space 2) unknown correlations among the dimensions, and 3) expensive nature of evaluating the fitness at each point, making GA a suitable candidate for hyperparameter searches.


## Before You Start

Clone this repo and install [requirements.txt](https://github.com/ultralytics/yolov5/blob/master/requirements.txt) dependencies, including **Python>=3.8** and **PyTorch>=1.7**.

```bash
$ git clone https://github.com/ultralytics/yolov5  # clone repo
$ cd yolov5
$ pip install -r requirements.txt  # install dependencies
```


## 1. Initialize Hyperparameters

YOLOv5 has about 25 hyperparameters used for various training settings. These are defined in yaml files in the /data directory. Better initial guesses will produce better final results, so it is important to initialize these values properly before evolving. If in doubt, simply use the default values, which are optimized for YOLOv5 COCO training from scratch.
https://github.com/ultralytics/yolov5/blob/3bb414890a253bb1a269fb81cc275d11c8fffa72/data/hyp.scratch.yaml#L1-L33


## 2. Define Fitness

Fitness is the value we seek to maximize. In YOLOv5 we have defined a default fitness function as a weighted combination of metrics: mAP@0.5 contributes 10% of the weight and mAP@0.5:0.95 contributes the remaining 90%. You may adjust these as you see fit or use the default fitness definition.
https://github.com/ultralytics/yolov5/blob/c5d233189729a9e7e25d3aa2d347aed02b545d30/utils/general.py#L917-L921


## 3. Evolve

Evolution is performed about a base scenario which we seek to improve upon. The base scenario in this example is finetuning COCO128 for 10 epochs using pretrained YOLOv5s. The base scenario training command is:
```bash
python train.py --epochs 10 --data coco128.yaml --weights yolov5s.pt --cache
```
To evolve hyperparameters **specific to this scenario**, starting from our initial values defined in **Section 1.**, and maximizing the fitness defined in **Section 2.**, append `--evolve`:
```bash
# Single-GPU
python train.py --epochs 10 --data coco128.yaml --weights yolov5s.pt --cache --evolve

# Multi-GPU
for i in 0 1 2 3; do
  nohup python train.py --epochs 10 --data coco128.yaml --weights yolov5s.pt --cache --evolve --device $i > evolve_gpu_$i.log &
done

# Multi-GPU bash while (not recommended)
for i in 0 1 2 3; do
  nohup "$(while true; do python train.py ... --evolve --device $i; done)" > evolve_gpu_$i.log &
done
```

The default evolution settings will run the base scenario 300 times, i.e. for 300 generations:
https://github.com/ultralytics/yolov5/blob/c5d233189729a9e7e25d3aa2d347aed02b545d30/train.py#L497

The main genetic operators are **crossover** and **mutation**. In this work mutation is used, with a 90% probability and a 0.04 variance to create new offspring based on a combination of the best parents from all previous generations. Results are tracked in `yolov5/evolve.txt`, and the highest fitness offspring is saved every generation as `yolov5/runs/evolve/hyp_evolved.yaml`:
```yaml
# Hyperparameter Evolution Results
# Generations: 1000
#                   P         R     mAP.5 mAP.5:.95       box       obj       cls
# Metrics:     0.4761      0.79     0.763    0.4951   0.01926   0.03286  0.003559

lr0: 0.01  # initial learning rate (SGD=1E-2, Adam=1E-3)
lrf: 0.2  # final OneCycleLR learning rate (lr0 * lrf)
momentum: 0.937  # SGD momentum/Adam beta1
weight_decay: 0.0005  # optimizer weight decay 5e-4
warmup_epochs: 3.0  # warmup epochs (fractions ok)
warmup_momentum: 0.8  # warmup initial momentum
warmup_bias_lr: 0.1  # warmup initial bias lr
box: 0.05  # box loss gain
cls: 0.5  # cls loss gain
cls_pw: 1.0  # cls BCELoss positive_weight
obj: 1.0  # obj loss gain (scale with pixels)
obj_pw: 1.0  # obj BCELoss positive_weight
iou_t: 0.20  # IoU training threshold
anchor_t: 4.0  # anchor-multiple threshold
anchors: 0  # anchors per output grid (0 to ignore)
fl_gamma: 0.0  # focal loss gamma (efficientDet default gamma=1.5)
hsv_h: 0.015  # image HSV-Hue augmentation (fraction)
hsv_s: 0.7  # image HSV-Saturation augmentation (fraction)
hsv_v: 0.4  # image HSV-Value augmentation (fraction)
degrees: 0.0  # image rotation (+/- deg)
translate: 0.1  # image translation (+/- fraction)
scale: 0.5  # image scale (+/- gain)
shear: 0.0  # image shear (+/- deg)
perspective: 0.0  # image perspective (+/- fraction), range 0-0.001
flipud: 0.0  # image flip up-down (probability)
fliplr: 0.5  # image flip left-right (probability)
mosaic: 1.0  # image mosaic (probability)
mixup: 0.0  # image mixup (probability)
```

We recommend a **minimum** of 300 generations of evolution for best results. Note that **evolution is generally expensive and time consuming**, as the base scenario is trained hundreds of times, possibly requiring hundreds or thousands of GPU hours.


## 4. Visualize

Results are saved as `yolov5/evolve.png`, with one plot per hyperparameter. Values are on the x axis and fitness on the y axis. Yellow indicates higher concentrations. Vertical lines indicate that a parameter has been disabled and does not mutate. This is user selectable in the `meta` dictionary in train.py, and is useful for fixing parameters and preventing them from evolving.

![evolve](https://user-images.githubusercontent.com/26833433/89130469-f43e8e00-d4b9-11ea-9e28-f8ae3622516d.png)


## Environments

YOLOv5 may be run in any of the following up-to-date verified environments (with all dependencies including [CUDA](https://developer.nvidia.com/cuda)/[CUDNN](https://developer.nvidia.com/cudnn), [Python](https://www.python.org/) and [PyTorch](https://pytorch.org/) preinstalled):

- **Google Colab and Kaggle** notebooks with free GPU: <a href="https://colab.research.google.com/github/ultralytics/yolov5/blob/master/tutorial.ipynb"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"></a> <a href="https://www.kaggle.com/ultralytics/yolov5"><img src="https://kaggle.com/static/images/open-in-kaggle.svg" alt="Open In Kaggle"></a>
- **Google Cloud** Deep Learning VM. See [GCP Quickstart Guide](https://github.com/ultralytics/yolov5/wiki/GCP-Quickstart)
- **Amazon** Deep Learning AMI. See [AWS Quickstart Guide](https://github.com/ultralytics/yolov5/wiki/AWS-Quickstart)
- **Docker Image**. See [Docker Quickstart Guide](https://github.com/ultralytics/yolov5/wiki/Docker-Quickstart) <a href="https://hub.docker.com/r/ultralytics/yolov5"><img src="https://img.shields.io/docker/pulls/ultralytics/yolov5?logo=docker" alt="Docker Pulls"></a>


## Status

![CI CPU testing](https://github.com/ultralytics/yolov5/workflows/CI%20CPU%20testing/badge.svg)

If this badge is green, all [YOLOv5 GitHub Actions](https://github.com/ultralytics/yolov5/actions) Continuous Integration (CI) tests are currently passing. CI tests verify correct operation of YOLOv5 training ([train.py](https://github.com/ultralytics/yolov5/blob/master/train.py)), testing ([test.py](https://github.com/ultralytics/yolov5/blob/master/test.py)), inference ([detect.py](https://github.com/ultralytics/yolov5/blob/master/detect.py)) and export ([export.py](https://github.com/ultralytics/yolov5/blob/master/models/export.py)) on MacOS, Windows, and Ubuntu every 24 hours and on every commit.