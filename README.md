# Steps to Install Pytorch on Jetson Nano and Run YoloV5

## Identify the jetpack on jetson nano 
> Refer: https://stackoverflow.com/questions/72359090/how-to-find-the-jetpack-version-of-nvidia-jetson-device
```
sudo apt-cache show nvidia-jetpack
```
- **_Note:_** Incase above step doesn't work, run following commands.
```
git clone https://github.com/jetsonhacks/jetsonUtilities.git
cd jetsonUtilities
python jetsonInfo.py
```
## Install torch
> Referred:
>
> a) https://forums.developer.nvidia.com/t/pytorch-for-jetson-version-1-11-now-available/72048 
>
> b) https://pytorch.org/blog/running-pytorch-models-on-jetson-nano/
>
> c) https://github.com/Qengineering/PyTorch-Jetson-Nano
- Download torch corresponding to the matching jetpack from https://forums.developer.nvidia.com/t/pytorch-for-jetson-version-1-11-now-available/72048.
- Run following commands to install torch (Eg., Assuming torch-1.9.0-cp36-cp36m-linux_aarch64.whl is downloaded in previous step):
```
sudo apt-get install python3-pip libopenblas-base libopenmpi-dev 
pip3 install Cython
pip3 install numpy torch-1.9.0-cp36-cp36m-linux_aarch64.whl
```
## Install torchvision 
> Referred: torchvision sub-section under Installation section on https://forums.developer.nvidia.com/t/pytorch-for-jetson-version-1-11-now-available/72048.
- Run following commands to install torchvision
```
sudo apt-get install libjpeg-dev zlib1g-dev libpython3-dev libavcodec-dev libavformat-dev libswscale-dev
git clone --branch <version> https://github.com/pytorch/vision torchvision   # see below for version of torchvision to download
cd torchvision
export BUILD_VERSION=0.x.0  # where 0.x.0 is the torchvision version  
python3 setup.py install --user
cd ../   
```
## Check the version of torch and torchvision:
```
pip3 list|grep torch
```
## To run yolov5 on Jetson Nano:
> Referred: https://pytorch.org/blog/running-pytorch-models-on-jetson-nano/
- Download the yolov5 repo
```
git clone https://github.com/ultralytics/yolov5
cd yolov5
pip install -r requirements.txt
```
- Run following command to test the yolov5 (Assuming yolov5s.pt model is there)
```
python3 detect.py
```
> Note:
>
> a) If following error `ImportError: The _imagingft C module is not installed.` prompts, then reinstall pillow:
> ```
> sudo apt-get install libpng-dev
> sudo apt-get install libfreetype6-dev
> pip3 uninstall pillow
> pip3 install --no-cache-dir pillow
> ```
>
> b) Yolov5 will run on torch versions higher than 1.6 as torch.cuda.amp is introduced in 1.6 only. Eg., torch=1.4 is compatable On jetpack 4.2. However, we can not run yolov5 with this.
