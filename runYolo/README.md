# yoloLocalRun

## For running YOlO locally (Ubuntu 22.04)

For running just download the correct dataset from Roboflow (or from anywhere in the correct format).
Place it in the `runYolo` folder.

Apart from the requirements list, it is also needed to have installed NVIDIA drivers, CUDA toolkit, cuDNN, and PyTorch.

For installing all of these in Ubuntu 22.04, use the following instructions:

```bash
### Steps ###
# Verify the system has a CUDA-capable GPU
# Download and install the NVIDIA CUDA toolkit and cuDNN
# Setup environmental variables
# Verify the installation

### To verify your GPU is CUDA enabled, check:
lspci | grep -i nvidia

### If you have a previous installation, remove it first:
sudo apt purge nvidia* -y
sudo apt remove nvidia-* -y
sudo rm /etc/apt/sources.list.d/cuda*
sudo apt autoremove -y && sudo apt autoclean -y
sudo rm -rf /usr/local/cuda*

# System update
sudo apt update && sudo apt upgrade -y

# Install other important packages
sudo apt install g++ freeglut3-dev build-essential libx11-dev libxmu-dev libxi-dev libglu1-mesa libglu1-mesa-dev

# First, get the PPA repository driver
sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt update

# Find recommended driver versions for you
ubuntu-drivers devices

# Install NVIDIA driver with dependencies (replace xxx with recommended driver)
sudo apt install libnvidia-common-xxx libnvidia-gl-xxx nvidia-driver-xxx -y

# Reboot
sudo reboot now

# Verify that the following command works
nvidia-smi

# Install CUDA toolkit
# Go to the following link and install the runfile:
# https://developer.nvidia.com/cuda-11-8-0-download-archive?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=22.04

# Installing CUDA-11.8
sudo apt install cuda-11-8 -y

# Setup your paths
echo 'export PATH=/usr/local/cuda-11.8/bin:$PATH' >> ~/.bashrc
echo 'export LD_LIBRARY_PATH=/usr/local/cuda-11.8/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
source ~/.bashrc
sudo ldconfig

# Install cuDNN v11.8
# First register here: https://developer.nvidia.com/developer-program/signup

CUDNN_TAR_FILE="cudnn-linux-x86_64-8.7.0.84_cuda11-archive.tar.xz"
sudo wget https://developer.download.nvidia.com/compute/redist/cudnn/v8.7.0/local_installers/11.8/cudnn-linux-x86_64-8.7.0.84_cuda11-archive.tar.xz
sudo tar -xvf ${CUDNN_TAR_FILE}
sudo mv cudnn-linux-x86_64-8.7.0.84_cuda11-archive cuda

# Copy the following files into the CUDA toolkit directory
sudo cp -P cuda/include/cudnn.h /usr/local/cuda-11.8/include
sudo cp -P cuda/lib/libcudnn* /usr/local/cuda-11.8/lib64/
sudo chmod a+r /usr/local/cuda-11.8/lib64/libcudnn*

# Finally, to verify the installation, check
nvidia-smi
nvcc -V

# Install PyTorch
# https://pytorch.org/get-started/locally/