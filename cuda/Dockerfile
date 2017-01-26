# From https://github.com/Kaixhin/dockerfiles/tree/master/cuda/cuda_v7.5, adapted for v8.0.
FROM ubuntu:14.04
MAINTAINER Kai Arulkumaran <design@kaixhin.com>

# Install wget and build-essential
RUN apt-get update && apt-get install -y \
  build-essential \
  wget

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
