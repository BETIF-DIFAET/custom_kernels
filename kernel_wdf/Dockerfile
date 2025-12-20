FROM ubuntu:22.04

RUN apt-get update && \
    DEBIAN_FRONTEND=noninteractive TZ=Europe/Zurich apt-get -y -q install \
    git curl sudo wget  #ipython3.10 python3-pip 

ENV CONDA_DIR /opt/conda
RUN wget --quiet https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda.sh && \
    /bin/bash ~/miniconda.sh -b -p /opt/conda
# Put conda in path so we can use conda activate
ENV PATH=$CONDA_DIR/bin:$PATH

ENV ENV_ROOT="/opt/conda"

RUN conda init bash
RUN echo "conda activate base" >> ~/.bashrc

SHELL ["conda", "run", "-n", "base", "/bin/bash", "-c"]

RUN conda tos accept --system --override-channels --channel https://repo.anaconda.com/pkgs/main \
 && conda tos accept --system --override-channels --channel https://repo.anaconda.com/pkgs/r

RUN conda install -y python=3.11

ENV TINI_VERSION v0.19.0
ADD https://github.com/krallin/tini/releases/download/${TINI_VERSION}/tini /bin/tini
RUN chmod +x /bin/tini

RUN python3 -m pip install -U pip setuptools wheel

RUN python3 -m pip install \
    click \
    ipython \
    ipykernel

RUN apt-get -y install davix

RUN python3 -m pip install rucio-clients

RUN conda tos accept --system --override-channels --channel https://repo.anaconda.com/pkgs/main \
 && conda tos accept --system --override-channels --channel https://repo.anaconda.com/pkgs/r

RUN conda install compilers "cmake>=3.10" make git boost pybind11 fftw gsl conda-forge::libframel -c conda-forge -y


RUN git clone https://github.com/elenacuoco/p4TSA && cd p4TSA && cmake --install-prefix=/opt/conda CMakeLists.txt \
        && make -j "$(nproc)" \
        && make install \
        && cd python-wrapper \
        && python setup.py install \
        && cd .. \
        && cd .. \
        && rm -fr p4TSA

RUN /sbin/ldconfig

RUN echo "WDF installation!"
RUN git clone https://gitlab.com/wdfpipe/wdf.git &&\
        cd wdf && python setup.py install &&\
        cd .. && rm -fr wdf/



RUN echo "Install additional packages!"
RUN conda install matplotlib pandas networkx scipy scikit-learn pycbc==2.8 statsmodels PyWavelets -c conda-forge -y
RUN pip install ipysigma

ENTRYPOINT ["conda", "run", "--no-capture-output", "-n", "base"]

