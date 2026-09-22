# kernel-wdflow

A Jupyter kernel for [wdflow](https://github.com/elenacuoco/wdflow) (WDF: Wavelet Detection Filter), a
real-time trigger-generation pipeline for transient time-series signals, together with the downstream
analysis that turns raw triggers into candidate events (clustering, multi-detector coincidence,
background/false-alarm-probability, ROC analysis, sky localisation).

Built on top of the platform's `kernel-default` base image.

## What's installed

- `wdflow[pipeline,data,mock,tutorials]` from PyPI:
  - `pipeline` -- `py4tsa`, the compiled trigger-generation core
  - `data` -- `gwpy`, for reading/fetching detector strain
  - `mock` -- `pycbc`, for injections and mock background
  - `tutorials` -- `jupyter`/`nbclient`/`ipykernel`, to run the project's tutorial notebooks
- CPU-only `torch` and `torch_geometric`, for the GNN-based coincidence classifier
- `jupyterlab`

## Usage

Once this PR merges, the image is published via CVMFS at:

```
/cvmfs/unpacked.cern.ch/ghcr.io/betif-difaet/kernel-wdflow:latest
```

Register this path through the Kernelspec Manager's Template icon (enter the path and select
"Create Kernelspec").
