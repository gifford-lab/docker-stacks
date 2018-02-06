FROM giffordlab/minimal-notebook

MAINTAINER Matt Edwards <matted@mit.edu>

USER root

# Hacky access control for the image
RUN echo "matted ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
RUN echo "thashim ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
RUN echo "zeng ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

RUN apt-get update -qq && apt-get install -qqy software-properties-common && \
    add-apt-repository ppa:marutter/rrutter && \
    add-apt-repository ppa:staticfloat/juliareleases && \
    add-apt-repository ppa:staticfloat/julia-deps && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
    emacs24-nox \
    fonts-dejavu \
    gcc \
    gfortran \
    graphviz \
    julia \
    nano \
    wget \
    zsh \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install Python 3 packages
RUN conda install --yes \
    'ipywidgets=5.2*' \
    'pandas=0.22*' \
    'numexpr=2.6*' \
    'matplotlib=2.1*' \
    'scipy=1.0*' \
    'seaborn=0.7*' \
    'scikit-learn=0.19*' \
    'pytables=3.4.2*' \
    'sympy=1.0*' \
    'cython=0.27*' \
    'patsy=0.4*' \
    'statsmodels=0.8*' \
    'cloudpickle=0.5*' \
    'dill=0.2*' \
    'numba=0.36*' \
    'bokeh=0.12*' \
    'h5py=2.7*' \
    'theano=1.0.1' \
    biopython \
    && conda clean -yat


RUN pip --no-cache-dir install tensorflow-gpu


# Install Python 2 packages
RUN conda create -p $CONDA_DIR/envs/python2 python=2.7 \
    'ipython=5.4*' \
    'ipywidgets=5.2*' \
    'pandas=0.22*' \
    'numexpr=2.6*' \
    'matplotlib=2.1*' \
    'scipy=1.0*' \
    'seaborn=0.7*' \
    'scikit-learn=0.19*' \
    'pytables=3.4.2*' \
    'sympy=1.0*' \
    'cython=0.27*' \
    'patsy=0.4*' \
    'statsmodels=0.8*' \
    'cloudpickle=0.5*' \
    'dill=0.2*' \
    'numba=0.36*' \
    'bokeh=0.12*' \
    'h5py=2.7*' \
    'theano=0.8.2' \
    pyzmq \
    biopython \
    pydot \
	pillow \ 
    && conda clean -yat

RUN	$CONDA_DIR/envs/python2/bin/pip --no-cache-dir install tensorflow-gpu

# R packages including IRKernel which gets installed globally.
RUN conda config --add channels r
RUN conda install --yes \
    'rpy2=2.8*' \
    'r-base=3.3.2' \
    'r-irkernel=0.7*' \
    'r-plyr=1.8*' \
    'r-devtools=1.12*' \
    'r-dplyr=0.5*' \
    'r-ggplot2=2.2*' \
    'r-tidyr=0.6*' \
    'r-shiny=0.14*' \
    'r-rmarkdown=1.2*' \
    'r-forecast=7.3*' \
    'r-stringr=1.1*' \
    'r-rsqlite=1.1*' \
    'r-reshape2=1.4*' \
    'r-caret=6.0*' \
    'r-rcurl=1.95*' \
    'r-randomforest=4.6*' && conda clean -yat

RUN julia -e 'Pkg.add("IJulia")' && \
    julia -e 'Pkg.add("Gadfly")' && \
    julia -F -e 'Pkg.add("HDF5")' && \
    chmod -R go+rx $CONDA_DIR/share/jupyter

# Install deep learning packages with pip, for both Python versions.
RUN $CONDA_DIR/envs/python2/bin/pip install --upgrade --no-deps git+https://github.com/Lasagne/lasagne && \
    $CONDA_DIR/envs/python2/bin/pip install --upgrade --no-deps git+https://github.com/dnouri/nolearn@0.5 && \
    $CONDA_DIR/envs/python2/bin/pip install --upgrade --no-deps git+https://github.com/fchollet/keras@1.2.1 && \
    $CONDA_DIR/envs/python2/bin/pip install --upgrade --no-deps git+https://github.com/maxpumperla/hyperas@0.3

RUN conda install -p $CONDA_DIR/envs/python2 pytorch torchvision cuda90 -c pytorch

RUN pip install --upgrade --no-deps git+https://github.com/Lasagne/lasagne && \
    pip install --upgrade --no-deps git+https://github.com/dnouri/nolearn@0.5 && \
    pip install --upgrade --no-deps git+https://github.com/fchollet/keras@1.2.1 && \
    pip install --upgrade --no-deps git+https://github.com/maxpumperla/hyperas@0.3

RUN conda install -p $CONDA_DIR pytorch torchvision cuda90 -c pytorch

ENV THEANO_FLAGS='cuda.root=/usr/local/cuda,device=gpu,floatX=float32,lib.cnmem=0'
ENV CUDA_HOME=/usr/local/cuda
ENV LD_LIBRARY_PATH=/usr/local/cuda/lib64

RUN pip install bash_kernel && \
    python -m bash_kernel.install && \
    $CONDA_DIR/envs/python2/bin/python \
        $CONDA_DIR/envs/python2/bin/ipython \
        kernelspec install-self

USER jovyan
