# DLYear2Sem3 - Environment Setup and Workflow

This README documents the **full setup workflow** for the `DLYear2Sem3` project. Following these steps allows you to replicate the environment on any Windows machine with NVIDIA GPUs (tested on RTX 3070 and RTX 2050) and maintain consistency between local Jupyter and Google Colab.

---

## 1\. Prerequisites

* **Windows 10/11** (or Linux/Mac with minor adjustments)
* **Anaconda / Miniconda** installed
* **Git** installed
* **NVIDIA GPU drivers** installed for CUDA (RTX 3070/2050 compatible)
* Optional: Visual Studio Build Tools if compiling PyTorch extensions

---

## 2\. Clone the GitHub Repository

Open **Git Bash**:

```bash
# Navigate to the folder where you want the project
cd ~/Projects  # or C:\\\\Projects

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

## 3\. Create Conda Environment

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

## 4\. Install PyTorch with GPU Support

```bash
# Install PyTorch + CUDA (compatible with both RTX 3070 and RTX 2050)
conda install pytorch torchvision torchaudio pytorch-cuda=11.8 -c pytorch -c nvidia -y
```

### Verify GPU:

```python
import torch
print(torch.cuda.is\\\_available())
print(torch.cuda.get\\\_device\\\_name(0))
```

---

## 5\. Install Other Dependencies

```bash
# Core ML libraries
conda install -y jupyter matplotlib tqdm pillow scikit-learn pandas

# Kaggle dataset downloader
pip install kagglehub
```

---

## 6\. Register Jupyter Kernel

```bash
python -m ipykernel install --user --name DLSem3 --display-name "DLSem3"
```

* Launch Jupyter Notebook:

```bash
jupyter notebook
```

* Select **DLSem3** kernel for your notebooks.

---

## 7\. Folder Structure Management

To preserve empty folders (`data/`, `outputs/`, `src/`) in GitHub:

```bash
# Add .gitkeep to all empty folders
find . -type d -empty -exec touch {}/.gitkeep \\\\;

# Commit and push
git add .
git commit -m "Add .gitkeep for empty folders"
git push origin main
```

---

## 8\. Dataset Management

* Project-relative path for dataset:

```
DLYear2Sem3/data/flowers102
```

* Colab / Jupyter code snippet:

```python
import os
import kagglehub
import shutil

PROJECT\\\_ROOT = "/content/DLYear2Sem3"  # adjust path locally
DATA\\\_DIR = os.path.join(PROJECT\\\_ROOT, "data/flowers102")
os.makedirs(DATA\\\_DIR, exist\\\_ok=True)

if not os.listdir(DATA\\\_DIR):  # folder empty
    tmp\\\_path = kagglehub.dataset\\\_download("waseemalastal/the-oxford-flowers-102-dataset")
    shutil.copytree(tmp\\\_path, DATA\\\_DIR, dirs\\\_exist\\\_ok=True)

print("Dataset ready at:", DATA\\\_DIR)
```

> This ensures \\\*\\\*Colab and local Jupyter use the same paths\\\*\\\*.

---

## 9\. Syncing With GitHub

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

## 10\. Launch Training / Notebooks

1. Open notebook in Jupyter or Colab.
2. Select kernel `DLSem3`.
3. Use project-relative paths:

```python
train\\\_dir = os.path.join(DATA\\\_DIR, "train")
val\\\_dir = os.path.join(DATA\\\_DIR, "valid")
```

4. Run training or evaluation scripts as usual.

---

## 11\. Optional: Mount Google Drive (for Colab)

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
\\\_\\\_pycache\\\_\\\_/
\\\*.pt
\\\*.pth
```

---

This README now documents **everything from cloning to environment setup, dataset management, and syncing**, so you can replicate your workflow on any new machine.

