# Guide to installing TensorFlow with GPU Support on Ubuntu 16.04

1. Install NVIDIA Drivers and restart your computer
```bash
$ sudo add-apt-repository ppa:graphics-drivers/ppa
$ sudo apt-get update
$ sudo ubuntu-drivers autoinstall
$ reboot
```
After restarting go to
```
System Settings -> Software & Updates -> Additional Drivers
```
select ***Using NVIDIA - version 361.42 from nvidia-361***

2. Download and install CUDA Toolkit<br>
[Download from here](https://developer.nvidia.com/cuda-toolkit) <br>
```bash
Because the toolkit uses an older driver, select **No** to driver.
Install NVIDIA Accelerated Graphics Driver for Linux-x86_64 352.39? ((y)es/(n)o/(q)uit): N
Install the CUDA 7.5 Toolkit? ((y)es/(n)o/(q)uit): Y
Enter Toolkit Location [ default is /usr/local/cuda-7.5 ]:
you want to install a symbolic link at /usr/local/cuda? ((y)es/(n)o/(q)uit): Y
Install the CUDA 7.5 Samples? ((y)es/(n)o/(q)uit): N
```
Then, wait for installation to complete. Ignore the warning that you will get for the drivers at the end of the installation.

3. Download cuDNN 7.5 <br>
[Download from here](https://developer.nvidia.com/rdp/cudnn-download) <br>
Extract and rename the cude directory as cudnn.
```bash
$ mv cuda cudnn
$ sudo cp cudnn /usr/local
```
Then create symlinks in /usr/local/cuda/lib64 to /usr/local/cudnn/lib64
```bash
$ sudo ln -s /usr/local/cuda/lib64/libcudnn.so /usr/local/cudnn/lib64/libcudnn.so
$ sudo ln -s /usr/local/cuda/lib64/libcudnn.so.4 /usr/local/cudnn/lib64/libcudnn.so.4
$ sudo ln -s /usr/local/cuda/lib64/libcudnn.so.4.0.7 /usr/local/cudnn/lib64/libcudnn.so.4.0.7
$ sudo ln -s /usr/local/cuda/lib64/libcudnn_static.a /usr/local/cudnn/lib64/libcudnn_static.a
```

4. Set up bashrc
```bash
$ sudo nano ~/.bashrc
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/local/cuda/lib64:/usr/local/cudnn/lib64
export CUDA_HOME=/usr/local/cuda
export PATH=/usr/local/cuda/bin:$PATH
```

5. Add environment variables
```bash
$ sudo nano /etc/profile.d/cuda.sh
export PATH=$PATH:/usr/local/cuda/bin
$ sudo nano /etc/ld.so.conf.d/cuda.conf
/usr/local/cuda/lib64
$ sudo nano /etc/ld.so.conf.d/cudnn.conf
/usr/local/cudnn/lib64
$ sudo ldconfig
$ source ~/.bashrc
```

6. Force it to work with gcc 5 <br>
```bash
$ sudo nano /usr/local/cuda/include/host_config.h
line: 115 comment out error
//#error -- unsupported GNU version! gcc versions later than 4.9 are not supported!
```

7. Verify your driver and installation
```
$ nvidia-smi
$ nvcc -V
$ which nvcc
```

8. Download Anaconda for either Python 2.7 or 3.5 (I will use 2.7)
```
[Download from here](https://www.continuum.io/downloads)
$ bash Anaconda2-4.1.1-Linux-x86_64.sh
```

9. Download TensorFlow and create a TensorFlow environment using Python 2.7
```
$ conda create -n tensorflow python=2.7
$ wget https://storage.googleapis.com/tensorflow/linux/gpu/tensorflow-0.10.0-cp27-none-linux_x86_64.whl
```

10. Finally start the TensorFlow environment
```
$ source activate tensorflow
(tensorflow) $ pip install --ignore-installed --upgrade tensorflow-0.10.0-cp27-none-linux_x86_64.whl
```
To exit the env
```
(tensorflow) $ source deactivate
```
