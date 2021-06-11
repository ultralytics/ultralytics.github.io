This quickstart guide helps new users run YOLOv5 🚀 on an Amazon Web Services (AWS) Deep Learning instance ⭐. AWS offers a [Free Tier](https://aws.amazon.com/free/) and a [credit program](https://aws.amazon.com/activate/) to get started quickly and affordably. Other quickstart options for YOLOv5 include our [Colab Notebook](https://colab.research.google.com/github/ultralytics/yolov5/blob/master/tutorial.ipynb) <a href="https://colab.research.google.com/github/ultralytics/yolov5/blob/master/tutorial.ipynb"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"></a> <a href="https://www.kaggle.com/ultralytics/yolov5"><img src="https://kaggle.com/static/images/open-in-kaggle.svg" alt="Open In Kaggle"></a>, [GCP Deep Learning VM](https://github.com/ultralytics/yolov5/wiki/GCP-Quickstart) and our Docker image at https://hub.docker.com/r/ultralytics/yolov5 ![Docker Pulls](https://img.shields.io/docker/pulls/ultralytics/yolov5?logo=docker).

## 1. Console Sign-in

Create and account or sign-in to the AWS console at https://aws.amazon.com/console/ and then select the **EC2** service.
<img width="800" alt="Console" src="https://user-images.githubusercontent.com/26833433/106323804-debddd00-622c-11eb-997f-b8217dc0e975.png">


## 2. Launch Instance

In the EC2 part of the AWS console, click the **Launch instance** button.
<img width="800" alt="Launch" src="https://user-images.githubusercontent.com/26833433/106323950-204e8800-622d-11eb-915d-5c90406973ea.png">


### Choose an Amazon Machine Image (AMI)
Enter 'Deep Learning' in the search field and select the most recent Ubuntu Deep Learning AMI (recommended), or select an alternative Deep Learning AMI. See [Choosing Your DLAMI](https://docs.aws.amazon.com/dlami/latest/devguide/options.html) for more information on selecting an AMI.
<img width="800" alt="Choose AMI" src="https://user-images.githubusercontent.com/26833433/106326107-c9e34880-6230-11eb-97c9-3b5fc2f4e2ff.png">

### Select an Instance Type

A GPU instance is recommended for most deep learning purposes. Training new models will be faster on a GPU instance than a CPU instance. You can scale sub-linearly when you have multi-GPU instances or if you use distributed training across many instances with GPUs. To set up distributed training, see [Distrbuted Training](https://docs.aws.amazon.com/dlami/latest/devguide/distributed-training.html).

Note: The size of your model should be a factor in selecting an instance. If your model exceeds an instance's available RAM, select a different instance type with enough memory for your application.

* [Amazon EC2 P3 Instances](https://aws.amazon.com/ec2/instance-types/p3/) have up to 8 NVIDIA Tesla V100 GPUs.
* [Amazon EC2 P2 Instances](https://aws.amazon.com/ec2/instance-types/p2/) have up to 16 NVIDIA NVIDIA K80 GPUs.
* [Amazon EC2 G3 Instances](https://aws.amazon.com/ec2/instance-types/g3/) have up to 4 NVIDIA Tesla M60 GPUs.
* [Amazon EC2 G4 Instances](https://aws.amazon.com/ec2/instance-types/g4/) have up to 4 NVIDIA T4 GPUs.
* [Amazon EC2 P4 Instances](https://aws.amazon.com/ec2/instance-types/p4/) have up to 8 NVIDIA Tesla A100 GPUs.

Check out [EC2 Instance Types](https://aws.amazon.com/ec2/instance-types/) and choose Accelerated Computing to see the different GPU instance options.

<img width="800" alt="Choose Type" src="https://user-images.githubusercontent.com/26833433/106324624-52141e80-622e-11eb-9662-1a376d9c887d.png">

DLAMI instances provide tooling to monitor and optimize your GPU processes. For more information on overseeing your GPU processes, see [GPU Monitoring and Optimization](https://docs.aws.amazon.com/dlami/latest/devguide/tutorial-gpu.html). For pricing see [On Demand Pricing](https://aws.amazon.com/ec2/pricing/on-demand/) and [Spot Pricing](https://aws.amazon.com/ec2/spot/pricing/).


### Configure Instance Details

Amazon EC2 Spot Instances let you take advantage of unused EC2 capacity in the AWS cloud. Spot Instances are available at up to a 70% discount compared to On-Demand prices. We recommend a persistent spot instance, which will save your data and restart automatically when spot instance availability returns after spot instance termination. For full-price On-Demand instances leave these settings to their default values. 

<img width="800" alt="Spot Request" src="https://user-images.githubusercontent.com/26833433/106324835-ac14e400-622e-11eb-8853-df5ec9b16dfc.png">

Complete Steps 4-7 to finalize your instance hardware and security settings and then launch the instance.


## 3. Connect to Instance

Select the check box next to your running instance, and then click connect. You can copy paste the SSH terminal command into a terminal of your choice to connect to your instance.

<img width="800" alt="Connect" src="https://user-images.githubusercontent.com/26833433/106325530-cf8c5e80-622f-11eb-9f64-5b313a9d57a1.png">


## 4. Run YOLOv5 🚀
Once you have logged in to your instance, clone this repo and install [requirements.txt](https://github.com/ultralytics/yolov5/blob/master/requirements.txt) dependencies, including **Python>=3.8** and **PyTorch>=1.7**.

```bash
$ git clone https://github.com/ultralytics/yolov5  # clone repo
$ cd yolov5
$ pip install -r requirements.txt  # install dependencies
```

Then get started training, testing and detecting!
```bash
$ python train.py  # train a model
$ python test.py --weights yolov5s.pt  # test a model for Precision, Recall and mAP
$ python detect.py --weights yolov5s.pt --source path/to/images  # run inference on images and videos
```

## Optional Extras

Add 64GB of swap memory (to `--cache` large datasets).
```bash
sudo fallocate -l 64G /swapfile
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
free -h  # check memory
```