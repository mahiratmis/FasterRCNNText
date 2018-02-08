## Cloning the original module
```shell 
git clone https://github.com/ruotianluo/pytorch-faster-rcnn.git
```
# RoiPooling module
```shell 
cd lib/layer_utils/roi_pooling/src/cuda
nvcc -c -o roi_pooling_kernel.cu.o roi_pooling_kernel.cu -x cu -Xcompiler -fPIC -arch=sm_60
cd ../../
python build.py
cd ../../../
```
# build NMS
```shell 
cd lib/nms/src/cuda
nvcc -c -o nms_kernel.cu.o nms_kernel.cu -x cu -Xcompiler -fPIC -arch=sm_60
cd ../../
python build.py
cd ../../
```
# install python coco api
```shell 
cd data
git clone https://github.com/pdollar/coco.git
cd coco/PythonAPI
make
cd ../../..
```
## move all files under coco to shared coco folder
```shell 
mv coco/* /SHARE/Datasets/coco
```
## create symbolic link to that coco folder
```shell 
cd data
ln -s /SHARE/Datasets/coco coco
```
## download proposals and annotation json files from 
 https://github.com/rbgirshick/py-faster-rcnn/blob/master/data/README.md
## download resnet101-caffe.pth model from 
 https://github.com/ruotianluo/pytorch-resnet

```shell 
mkdir -p data/imagenet_weights
cd data/imagenet_weights
```

## download from my gdrive (link with pytorch-resnet)
```shell 
mv resnet101-caffe.pth res101.pth
cd ../..
```

## run training
```shell 
./experiments/scripts/train_faster_rcnn.sh 1 coco res101
```

## run demo
#change the iteration number in test_faster_rcnn.sh to 
#the number of iterations you made 
```shell 
GPU_ID=0
./experiments/scripts/test_faster_rcnn.sh $GPU_ID coco res101
```