```

Perfect! ✅

Now your Git Bash session is correctly using the DLSem3 Conda environment. You can tell because your prompt now shows:

(DLSem3) User@DESKTOP-6RTFFJ0 MINGW64 /c/Projects/DLSem3\\\_Assignment1 (main)

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


# 20-10-2025, aFreshNewStart.ipynb update

Yes, here is a comprehensive evaluation of the improvements made between the two notebooks, formatted in Markdown for your `README.md` file.



````markdown

\\# Evaluation of Model Improvements (v1 vs. v2)



This document outlines the key improvements made between the initial model script (`Assignment1\\\_Jupyter.ipynb`) and the refined, robust training pipeline (`aFreshNewStart.ipynb`). The goal was to move from a basic implementation to a professional-grade workflow that enhances model performance, prevents overfitting, and provides deeper insights into the training process.



The new methodology proved successful, \\\*\\\*increasing the peak validation accuracy from ~92.5% to 95.1%\\\*\\\* and establishing a much stronger foundation for future experiments.



---



\\## Key Improvements Implemented



\\### 1. Data Augmentation \\\& Preprocessing 🚀



The most significant change was the introduction of a sophisticated data preprocessing pipeline using `torchvision.transforms`.



\\#### Before (`v1`): 

A single, simple transform was used for all datasets. This approach had several drawbacks:

\\\* `transforms.Resize((224, 224))` can distort images by forcing their aspect ratio, making the model learn from warped features.

\\\* \\\*\\\*No normalization was applied\\\*\\\*, which is a critical issue for transfer learning. The pre-trained ResNet50 model expects its input to be normalized with ImageNet statistics.

\\\* Using the same transform for training and validation meant the model was not being trained to be robust against variations.



```python

\\# v1 transform (Assignment1\\\_Jupyter.ipynb)

transform = transforms.Compose(\\\[

\&nbsp;   transforms.Resize((224, 224)),

\&nbsp;   transforms.ToTensor(),

])

````



\#### After (`v2`):



Separate, specialized transforms were created for training and validation/testing.



```python

\\# v2 training transform (aFreshNewStart.ipynb)

transform\\\_train\\\_data = transforms.Compose(\\\[

\&nbsp;   transforms.RandomRotation(30),

\&nbsp;   transforms.RandomResizedCrop(224), # Prevents distortion and forces focus on different parts

\&nbsp;   transforms.RandomHorizontalFlip(),

\&nbsp;   transforms.ColorJitter(brightness=0.2, contrast=0.2, saturation=0.2, hue=0.1),

\&nbsp;   transforms.ToTensor(),

\&nbsp;   transforms.Normalize(\\\[0.485, 0.456, 0.406], \\\[0.229, 0.224, 0.225]) # Crucial for transfer learning

])



\\# v2 validation/test transform (deterministic)

transform\\\_valid\\\_test\\\_data = transforms.Compose(\\\[

\&nbsp;   transforms.Resize(255),

\&nbsp;   transforms.CenterCrop(224), # Preserves aspect ratio

\&nbsp;   transforms.ToTensor(),

\&nbsp;   transforms.Normalize(\\\[0.485, 0.456, 0.406], \\\[0.229, 0.224, 0.225])

])

```



  \* \*\*Benefits:\*\*

      \* \*\*Data Augmentation\*\* on the training set (random rotations, crops, flips, color jitter) artificially expands the dataset, making the model more robust and significantly reducing overfitting.

      \* \*\*Proper Resizing (`Resize` + `CenterCrop`)\*\* for validation preserves the image's aspect ratio, providing a more accurate and fair evaluation.

      \* \*\*Normalization\*\* is now correctly applied to all data, ensuring the model receives input in the format it was trained on.



---



\### 2\\. Dynamic Learning Rate Scheduler 📈



To achieve better and more stable convergence, a learning rate scheduler was introduced.



  \* \*\*Before (`v1`):\*\* A fixed `LEARNING\\\_RATE` was used for all 30 epochs. This can cause the model to converge too slowly in later stages or overshoot the best possible solution.

  \* \*\*After (`v2`):\*\* A `CosineAnnealingLR` scheduler was implemented. This scheduler starts with the initial learning rate and smoothly decreases it over the 100 epochs. This allows the model to make large progress in the early stages and then make very fine, precise adjustments in the later stages, leading to a better final result.



