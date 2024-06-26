# FROM nvidia/cuda:11.3.1-base-ubuntu20.04
FROM nvidia/cuda:12.4.1-base-ubuntu20.04
# Image with cellpose installed and GUI working. Access to NVIDIA GPU.

# set bash as current shell
RUN chsh -s /bin/bash
SHELL ["/bin/bash", "-c"]

# Install base utilities
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get update \
    && apt-get install -y python3-pyqt5 \
    && apt-get install -y libxcb-cursor0 \
    && apt-get install -y build-essential \
    && apt-get install -y git \
    && apt-get install -y wget \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

# Install miniconda
ENV CONDA_DIR /opt/conda
RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh && \
    /bin/bash ~/miniconda.sh -b -p /opt/conda

# Put conda in path so we can use conda activate
ENV PATH=$CONDA_DIR/bin:$PATH

# Install mamba
RUN conda install -y mamba -c conda-forge

# Create conda environment
ADD ./environment.yml /media/environment.yml
ADD ./Dockerfile /media/Dockerfile
RUN mamba env create --file /media/environment.yml \
    && conda clean --all

RUN conda init bash
RUN echo "conda activate cp_dock"  >> ~/.bashrc
ENV PATH /opt/conda/envs/cp_dock/bin:$PATH
ENV CONDA_DEFAULT_ENV cp_dock

# Install pytorch and dep
RUN pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu121
RUN conda install -y pytorch torchvision torchaudio pytorch-cuda=12.1 -c pytorch -c nvidia
RUN conda install -y https://conda.anaconda.org/pytorch/linux-64/pytorch-2.3.0-py3.8_cuda12.1_cudnn8.9.2_0.tar.bz2
# RUN conda install -y pyg -c pyg
# RUN pip install --no-index torch-sparse -f https://pytorch-geometric.com/whl/torch-2.3.0+cu121.html
RUN pip install --no-index torch-scatter -f https://pytorch-geometric.com/whl/torch-2.3.0+cu121.html
RUN pip install torch_geometric && pip install pyg_lib torch_scatter torch_sparse torch_cluster torch_spline_conv -f https://data.pyg.org/whl/torch-2.3.0+cu121.html

# Install ImageAnalysis
RUN git clone -b main2.0_dev https://github.com/BennyGinger/ImageAnalysis_pipeline.git ./ImageAnalysis
WORKDIR /ImageAnalysis
RUN pip install -e .



# Set up user (in linux this allow to modify the files/folders created by the container in the host machine)
# ARG UID
# ARG GID
# RUN addgroup --gid $GID cpdev && \
#     adduser --uid $UID --gid $GID --disabled-password --gecos "" cpdev && \
#     echo "cpdev ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers
# USER cpdev

# Build with: docker build . --build-arg UID=$(id -u) --build-arg GID=$(id -g) -t cpdev:ben2
# Run with: docker run -it --gpus all --name cp_ben -h imaginganalysis  -e DISPLAY=${DISPLAY} -v /tmp/.X11-unix:/tmp/.X11-unix -v "${PWD}:/home" cpdev:ben
