
# DLYear2Sem3 - Environment Setup and Workflow

This README documents the **full setup workflow** for the `DLYear2Sem3` project. Following these steps allows you to replicate the environment on any Windows machine with NVIDIA GPUs (tested on RTX 3070 and RTX 2050) and maintain consistency between local Jupyter and Google Colab.

---

## 1. Prerequisites

- **Windows 10/11** (or Linux/Mac with minor adjustments)
- **Anaconda / Miniconda** installed
- **Git** installed
- **NVIDIA GPU drivers** installed for CUDA (RTX 3070/2050 compatible)
- Optional: Visual Studio Build Tools if compiling PyTorch extensions

---

## 2. Clone the GitHub Repository

Open **Git Bash**:

```bash
# Navigate to the folder where you want the project
cd ~/Projects  # or C:\Projects

# Clone the repo
git clone https://github.com/WengHongChua/DLYear2Sem3.git

# Enter the project folder
cd DLYear2Sem3

# Verify folder structure
ls
````

You should see:

```
data/  notebooks/  outputs/  src/
```

> Note: Empty folders may not appear on GitHub unless `.gitkeep` is added.

---

## 3. Create Conda Environment

Open **Git Bash** or **Anaconda Prompt**:

```bash
# Correct syntax (no spaces around =)
conda create -n DLSem3 python=3.12 -y

# Activate environment
eval "$(C:/Users/User/anaconda3/Scripts/conda.exe shell.bash hook)"  # for Git Bash
conda activate DLSem3
```

> Your prompt should now show `(DLSem3)`.

---

## 4. Install PyTorch with GPU Support

```bash
# Install PyTorch + CUDA (compatible with both RTX 3070 and RTX 2050)
conda install pytorch torchvision torchaudio pytorch-cuda=11.8 -c pytorch -c nvidia -y
```

### Verify GPU:

```python
import torch
print(torch.cuda.is_available())
print(torch.cuda.get_device_name(0))
```

---

## 5. Install Other Dependencies

```bash
# Core ML libraries
conda install -y jupyter matplotlib tqdm pillow scikit-learn pandas

# Kaggle dataset downloader
pip install kagglehub
```

---

## 6. Register Jupyter Kernel

```bash
python -m ipykernel install --user --name DLSem3 --display-name "DLSem3"
```

* Launch Jupyter Notebook:

```bash
jupyter notebook
```

* Select **DLSem3** kernel for your notebooks.

---

## 7. Folder Structure Management

To preserve empty folders (`data/`, `outputs/`, `src/`) in GitHub:

```bash
# Add .gitkeep to all empty folders
find . -type d -empty -exec touch {}/.gitkeep \;

# Commit and push
git add .
git commit -m "Add .gitkeep for empty folders"
git push origin main
```

---

## 8. Dataset Management

* Project-relative path for dataset:

```
DLYear2Sem3/data/flowers102
```

* Colab / Jupyter code snippet:

```python
import os
import kagglehub
import shutil

PROJECT_ROOT = "/content/DLYear2Sem3"  # adjust path locally
DATA_DIR = os.path.join(PROJECT_ROOT, "data/flowers102")
os.makedirs(DATA_DIR, exist_ok=True)

if not os.listdir(DATA_DIR):  # folder empty
    tmp_path = kagglehub.dataset_download("waseemalastal/the-oxford-flowers-102-dataset")
    shutil.copytree(tmp_path, DATA_DIR, dirs_exist_ok=True)

print("Dataset ready at:", DATA_DIR)
```

> This ensures **Colab and local Jupyter use the same paths**.

---

## 9. Syncing With GitHub

Inside Git Bash or Anaconda Prompt:

```bash
# Pull latest changes
git pull origin main

# After editing files
git add .
git commit -m "Your commit message"
git push origin main
```

---

## 10. Launch Training / Notebooks

1. Open notebook in Jupyter or Colab.
2. Select kernel `DLSem3`.
3. Use project-relative paths:

```python
train_dir = os.path.join(DATA_DIR, "train")
val_dir = os.path.join(DATA_DIR, "valid")
```

4. Run training or evaluation scripts as usual.

---

## 11. Optional: Mount Google Drive (for Colab)

```python
from google.colab import drive
drive.mount('/content/drive')
```

* You can store datasets or outputs persistently.
* Sync with GitHub as usual.

---

## ✅ Notes

* All commands are tested on **Windows + RTX 3070 / 2050**.
* Folder structure is fully reproducible on Colab and local Jupyter.
* Keep `data/` and `outputs/` in `.gitignore` to avoid pushing large files.

```
data/
outputs/
__pycache__/
*.pt
*.pth
```

---

This README now documents **everything from cloning to environment setup, dataset management, and syncing**, so you can replicate your workflow on any new machine.

```

Perfect! ✅

Now your Git Bash session is correctly using the DLSem3 Conda environment. You can tell because your prompt now shows:

(DLSem3) User@DESKTOP-6RTFFJ0 MINGW64 /c/Projects/DLSem3_Assignment1 (main)

Next Steps in This Environment

Install PyTorch with GPU support (works for both RTX 3070 and RTX 2050):


----
1. You don’t need to install notebook separately

Your pip packages include ipykernel and jupyter-client, which are enough to run notebooks.

You can launch a notebook with:

conda activate DLSem3
jupyter notebook


If it opens successfully, you’re fine; no need to force notebook=7.4.5 installation, which previously failed.

2. About CUDA / PyTorch

You have pytorch=2.5.1 with pytorch-cuda=11.8, which matches your GPU setup (assuming CUDA 11.8 compatible GPU).

torchaudio and torchvision are included via pip — they match your PyTorch version.

3. ipykernel is included

Pip has ipykernel==7.0.0. This is sufficient to register the kernel:

python -m ipykernel install --user --name DLSem3 --display-name "DLSem3"


After that, you should see DLSem3 as a kernel in Jupyter Notebook or Lab.

4. Pip vs Conda

Most Python packages are pip-installed in your YAML. That’s fine, but it means conda env create -f environment.yml might mix pip and conda installs.

Conda will handle CUDA, MKL, numpy, scipy, pandas etc., and pip will handle everything else. This is normal and expected for environments with PyTorch + GPU + pip-only packages.

5. Launch order

Activate conda environment: conda activate DLSem3

Launch jupyterlab : jupyter lab

Select kernel DLSem3 for your notebooks.

No further installations should be needed unless you add new packages for a project.
