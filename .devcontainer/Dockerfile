FROM ubuntu:20.04
ARG RELEASE
ARG LAUNCHPAD_BUILD_ARCH
LABEL org.opencontainers.image.ref.name=ubuntu
LABEL org.opencontainers.image.version=20.04
ADD file:4809da414c2d478b4d991cbdaa2df457f2b3d07d0ff6cf673f09a66f90833e81 in /
CMD ["/bin/bash"]
ENV NVARCH=x86_64
ENV NVIDIA_REQUIRE_CUDA=cuda>=11.3 brand=tesla,driver>=418,driver<419 driver>=450
ENV NV_CUDA_CUDART_VERSION=11.3.109-1
ENV NV_CUDA_COMPAT_PACKAGE=cuda-compat-11-3
ARG TARGETARCH
LABEL maintainer="NVIDIA CORPORATION <cudatools@nvidia.com>"
RUN |1 TARGETARCH=amd64 /bin/sh -c apt-get update && apt-get install -y --no-install-recommends gnupg2 curl ca-certificates && curl -fsSL https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/${NVARCH}/3bf863cc.pub | apt-key add - && echo "deb https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/${NVARCH} /" > /etc/apt/sources.list.d/cuda.list && apt-get purge --autoremove -y curl && rm -rf /var/lib/apt/lists/*
ENV CUDA_VERSION=11.3.1
RUN |1 TARGETARCH=amd64 /bin/sh -c apt-get update && apt-get install -y --no-install-recommends cuda-cudart-11-3=${NV_CUDA_CUDART_VERSION} ${NV_CUDA_COMPAT_PACKAGE} && rm -rf /var/lib/apt/lists/*
RUN |1 TARGETARCH=amd64 /bin/sh -c echo "/usr/local/nvidia/lib" >> /etc/ld.so.conf.d/nvidia.conf && echo "/usr/local/nvidia/lib64" >> /etc/ld.so.conf.d/nvidia.conf
ENV PATH=/usr/local/nvidia/bin:/usr/local/cuda/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
ENV LD_LIBRARY_PATH=/usr/local/nvidia/lib:/usr/local/nvidia/lib64
COPY NGC-DL-CONTAINER-LICENSE /
ENV NVIDIA_VISIBLE_DEVICES=all
ENV NVIDIA_DRIVER_CAPABILITIES=compute,utility
RUN /bin/sh -c chsh -s /bin/bash
SHELL [/bin/bash -c]
ARG UID
ARG GID
ENV DEBIAN_FRONTEND=noninteractive
RUN |2 UID= GID= /bin/bash -c apt-get update     && apt install -y python3-pyqt5     && apt-get install -y libxcb-cursor0     && apt-get install -y build-essential     && apt-get install -y sudo     && apt-get install -y git     && apt-get install -y wget     && apt-get clean     && rm -rf /var/lib/apt/lists/* # buildkit
ENV CONDA_DIR=/opt/conda
RUN |2 UID= GID= /bin/bash -c wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh &&     /bin/bash ~/miniconda.sh -b -p /opt/conda # buildkit
ENV PATH=/opt/conda/bin:/usr/local/nvidia/bin:/usr/local/cuda/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
RUN |2 UID= GID= /bin/bash -c conda install -y mamba -c conda-forge # buildkit
ADD ./environment.yml /root/environment.yml # buildkit
RUN |2 UID= GID= /bin/bash -c mamba env create --file /root/environment.yml &&    conda clean --all # buildkit
RUN |2 UID= GID= /bin/bash -c conda init bash # buildkit
RUN |2 UID= GID= /bin/bash -c echo "conda activate cp_dock"  >> ~/.bashrc # buildkit
ENV PATH=/opt/conda/envs/cp_dock/bin:/opt/conda/bin:/usr/local/nvidia/bin:/usr/local/cuda/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
ENV CONDA_DEFAULT_ENV=
RUN |2 UID= GID= /bin/bash -c conda run --no-capture-output -n cp_dock python -m pip install cellpose[gui] # buildkit
