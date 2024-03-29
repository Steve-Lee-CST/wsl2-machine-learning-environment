# Install Tensorflow-gpu on WSL2(Ubuntu-20.04 LTS)
> created: 20210130  
> last-edit: 20210130  


> ubuntu: 20.04 LTS  
> python: 3.8.5 (wsl-ubuntu-20.04 LTS default version at 20210130)  
> cuda: 11.0  
> cudnn: 8.0.4  
> tensorflow-gpu: 2.4.0  


> references:  
> [Windows Subsystem for Linux Installation Guide for Windows 10](https://docs.microsoft.com/en-us/windows/wsl/install-win10)  
> [CUDA on WSL User Guide](https://docs.nvidia.com/cuda/wsl-user-guide/index.html)  
> [Tensorflow-gpu version information](https://www.tensorflow.org/install/source#gpu_support_3)  


1. update & upgrade
```bash 
sudo apt-get update && apt-get upgrade -y
```
2. install pip
```bash
wget https://bootstrap.pypa.io/get-pip.py
sudo python3 get-pip.py
```
3. install cuda
> note: check the two link is available or not before use
```bash
apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/3bf863cc.pub
sh -c 'echo "deb http://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64 /" > /etc/apt/sources.list.d/cuda.list'
apt update
apt install -y cuda-toolkit-11-0
```
4. add cuda to path
```bash
vim ~/.bashrc
# add following two lines to the end of file and save the file
export PATH=/usr/local/cuda-11.0/bin:$PATH
export LD_LIBRARY_PATH=/usr/local/cuda-11.0/lib64:$LD_LIBRARY_PATH

source ~/.bashrc
```
5. check cuda
```bash
nvcc -V # shall output cuda-information
cd /usr/local/cuda-11.0/samples/1_Utilities/deviceQuery
make
./deviceQuery # shall output device-list with details
```
6. download cudnn
* download '**cudnn-11.0-linux-x64-v8.0.4.xx.tgz**' at: https://developer.nvidia.com/rdp/cudnn-archive , which needs nvidia-account.
7. install cudnn
```bash
tar vzxf cudnn-11.0-linux-x64-v8.0.4.xx.tgz
cp cuda/include/cudnn.h /usr/local/cuda-11.0/include
cp cuda/lib64/libcudnn* /usr/local/cuda-11.0/lib64
chmod a+r /usr/local/cuda-11.0/include/cudnn.h
chmod a+r /usr/local/cuda-11.0/lib64/libcudnn*
```
8. install tensorflow-gpu
```bash
pip install tensorflow-gpu==2.4.0
```
9. check tensorflow-gpu
```bash
python3
```
```python
import tensorflow as tf
tf.test.is_gpu_available() # shall output "True" at last
tf.config.list_physical_devices('GPU') # shall output gpu-device-info at first and device-list at last
exit()
```
10. install other machine-learning tools
