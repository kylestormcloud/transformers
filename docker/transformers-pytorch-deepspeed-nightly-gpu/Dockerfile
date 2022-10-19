# https://docs.nvidia.com/deeplearning/frameworks/pytorch-release-notes/rel_22-04.html#rel_22-04
FROM nvcr.io/nvidia/pytorch:22.04-py3
LABEL maintainer="Hugging Face"

ARG DEBIAN_FRONTEND=noninteractive

# Example: `cu102`, `cu113`, etc.
ARG CUDA='cu116'

RUN apt -y update
RUN apt install -y libaio-dev
RUN python3 -m pip install --no-cache-dir --upgrade pip

ARG REF=main
RUN git clone https://github.com/huggingface/transformers && cd transformers && git checkout $REF

# Install **nightly** release PyTorch (flag `--pre`)
# (PyTorch must be installed before pre-compiling any DeepSpeed c++/cuda ops.)
# (https://www.deepspeed.ai/tutorials/advanced-install/#pre-install-deepspeed-ops)

# ----------------------------------------------------------------------------------------------------
# Should be re-enabled later

# RUN python3 -m pip install --no-cache-dir -U --pre torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/nightly/$CUDA

# ----------------------------------------------------------------------------------------------------
# Temporarily trick to install specific nightly version for torch 1.13 pre-release
# Should be removed later

RUN python3 -m pip install --no-cache-dir -U torch==1.13.0+$CUDA torchvision torchaudio torchtext -f https://download.pytorch.org/whl/test/$CUDA/torch_test.html

# ----------------------------------------------------------------------------------------------------

RUN python3 -m pip install --no-cache-dir ./transformers[deepspeed-testing]