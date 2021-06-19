---
title: Deep Learning & AI frameworks
titleSuffix: Azure Data Science Virtual Machine 
description: Available deep learning frameworks and tools on Azure Data Science Virtual Machine.
keywords: data science tools, data science virtual machine, tools for data science, linux data science
services: machine-learning
ms.service: data-science-vm
ms.custom: devx-track-python

author: timoklimmer
ms.author: tklimmer
ms.topic: conceptual
ms.date: 05/12/2021
---

# Deep learning and AI frameworks for the Azure Data Science VM
Deep learning frameworks on the DSVM are listed below.


## [CUDA, cuDNN, NVIDIA Driver](https://developer.nvidia.com/cuda-toolkit)

| Category | Value |
|--|--|
| Version(s) supported | 11 |
| Supported DSVM editions | Windows Server 2019<br>Ubuntu 18.04 |
| How is it configured / installed on the DSVM? | _nvidia-smi_ is available on the system path. |
| How to run it | Open a command prompt (on Windows) or a terminal (on Linux), and then run _nvidia-smi_. |
## [Horovod](https://github.com/uber/horovod)

| Category | Value |
| ------------- | ------------- |
| Version(s) supported | 0.21.3|
| Supported DSVM editions      | Ubuntu 18.04 |
| How is it configured / installed on the DSVM?  | Horovod is installed in Python 3.5 |
| How to run it      | Activate the correct environment at the terminal, and then run Python. |


## [NVidia System Management Interface (nvidia-smi)](https://developer.nvidia.com/nvidia-system-management-interface)

| Category | Value |
|--|--|
| Version(s) supported |  |
| Supported DSVM editions | Windows Server 2019<br>Ubuntu 18.04 |
| What is it for? | NVIDIA tool for querying GPU activity |
| How is it configured / installed on the DSVM? | `nvidia-smi` is on the system path. |
| How to run it | On a virtual machine **with GPU's**, open a command prompt (on Windows) or a terminal (on Linux), and then run `nvidia-smi`. |

## [PyTorch](https://pytorch.org/)

| Category | Value |
|--|--|
| Version(s) supported | 1.8.1 (Ubuntu 18.04, Windows 2019) |
| Supported DSVM editions | Windows Server 2019<br>Ubuntu 18.04 |
| How is it configured / installed on the DSVM? | Installed in Python, conda environment 'py38_pytorch' |
| How to run it | Terminal: Activate the correct environment, and then run Python.<br/>* [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine): Connect, and then open the PyTorch directory for samples. |

## [TensorFlow](https://www.tensorflow.org/)

| Category | Value |
|--|--|
| Version(s) supported | 2.5 |
| Supported DSVM editions | Windows Server 2019<br>Ubuntu 18.04 |
| How is it configured / installed on the DSVM? | Installed in Python, conda environment 'py38_tensorflow' |
| How to run it | Terminal: Activate the correct environment, and then run Python. <br/> * Jupyter: Connect to [Jupyter](provision-vm.md) or [JupyterHub](dsvm-ubuntu-intro.md#how-to-access-the-ubuntu-data-science-virtual-machine), and then open the TensorFlow directory for samples. |
