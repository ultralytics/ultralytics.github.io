YOLOv5 🚀 applies online imagespace and colorspace augmentations in the trainloader (but not the val_loader) to present a new and unique augmented [Mosaic](https://github.com/ultralytics/yolov5/blob/90b7895d652c3bd3d361b2d6e9aee900fd67f5f7/utils/datasets.py#L678-L732) (original image + 3 random images) each time an image is loaded for training. **Images are never presented twice in the same way**.

![YOLOv5 augmentation](https://user-images.githubusercontent.com/26833433/120995721-f3cfed00-c785-11eb-8ee2-b6ef2fa205e8.jpg)

### Augmentation Hyperparameters

The hyperparameters used to define these augmentations are in your hyperparameter file (default `data/hyp.scratch.yaml`) defined when training:
```
python train.py --hyp hyp.scratch-low.yaml
```
https://github.com/ultralytics/yolov5/blob/b94b59e199047aa8bf2cdd4401ae9f5f42b929e6/data/hyps/hyp.scratch-low.yaml#L6-L34

### Augmentation Previews

You can view the effect of your augmentation policy in your train_batch*.jpg images once training starts. These images will be in your train logging directory, typically `yolov5/runs/train/exp`:

`train_batch0.jpg` shows train batch 0 mosaics and labels:  
<img src="https://user-images.githubusercontent.com/26833433/131255960-b536647f-7c61-4f60-bbc5-cb2544d71b2a.jpg" width="800">


### YOLOv5 Albumentations Integration

[YOLOv5](https://github.com/ultralytics/yolov5) 🚀 is now fully integrated with [Albumentations](https://github.com/albumentations-team/albumentations), a popular open-source image augmentation package. Now you can train the world's best Vision AI models even better with custom Albumentations 😃! 

PR https://github.com/ultralytics/yolov5/pull/3882 implements this integration, which will automatically apply Albumentations transforms during YOLOv5 training if `albumentations>=1.0.3` is installed in your environment. See https://github.com/ultralytics/yolov5/pull/3882 for full details.

Example `train_batch0.jpg` on COCO128 dataset with Blur, MedianBlur and ToGray. See the YOLOv5 [Notebooks](https://github.com/ultralytics/yolov5/blob/master/tutorial.ipynb) to reproduce: <a href="https://colab.research.google.com/github/ultralytics/yolov5/blob/master/tutorial.ipynb"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"></a> <a href="https://www.kaggle.com/ultralytics/yolov5"><img src="https://kaggle.com/static/images/open-in-kaggle.svg" alt="Open In Kaggle"></a>

<img src="https://user-images.githubusercontent.com/26833433/124400879-ff331b80-dd25-11eb-9b67-fe85ac4ca104.jpg" width="800">

Good luck 🍀 and let us know if you have any other questions!