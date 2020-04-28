# MobileNet-SSD and MobileNetV2-SSDLite with PyTorch

Object Detection with MobileNet-SSD, MobileNetV2-SSDLite on VOC, BDD100K Datasets

================================================================> (LOADING 99%...)

## Results
1. Detection

<img src="readme_images/detection_105e.jpg" width="1200">

2. View the result on [Youtube](https://www.youtube.com/watch?v=0u3f4t-Wkv4)

## Dependencies
- Python 3.6+
- OpenCV
- PyTorch
- Pyenv (optional)

## Dataset Path (optional)
The dataset path should be structured as follow:
```bashrc
|- bdd100k -- bdd100k -- images -- 100k -- train -- (70000 images)
|                     |                 |- val -- (10000 images)
|                     |
|                     |- labels -- (.json)
|                     |
|                     |- xml -- train -- (.xml)
|                            |- val -- (.xml)
|
|- MobileNets-SSD -- data -- VOCdevkit -- test -- VOC2007 -- (Annotations, ImageSets, JPEGImages,...)
     (our repo)   |                    |- VOC2007 -- (Annotations, ImageSets, JPEGImages,...)
                  |
                  |- bdd_files
                  |- images
                  |- models
                  |- ...
                  |- train_ssd_BDD.py
                  |- ssd_test_img.py
                  |- ...
@TranLeAnh
```
## Pre-processing
1. Convert BDD100K anotation format (.json) to VOC anotation format (.xml)
```bashrc
$ python bdd2voc.py
```
2. Remove training samples having no anotation (70000 to 69863)
```bashrc
notebook: remove_nolabel_samples_bdd.ipynb
```
## Download Pre-trained Models (VOC)
1. MobileNet-SSD
```bashrc
$ wget -P models https://storage.googleapis.com/models-hao/mobilenet-v1-ssd-mp-0_675.pth
```
2. MobileNetV2-SSDLite
```bashrc
$ wget -P models https://storage.googleapis.com/models-hao/mb2-ssd-lite-mp-0_686.pth
```
## Train
1. Train MobileNet-SSD (VOC)
```bashrc
$ python train_ssd_VOC.py --datasets ~/data/VOCdevkit/VOC2007/ --validation_dataset ~/data/VOCdevkit/test/VOC2007/ --net mb1-ssd --base_net models/mobilenet_v1_with_relu_69_5.pth  --batch_size 24 --num_epochs 100 --scheduler cosine --lr 0.01 --t_max 200
```
2. Train MobileNet-SSD (BDD100K)
```bashrc
$ python train_ssd_BDD.py --datasets ../bdd100k/bdd100k/images/100k/train/ --validation_dataset ../bdd100k/bdd100k/images/100k/val/ --net mb1-ssd --base_net models/mobilenet_v1_with_relu_69_5.pth  --batch_size 48 --num_epochs 200 --scheduler cosine --lr 0.01 --t_max 200
```
3. Train pretrained-model (BDD100K)
```bashrc
$ python train_ssd_BDD.py --datasets ../bdd100k/bdd100k/images/100k/train/ --validation_dataset ../bdd100k/bdd100k/images/100k/val/ --net mb1-ssd --base_net models/mobilenet_v1_with_relu_69_5.pth  --batch_size 48 --num_epochs 200 --scheduler cosine --lr 0.01 --t_max 200 --resume models/mb1-ssd-Epoch-105-Loss-inf.pth 
```
4. Train pretrained-model MobileNetV2-SSDLite (BDD100K)
```bashrc
$ python train_ssd_BDD.py --datasets ../bdd100k/bdd100k/images/100k/train/ --validation_dataset ../bdd100k/bdd100k/images/100k/val/ --net mb2-ssd-lite --pretrained_ssd models/mb2-ssd-lite-net.pth --scheduler cosine --lr 0.01 --t_max 100 --batch_size 36 --num_epochs 200 
```
## Test
1. Test on image
```bashrc
$ python ssd_test_img.py
```
2. Test on video
```bashrc
$ python ssd_test_video.py
```

## References
- https://github.com/qfgaohao/pytorch-ssd
- https://github.com/leeesangwon/bdd100k_to_VOC

April 2020

Tran Le Anh
