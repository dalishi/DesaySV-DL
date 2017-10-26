# DesaySV-DL
Train on AF01 dataset on SSD


# Prepare Training Data

## 1. Download AF01 image data

Put ``xml`` files into ``~/data/AF01/dataset/Annotations`` and put image files into ``~/data/AF01/dataset/BMPImages`` respectively. ``/dataset`` is the folder name of the AF01 dataset, e.g. ``~/data/AF01/20170112/Annotations`` and ``~/data/AF01/20170112/BMPImages``.

## 2. Create image lists
The step is to create two text files": ``trainval.txt`` and ``test.txt`` which will be used to separate the dataset into training and testing, and to convert the images to ``lmdb`` format.

```shell
$ cd caffe-SSD/data/ && mkdir AF01
$ cp -r VOC0712/* AF01/
```
Open ``create_list.sh`` in ``$CAFFE_ROOT/data/AF01`` and change the following lines:
```shell

```

## 3. Convert images to lmdb

## 4. Create mean value

Image pre-processing like data normalization is one of the critical factors that affect the training process. To do data normalization, one way is to use the mean image. 

# Train

## 1. Download Network
Download the pre-trained SSD network weights from [SSD github](https://drive.google.com/open?id=0BzKzrI_SkD1_WVVTSmQxU0dVRzA), unzip and copy the file ``VGG_VOC0712_SSD_300x300_iter_120000.caffemodel`` to ``$CAFFE_ROOT/models/VGGNet``.

## 2. Modify training script

1. pre-trained model

