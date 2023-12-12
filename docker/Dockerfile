FROM pytorch/pytorch:2.0.0-cuda11.7-cudnn8-runtime

ENV DEBIAN_FRONTEND=noninteractive

# Install git, wget, and gcc
RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    build-essential git wget && \
    apt-get clean && rm -rf /var/cache/apt/archives /var/lib/apt/lists

# Older versions of the bedtools contain bug in the bedtools merge command
RUN wget "https://github.com/arq5x/bedtools2/releases/download/v2.30.0/bedtools.static.binary" && \
    chmod a+x bedtools.static.binary && \
    mv bedtools.static.binary /usr/bin/bedtools

RUN wget -qO- https://cloud.r-project.org/bin/linux/ubuntu/marutter_pubkey.asc | tee -a /etc/apt/trusted.gpg.d/cran_ubuntu_key.asc && \
    apt-get update && \
    apt-get install -y software-properties-common && \
    add-apt-repository "deb https://cloud.r-project.org/bin/linux/ubuntu $(lsb_release -cs)-cran40/" && \
    apt-get install --no-install-recommends -y r-base

RUN pip install \
    jupyterlab==3.6.3 \
    tensorboard==2.12.2 \
    pytorch-ignite==0.4.12 \
    pandas==2.0.2 \
    tqdm==4.65.0 \
    numpy==1.24.3 \
    matplotlib==3.8.2 \
    seaborn==0.12.2 \
    ipywidgets==8.0.4 \
    rpy2==3.4.2

RUN apt-get update && \
    apt-get install -y \
    curl \
    gfortran \
    libssl-dev \
    libcurl4-openssl-dev \
    liblzma-dev \
    libbz2-dev \
	libgsl0-dev \
	libncurses5-dev \
	libperl-dev \
	zlib1g-dev

RUN R -e 'install.packages(c("BiocManager", "curl", "optparse", "doParallel"), repos = "https://cloud.r-project.org",dependencies = TRUE)' && \
    R -e 'BiocManager::install("GenomicFeatures",version = "3.18",update = F)' && \
    R -e 'BiocManager::install("GenomicRanges",version = "3.18",update = F)' && \
    R -e 'BiocManager::install("rtracklayer",version = "3.18",update = F)' && \
    R -e 'BiocManager::install("regioneR",version = "3.18",update = F)'

CMD ["jupyter", "lab", "--ip=0.0.0.0", "--port=8888", "--allow-root", "--no-browser"]
EXPOSE 8888