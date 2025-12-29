FROM ghcr.io/betif-difaet/kernel-default:latest

# Install prerequisites for WDF
ENV ENV_ROOT="/opt/conda/envs/kernel/"
RUN conda install compilers "cmake>=3.10" make git boost pybind11 fftw gsl conda-forge::libframel -c conda-forge -y

RUN echo "Install additional packages!"
RUN conda install matplotlib pandas networkx scipy scikit-learn pycbc==2.8 statsmodels PyWavelets -c conda-forge -y
RUN pip install ipysigma

# Install p4TSA
RUN git clone https://github.com/elenacuoco/p4TSA && cd p4TSA && cmake --install-prefix=$ENV_ROOT CMakeLists.txt \
        && make -j "$(nproc)" \
        && make install \
        && cd python-wrapper \
        && python setup.py install \
        && cd .. \
        && cd .. \
        && rm -fr p4TSA

RUN /sbin/ldconfig

# Install WDF
RUN git clone https://gitlab.com/wdfpipe/wdf.git &&\
        cd wdf && python setup.py install &&\
        cd .. && rm -fr wdf/

RUN conda clean -a -y