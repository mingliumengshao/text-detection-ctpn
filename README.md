# text-detection-ctpn(for CPU version)

  The original repo: please click the link [here](https://github.com/eragonruan/text-detection-ctpn)

***
# setup
- requirements: tensorflow1.8.0, cython0.28.2, opencv-python, easydict,(recommend to install Anaconda)
- if you do not have a gpu device, follow here to [setup](https://github.com/eragonruan/text-detection-ctpn/issues/43) or you can try as follows.

This is my way:      
(0)download the checkpoints from release, unzip it in xxx/text-detection-ctpn-master/checkpoints/  

(1) Set "USE_GPU_NMS " in the file ./ctpn/text.yml as "False"                

(2) Set the "__C.USE_GPU_NMS" in the file ./lib/fast_rcnn/config.py as "False";    

(3) Comment out the line "from lib.utils.gpu_nms import gpu_nms" in the file ./lib/fast_rcnn/nms_wrapper.py;    

(4) In this repo, I have replace the original setup.py with new setup.py.    

(5)run:            
python setup.py build_ext --include-dirs={your-numpy-include-path}               
for example my path is:               
/usr/local/lib/python2.7/dist-packages/numpy/core/include                  
so:             
python setup.py build_ext --include-dirs=/usr/local/lib/python2.7/dist-packages/numpy/core/include

(6) by execute (5), a new "build" directory will be create. Open the "build" directory and copy the .so file from the "build" directory to the xxx/text-detection-ctpn-master/lib/utils. 

(7) cd xxx/text-detection-ctpn-master
and execute: python ./ctpn/demo.py

now, you will see the processed demo results under directory: xxx/text-detection-ctpn-master/data/results 
***
# parameters
there are some parameters you may need to modify according to your requirement, you can find them in ctpn/text.yml
- USE_GPU_NMS # whether to use nms implemented in cuda or not
- DETECT_MODE # H represents horizontal mode, O represents oriented mode, default is H
- checkpoints_path # the model I provided is in checkpoints/, if you train the model by yourself, it will be saved in output/
***
# training
## prepare data
- First, download the pre-trained model of VGG net and put it in data/pretrain/VGG_imagenet.npy. you can download it from [google drive](https://drive.google.com/open?id=0B_WmJoEtfQhDRl82b1dJTjB2ZGc) or [baidu yun](https://pan.baidu.com/s/1kUNTl1l). 
- Second, prepare the training data as referred in paper, or you can download the data I prepared from [google drive](https://drive.google.com/open?id=0B_WmJoEtfGhDRl82b1dJTjB2ZGc) or [baidu yun](https://pan.baidu.com/s/1kUNTl1l). 

- then, extract the data (VOCdevkit),and put it in data/VOCdevkit. execute:              
ln -s VOCdevkit VOCdevkit2007
- the tensorflow version that original repo used is 1.3.0, but my tensorflow version is 1.8.0, so you have to change gen_logging_ops._image_summary(...) to 
gen_logging_ops.image_summary(...) in the file ./lib/fast_rcnn/train.py. 

if not, you will get a bug report about "gen_logging_ops._image_summary(...)".


- train 
Simplely run
```shell
python ./ctpn/train_net.py
```
- you can modify some hyper parameters in ctpn/text.yml, or just used the parameters I set.
- The model I provided in checkpoints is trained on GTX1070 for 50k iters.
- If you are using cuda nms, it takes about 0.2s per iter. So it will takes about 2.5 hours to finished 50k iterations.
***

***
# some results
`NOTICE:` all the photos used below are collected from the internet. If it affects you, please contact me to delete them.
<img src="/data/results/001.jpg" width=320 height=240 /><img src="/data/results/002.jpg" width=320 height=240 />
<img src="/data/results/003.jpg" width=320 height=240 /><img src="/data/results/004.jpg" width=320 height=240 />
<img src="/data/results/009.jpg" width=320 height=480 /><img src="/data/results/010.png" width=320 height=320 />
***
## oriented text connector
- oriented text connector has been implemented, i's working, but still need futher improvement.
- left figure is the result for DETECT_MODE H, right figure for DETECT_MODE O
<img src="/data/results/007.jpg" width=320 height=240 /><img src="/data/oriented_results/007.jpg" width=320 height=240 />
<img src="/data/results/008.jpg" width=320 height=480 /><img src="/data/oriented_results/008.jpg" width=320 height=480 />
***
