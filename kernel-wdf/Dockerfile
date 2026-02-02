FROM ghcr.io/betif-difaet/kernel-igwn:latest

ENV ENV_ROOT="/opt/conda"

RUN dnf config-manager --set-enabled crb && \
    dnf install -y --nodocs --setopt=install_weak_deps=False \
        gcc gcc-c++ gcc-gfortran cmake make git \
        boost-devel pybind11-devel fftw-devel gsl-devel && \
    dnf clean all && \
    rm -rf /var/cache/dnf/*

RUN conda install conda-forge::pybind11 conda-forge::libframel
RUN /sbin/ldconfig

RUN git clone https://github.com/elenacuoco/p4TSA && cd p4TSA && cmake CMakeLists.txt \
    && make -j "$(nproc)" \
    && make install \
    && cd python-wrapper \
    && python setup.py install \
    && cd ../.. \
    && rm -fr p4TSA

RUN /sbin/ldconfig

RUN git clone https://gitlab.com/wdfpipe/wdf.git && \
    cd wdf && python setup.py install && \
    cd .. && rm -fr wdf/

RUN /sbin/ldconfig
