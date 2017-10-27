# DesaySV-DL
Train on AF01 dataset on [SSD](https://github.com/weiliu89/caffe/tree/ssd)<sup>[1]</sup>


# Prepare Training Data

## 1. Download AF01 image data

Put ``xml`` files into ``~/data/AF01/"dataset"/Annotations`` and put image files into ``$HOME/data/AF01/"dataset"/BMPImages`` respectively. ``/dataset`` is the folder name of the AF01 dataset, e.g. ``$HOME/data/AF01/20170112/Annotations`` and ``~/data/AF01/20170112/BMPImages``.

## 2. Create image lists
This step is to create three text files: ``trainval.txt``, ``test.txt``, and ``test_name_size.txt`` in ``$CAFFE_ROOT/data/AF01/20170112``. The dataset has been separated into trainval and test.

```shell
$ cd $CAFFE_ROOT/data/ && mkdir AF01
$ cp -r VOC0712/* AF01/
```
Contact me for the files ``create_list.sh`` and ``create_data.sh`` and replace them in ``$CAFFE_ROOT/data/AF01``.

```shell
cd $CAFFE_ROOT/data/AF01
./create_list.sh
```

## 3. Convert images to lmdb
Create lmdb files for trainval and test with encoded original image and make soft links at ``$CAFFE_ROOT/examples/AF01_20170112/``. The generated database files will be in ``$HOME/data/AF01/20170112/lmdb/AF01_20170112_trainval_lmdb`` and ``$HOME/data/AF01/20170112/lmdb/AF01_20170112_test_lmdb``.

```shell
./create_data.sh
```

## 4. Create mean value

Image pre-processing like data normalization is one of the critical factors that affect the training process. To do data normalization, one way is to use the mean image. 

# Train

## 1. Download Network
Download the pre-trained SSD network weights from [SSD github](https://drive.google.com/open?id=0BzKzrI_SkD1_WVVTSmQxU0dVRzA), unzip and copy the file ``VGG_VOC0712_SSD_300x300_iter_120000.caffemodel`` to ``$CAFFE_ROOT/models/VGGNet``.

## 2. Modify training script

1. pre-trained model

<sup>[1]</sup> [SSD(SSD: Single Shot MultiBox Detector)](https://github.com/weiliu89/caffe/tree/ssd)
