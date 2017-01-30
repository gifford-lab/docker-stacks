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
    'pandas=0.19*' \
    'numexpr=2.6*' \
    'matplotlib=1.5*' \
    'scipy=0.17*' \
    'seaborn=0.7*' \
    'scikit-learn=0.18*' \
    'pytables=3.2.2*' \
    'sympy=1.0*' \
    'cython=0.23*' \
    'patsy=0.4*' \
    'statsmodels=0.6*' \
    'cloudpickle=0.1*' \
    'dill=0.2*' \
    'numba=0.30*' \
    'bokeh=0.11*' \
    'h5py=2.6*' \
    'theano=0.8.2' \
    biopython \
    && conda clean -yat

# Install Python 2 packages
RUN conda create -p $CONDA_DIR/envs/python2 python=2.7 \
    'ipython=4.2*' \
    'ipywidgets=5.2*' \
    'pandas=0.19*' \
    'numexpr=2.6*' \
    'matplotlib=1.5*' \
    'scipy=0.17*' \
    'seaborn=0.7*' \
    'scikit-learn=0.17*' \
    'pytables=3.2.2*' \
    'sympy=1.0*' \
    'cython=0.23*' \
    'patsy=0.4*' \
    'statsmodels=0.6*' \
    'cloudpickle=0.1*' \
    'dill=0.2*' \
    'numba=0.30*' \
    'bokeh=0.11*' \
    'h5py=2.6*' \
    'theano=0.8.2' \
    pyzmq \
    biopython \
    pydot \
    && conda clean -yat

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

RUN pip install --upgrade --no-deps git+https://github.com/Lasagne/lasagne && \
    pip install --upgrade --no-deps git+https://github.com/dnouri/nolearn@0.5 && \
    pip install --upgrade --no-deps git+https://github.com/fchollet/keras@1.2.1 && \
    pip install --upgrade --no-deps git+https://github.com/maxpumperla/hyperas@0.3

ENV THEANO_FLAGS='cuda.root=/usr/local/cuda,device=gpu,floatX=float32,lib.cnmem=0'

RUN pip install bash_kernel && \
    python -m bash_kernel.install && \
    $CONDA_DIR/envs/python2/bin/python \
        $CONDA_DIR/envs/python2/bin/ipython \
        kernelspec install-self

USER jovyan