---



\### 3\\. Enhanced Training \& Analysis Workflow 📊



The entire workflow for training, checkpointing, and analysis was upgraded for better insights and reproducibility.



  \* \*\*Training Accuracy Tracking:\*\* The new training loop now calculates and logs `train\\\_acc` in addition to `val\\\_acc`. Plotting both on the same graph is the best way to visually identify the onset of overfitting.



  \* \*\*Enhanced Checkpointing:\*\* The saved checkpoint is no longer just the model's weights. It is now a self-contained dictionary that includes:



      \* The model's `state\\\_dict`.

      \* The optimizer's `state\\\_dict`.

      \* The `epoch` number.

      \* The complete `history` dictionary (losses, accuracies, learning rate).

      \* The `hyperparameters` used for the run.

      \* Class mappings (`class\\\_to\\\_idx`, `cat\\\_to\\\_name`).



  \* \*\*Robust Analysis Workflow:\*\* The post-training analysis cells were consolidated and now \*\*load all necessary information directly from the best checkpoint file\*\*. This allows analysis to be run at any time after training without losing the results in the notebook session. The workflow now generates:



      \* A full training log (`training\\\_log\\\_fresh.csv`) for external analysis.

      \* Loss and Accuracy plots.

      \* A \*\*Confusion Matrix\*\* to visualize exactly which flower classes the model is confusing with others, providing clear direction for further improvements.



  \* \*\*Resume-from-Checkpoint Logic:\*\* The training cell was upgraded with a `try...except` block that automatically detects if a checkpoint file exists. This allows training to be seamlessly resumed from the last saved epoch, preserving the model's state, optimizer, and history.



---



\## Results \& Performance Analysis



The new training pipeline was run for \*\*100 epochs\*\*, and the results clearly demonstrate the benefits of the improved methodology.



| Metric | Old Model (`v1`, 30 Epochs) | New Model (`v2`, 100 Epochs) |

| :--- | :--- | :--- |

| \*\*Best Validation Accuracy\*\* | 92.54% | \*\*95.11%\*\* |

| \*\*Epoch of Best Accuracy\*\* | 30 | \*\*75\*\* |



The refined model achieved a significantly higher peak validation accuracy.



\### Training \& Validation Curves (`v2` Model)



The following plots were generated from the `history` object saved in the best checkpoint file.



\### Analysis of Curves:



1\.  \*\*Clear Learning:\*\* Both the training and validation loss consistently decrease, while the accuracies consistently rise, indicating the model is learning effectively from the data.

2\.  \*\*Impact of Augmentation:\*\* The training accuracy (orange) is consistently higher than the validation accuracy (green). This gap is expected and is a direct result of data augmentation making the training task harder. The widening gap in later epochs also indicates that the model is beginning to overfit.

3\.  \*\*Proof of Checkpointing Value:\*\* The model reached its \*\*peak validation accuracy of 95.11% at epoch 75\*\*. After this point, the validation accuracy plateaus and even slightly degrades, while the training accuracy continues to climb. This is classic overfitting behavior and proves that our strategy of saving only the \*best\* checkpoint was crucial. Without it, we would have ended up with a slightly worse model at epoch 100.



---



\## Conclusion



The transition from the initial script to the refined pipeline was highly successful. The implementation of data augmentation, a learning rate scheduler, and a robust analysis workflow directly contributed to a higher-performing and better-generalizing model. The final `best\\\_model\\\_checkpoint\\\_fresh.pth` file represents a strong baseline, and the new workflow provides all the necessary tools to efficiently conduct further experiments, such as implementing \*\*two-stage fine-tuning\*\* or \*\*early stopping\*\*.



```

```

