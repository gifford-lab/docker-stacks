# From https://github.com/Kaixhin/dockerfiles/tree/master/cuda/cuda_v7.5/Dockerfile
# And https://github.com/Kaixhin/dockerfiles/blob/master/cudnn/cudnn_v7.5/Dockerfile
# Adapted for CUDA 8.0
FROM ubuntu:14.04
MAINTAINER Kai Arulkumaran <design@kaixhin.com>

# Install wget and build-essential
RUN apt-get update && apt-get install -y \
  build-essential \
  wget && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/*

# Change to the /tmp directory
RUN cd /tmp && \
# Download run file
  wget https://developer.nvidia.com/compute/cuda/8.0/prod/local_installers/cuda_8.0.44_linux-run -o /dev/null && \
# Make the run file executable and extract
  chmod +x cuda_*_linux-run && ./cuda_*_linux-run -extract=`pwd` && \
  ls && \
# Install CUDA drivers (silent, no kernel)
  ./NVIDIA-Linux-x86_64-*.run -s --no-kernel-module && \
# Install toolkit (silent)  
  ./cuda-linux64-rel-*.run -noprompt | cat > /dev/null && \
# Clean up
  rm -rf *

# Add to path
ENV PATH=/usr/local/cuda/bin:$PATH \
  LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH
RUN ldconfig /usr/local/cuda/lib64

# Install CUDA repo (needed for cuDNN)
ENV CUDA_REPO_PKG=cuda-repo-ubuntu1404_8.0.44-1_amd64.deb

RUN wget http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1404/x86_64/$CUDA_REPO_PKG && \
  dpkg -i $CUDA_REPO_PKG && \
  rm $CUDA_REPO_PKG

# Install cuDNN v4
ENV ML_REPO_PKG=nvidia-machine-learning-repo-ubuntu1404_4.0-2_amd64.deb

RUN wget http://developer.download.nvidia.com/compute/machine-learning/repos/ubuntu1404/x86_64/$ML_REPO_PKG && \
  dpkg -i $ML_REPO_PKG && \
  rm $ML_REPO_PKG && \
  apt-get update && apt-get install -y libcudnn4 libcudnn4-dev
