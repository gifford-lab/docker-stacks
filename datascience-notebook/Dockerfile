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
    add-apt-repository ppa:octave/stable

# R pre-requisites
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    emacs24-nox \
    fonts-dejavu \
    gfortran \
    gcc \
    nano \
    wget \
    zsh \
    && apt-get clean

# Julia dependencies
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    julia \
    libnettle4 && apt-get clean

# Assorted other dependencies
RUN apt-get update -qq && apt-get install -qqy \
    graphviz \
    octave \
    octave-signal \
    scilab && apt-get clean

# Install packages globally now.
# USER jovyan

# Install Python 3 packages
RUN conda install --yes \
    'ipywidgets=4.1*' \
    'pandas=0.17*' \
    'numexpr=2.5*' \
    'matplotlib=1.5*' \
    'scipy=0.17*' \
    'seaborn=0.7*' \
    'scikit-learn=0.17*' \
    'scikit-image=0.11*' \
    'sympy=0.7*' \
    'cython=0.23*' \
    'patsy=0.4*' \
    'statsmodels=0.6*' \
    'cloudpickle=0.1*' \
    'dill=0.2*' \
    'numba=0.23*' \
    'bokeh=0.11*' \
    'h5py=2.5*' \
    biopython \
    && conda clean -yt

# Install Python 2 packages
RUN conda create -p $CONDA_DIR/envs/python2 python=2.7 \
    'ipython=4.1*' \
    'ipywidgets=4.1*' \
    'pandas=0.17*' \
    'numexpr=2.5*' \
    'matplotlib=1.5*' \
    'scipy=0.17*' \
    'seaborn=0.7*' \
    'scikit-learn=0.17*' \
    'pytables=3.2.2*' \
    'scikit-image=0.11*' \
    'sympy=0.7*' \
    'cython=0.23*' \
    'patsy=0.4*' \
    'statsmodels=0.6*' \
    'cloudpickle=0.1*' \
    'dill=0.2*' \
    'numba=0.23*' \
    'bokeh=0.11*' \
    'h5py=2.5*' \
    pyzmq \
    biopython \
    pydot \
    && conda clean -yt

# R packages including IRKernel which gets installed globally.
RUN conda config --add channels r
RUN conda install --yes \
    'rpy2=2.7*' \
    'r-base=3.2*' \
    'r-irkernel=0.5*' \
    'r-plyr=1.8*' \
    'r-devtools=1.9*' \
    'r-dplyr=0.4*' \
    'r-ggplot2=1.0*' \
    'r-tidyr=0.3*' \
    'r-shiny=0.12*' \
    'r-rmarkdown=0.8*' \
    'r-forecast=5.8*' \
    'r-stringr=0.6*' \
    'r-rsqlite=1.0*' \
    'r-reshape2=1.4*' \
    'r-nycflights13=0.1*' \
    'r-caret=6.0*' \
    'r-rcurl=1.95*' \
    'r-randomforest=4.6*' && conda clean -yt

RUN julia -e 'Pkg.add("IJulia")' && \
    julia -e 'Pkg.add("Gadfly")' && \
    julia -e 'Pkg.add("RDatasets")' && \
    julia -F -e 'Pkg.add("HDF5")' && \
    chmod -R go+rx $CONDA_DIR/share/jupyter

# Install deep learning packages with pip, for both Python versions.
RUN $CONDA_DIR/envs/python2/bin/pip install --upgrade --no-deps git+https://github.com/Theano/theano@rel-0.8.1 && \
    $CONDA_DIR/envs/python2/bin/pip install --upgrade --no-deps git+https://github.com/Lasagne/lasagne && \
    $CONDA_DIR/envs/python2/bin/pip install --upgrade --no-deps git+https://github.com/dnouri/nolearn@0.5 && \
    $CONDA_DIR/envs/python2/bin/pip install --upgrade --no-deps git+https://github.com/fchollet/keras@0.3.3 &&\
    $CONDA_DIR/envs/python2/bin/pip install --upgrade --no-deps git+https://github.com/maxpumperla/hyperas@0.1.2

RUN pip install --upgrade --no-deps git+https://github.com/Theano/theano@rel-0.8.1 && \
    pip install --upgrade --no-deps git+https://github.com/Lasagne/lasagne && \
    pip install --upgrade --no-deps git+https://github.com/dnouri/nolearn@0.5 && \
    pip install --upgrade --no-deps git+https://github.com/fchollet/keras@0.3.3

ENV THEANO_FLAGS='cuda.root=/usr/local/cuda,device=gpu,floatX=float32,lib.cnmem=0'

RUN apt-get update \
    && apt-get install -y libcudnn4 libcudnn4-dev \
    && apt-get clean

RUN pip install octave_kernel
RUN pip install bash_kernel
RUN pip install scilab_kernel

RUN python -m octave_kernel.install
RUN python -m bash_kernel.install
RUN python -m scilab_kernel.install

USER root

# Install Python 2 kernel spec globally to avoid permission problems when NB_UID
# switching at runtime.
RUN $CONDA_DIR/envs/python2/bin/python \
    $CONDA_DIR/envs/python2/bin/ipython \
    kernelspec install-self

USER jovyan
