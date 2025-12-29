FROM almalinux:9

RUN yum -y clean metadata \
    && yum install -y dnf-plugins-core \
    && yum -y install epel-release git wget python39 python3-pip python3-devel boost-python3 jq procps \
    && yum -y clean all

RUN yum search voms
RUN yum -y install voms-clients-cpp \
    && yum -y clean all

WORKDIR /

ENV CONDA_DIR=/opt/conda

RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-py311_25.3.1-1-Linux-x86_64.sh -O ~/miniconda.sh && \
    /bin/bash ~/miniconda.sh -b -p /opt/conda

# Put conda in path so we can use conda activate
ENV PATH=$CONDA_DIR/bin:$PATH

RUN conda init bash
RUN echo "conda activate kernel" >> ~/.bashrc

RUN conda create -n kernel -y
SHELL ["conda", "run", "-n", "kernel", "/bin/bash", "-c"]

RUN conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/main \
 && conda tos accept --override-channels --channel https://repo.anaconda.com/pkgs/r

ENV TINI_VERSION=v0.19.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /bin/tini
RUN chmod +x /bin/tini

RUN conda install -c conda-forge davix==0.8.6 xrootd==5.5.5 gfal2==2.21.3 gfal2-util==1.9.0 python-gfal2==1.12.0 -y

RUN python3 -m pip install rucio==37.7.1 rucio-clients==37.7.1 

RUN python3 -m pip install -U pip setuptools wheel

RUN python3 -m pip install \
    click \
    ipython \
    ipykernel \
    ipywidgets \
    matplotlib

RUN conda clean -a -y

ENTRYPOINT ["conda", "run", "--no-capture-output", "-n", "kernel"]