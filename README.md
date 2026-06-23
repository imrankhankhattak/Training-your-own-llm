# TinyStories Model Training Guide

This repository contains a notebook for training a GPT-style model on the TinyStories dataset from Hugging Face.

## Overview

The project uses:
- `datasets` to load TinyStories
- `transformers` for tokenization
- `torch` for model training
- A custom GPT-style transformer model defined in the notebook

## Step-by-step Setup

1. Install a compatible Python version
   - We discovered that the default Python environment was Python 3.14, which did not support GPU-enabled PyTorch wheels on this machine.
   - We installed Python 3.12 and created a dedicated virtual environment for GPU training.
   - NOTE: You can use latest versions of Python, I used 3.12 because my GPU is older and was not detected.

2. Create and use a Python 3.12 virtual environment
   ```powershell
   py -3.12 -m venv F:\TrainingLLM\venv312
   F:\TrainingLLM\venv312\Scripts\python.exe -m pip install --upgrade pip
   ```

3. Install required Python packages in the new venv
   ```powershell
   F:\TrainingLLM\venv312\Scripts\python.exe -m pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/cu124
   F:\TrainingLLM\venv312\Scripts\python.exe -m pip install transformers datasets numpy
   ```

4. Verify GPU-enabled PyTorch
   ```powershell
   F:\TrainingLLM\venv312\Scripts\python.exe -c "import torch; print(torch.__version__); print(torch.cuda.is_available()); print(torch.version.cuda)"
   ```
   - Expected output should show a CUDA-enabled torch package, `True` for `cuda.is_available()`, and a CUDA version like `12.4`.

5. Open the notebook in VS Code and select the new kernel
   - Use `Python: Select Interpreter` and choose `F:\TrainingLLM\venv312\Scripts\python.exe`
   - Restart the notebook kernel after switching.

6. Run the notebook cells in order
   - Load the dataset
   - Tokenize the text
   - Prepare train/validation splits
   - Build the GPT model
   - Configure training with `device = 'cuda' if torch.cuda.is_available() else 'cpu'`
   - Run the training loop

## How to start training

1. Activate the Python 3.12 virtual environment:
   ```powershell
   F:\TrainingLLM\venv312\Scripts\Activate.ps1
   ```
2. Open `main.ipynb` in VS Code and select the `venv312` interpreter.
3. Confirm CUDA availability in a quick test cell:
   ```python
   import torch
   print(torch.__version__)
   print(torch.cuda.is_available())
   print(torch.cuda.get_device_name(0))
   ```
4. Run the notebook cells from top to bottom.
5. Monitor training output for loss and validation loss.
6. If training is interrupted, restart the kernel and rerun from the first cell.

## GPU Troubleshooting

### What happened

- The notebook initially used Python 3.14.
- `torch.cuda.is_available()` returned `False`, even though the system had an RTX 2080 GPU and a working NVIDIA driver.
- This happens because the installed PyTorch package was CPU-only, and Python 3.14 did not have compatible CUDA wheel support for the required PyTorch version.

### How it was fixed

- Installed Python 3.12 separately.
- Created a fresh `venv312` virtual environment using Python 3.12.
- Installed `torch==2.6.0+cu124` and related packages into that environment.
- Verified CUDA support inside the new venv with `torch.cuda.is_available()`.

## Notes

- Always use the GPU-enabled environment for training, not the CPU-only Python 3.14 environment.
- If you need to install additional libraries, do so inside `venv312`.
- If you switch kernels, restart the notebook kernel before rerunning cells.

## Useful commands

```powershell
# Activate the venv (PowerShell)
F:\TrainingLLM\venv312\Scripts\Activate.ps1

# Install additional packages
python -m pip install <package>

# Run a Python script with the venv directly
F:\TrainingLLM\venv312\Scripts\python.exe script.py
```
