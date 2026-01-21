FROM almalinux:9
ENV CONDA_OVERRIDE_ARCHSPEC="haswell"
# You can specify these at build time 
ARG TARGET_PLATFORM=linux-64
ARG CONDA_ENV_FILE=https://computing.docs.ligo.org/conda/environments/$TARGET_PLATFORM/igwn-py311.yaml

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

RUN wget "${CONDA_ENV_FILE}" -O /tmp/env.yml && \
    conda env create -n kernel -f /tmp/env.yml -y && \
    rm /tmp/env.yml
SHELL ["conda", "run", "-n", "kernel", "/bin/bash", "-c"]

ENV TINI_VERSION=v0.19.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /bin/tini
RUN chmod +x /bin/tini

RUN conda install -c conda-forge davix xrootd gfal2 gfal2-util python-gfal2 -y

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
