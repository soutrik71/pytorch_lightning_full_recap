Setup: Poetry environment for PyTorch Lightning + LLMs (macOS / MPS)

This repository contains a `pyproject.toml` with common packages for deep learning and LLM training. On macOS you should install the PyTorch wheel that enables MPS support inside the Poetry virtualenv (instructions below).

1) Prerequisites

- Homebrew (optional, recommended for installing Python):

    $ /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
    $ brew update
    $ brew install python@3.11

- Install Poetry (if you don't have it):

    $ curl -sSL https://install.python-poetry.org | python3 -
    $ export PATH="$HOME/.local/bin:$PATH"

2) Create & activate the Poetry environment

From the repository root:

    $ cd $(pwd)
    $ poetry env use $(which python3)   # ensure Poetry uses the system/python you want
    $ poetry install                    # installs dependencies from pyproject.toml (excluding torch if you prefer custom wheel)
    $ poetry shell                       # or use `poetry run <cmd>` without entering the shell

3) Install PyTorch (MPS) inside the Poetry env

Notes:
- PyTorch wheels and recommended install flags change over time. If you want the most reliable command, visit https://pytorch.org/get-started/locally and pick macOS / pip / your Python option.
- In many recent setups `pip install torch torchvision torchaudio` installs an MPS-capable wheel for macOS Apple Silicon.

From inside the poetry shell (recommended):

    $ python -m pip install --upgrade pip
    $ python -m pip install --upgrade "torch" torchvision torchaudio

If you run into wheel availability issues, use the command from the official site. Example (may change):

    $ python -m pip install --pre --extra-index-url https://download.pytorch.org/whl/nightly/cpu torch torchvision torchaudio --upgrade

4) Register a Jupyter kernel for this Poetry environment

Run (from repo root or inside `poetry shell`):

    $ poetry run python -m ipykernel install --user --name=pytorch-lightning-full-recap --display-name "Poetry: pytorch_lightning_full_recap"

Then open JupyterLab and select the kernel `Poetry: pytorch_lightning_full_recap`.

    $ poetry run jupyter lab

5) Verify PyTorch + MPS

In a Python REPL or notebook cell (using the new kernel):

    >>> import torch
    >>> print('torch', torch.__version__)
    >>> print('mps available:', getattr(torch.backends, 'mps', None) and torch.backends.mps.is_available())

6) Notes & known caveats

- `bitsandbytes` and some CUDA-accelerated packages are not supported on macOS; install them only on Linux/GPU hosts.
- If you encounter IPython extension errors (e.g., `%autoreload` issues on Python 3.12), upgrade `ipython` inside the poetry env:

    $ poetry run pip install -U ipython

If you want, I can add a small `scripts` helper to automate these steps.
