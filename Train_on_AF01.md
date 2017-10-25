# DesaySV-DL
Train on AF01 dataset on SSD


# Prepare Training Data

## 1. Download AF01 image data

Put ``xml`` files into ``~/data/AF01/Annotations`` and put image files into ``~/data/AF01/BMPImages`` respectively.

## 2. Convert images to lmdb

## 3. Create mean value

Image pre-processing like data normalization is one of the critical factors that affect the training process. To do data normalization, one way is to use the mean image. 

# Train

## 1. Download Network
Download the pre-trained SSD network weights from [SSD github](https://drive.google.com/open?id=0BzKzrI_SkD1_WVVTSmQxU0dVRzA), unzip and copy the file ``VGG_VOC0712_SSD_300x300_iter_120000.caffemodel`` to ``$CAFFE_ROOT/models/VGGNet``.

## 2. Modify training script

1. pre-trained model

