# A custom Jupyter kernel image for ET-BETIF (https://betif-difaet.readthedocs.io),
# built on top of the platform's own base kernel image as required by:
# https://betif-difaet.readthedocs.io/en/latest/user_guide.html#create-your-own-custom-singularity-kernel-image
#
# Build:
#   docker build -f Dockerfile.betif-kernel -t wdflow-betif-kernel .
#
# Push to a registry the ET-BETIF JupyterHub can reach, then from a JupyterLab
# terminal on ET-BETIF pull it into a Singularity image:
#   singularity pull docker://<YOUR_CONTAINER_REGISTRY>/wdflow-betif-kernel:latest
#
# Move the resulting .sif to persistent storage, then register it as a kernel
# through the Kernelspec Manager's Template icon (enter the absolute path to
# the .sif and select "Create Kernelspec"). See the user guide section above
# for the alternative of contributing this Dockerfile to
# https://github.com/BETIF-DIFAET/custom_kernels instead.
FROM ghcr.io/betif-difaet/kernel-default:v1.2.0

# The CPU build of torch: the default one from PyPI carries CUDA, which an
# image without a GPU never uses (mirrors the WITH_TORCH branch in Dockerfile).
RUN python3 -m pip install --no-cache-dir "torch>=2.1" --index-url https://download.pytorch.org/whl/cpu \
    && python3 -m pip install --no-cache-dir "torch_geometric>=2.5"

# wdflow[pipeline,data,mock,tutorials]:
#   pipeline  -> py4tsa, the compiled trigger-generation core
#   data      -> gwpy, for reading/fetching detector strain
#   mock      -> pycbc, for injections and mock background
#   tutorials -> jupyter/nbclient/ipykernel, to run tutorials/ notebooks
RUN python3 -m pip install --no-cache-dir "wdflow[pipeline,data,mock,tutorials]" jupyterlab
