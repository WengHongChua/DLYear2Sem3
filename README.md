# Flower Species Classification with ResNet50 (Oxford Flowers-102)

Transfer learning in PyTorch to classify **102 flower species** from the [Oxford Flowers-102 dataset](https://www.kaggle.com/datasets/waseemalastal/the-oxford-flowers-102-dataset) (8,189 images). Team coursework for the Deep Learning module, Taylor's University (2025).

## Results

| Version | Training setup | Best validation accuracy |
| --- | --- | --- |
| v1 (`notebooks/Assignment1_Jupyter_saved 1.ipynb`) | Fixed learning rate, 30 epochs, no augmentation or normalisation | 92.54% |
| v2 (`aFreshNewStart.ipynb`) | Augmentation, ImageNet normalisation, cosine annealing LR, 100 epochs | **95.11%** (epoch 75) |

The v2 pipeline raised validation accuracy by 2.6 points. Validation accuracy peaked at epoch 75 and plateaued while training accuracy kept rising, so the best checkpoint (not the last) is the one kept.

## What changed from v1 to v2

- **Data augmentation:** random rotation, `RandomResizedCrop`, horizontal flip and colour jitter on the training set; resize + centre crop for validation and test, so images are not distorted.
- **ImageNet normalisation**, which the pre-trained ResNet50 expects.
- **Cosine annealing** learning-rate schedule instead of a fixed rate.
- **Full checkpoints** (model, optimiser, epoch, history, hyperparameters, class mapping) with automatic resume-from-checkpoint.
- **Analysis from the checkpoint:** loss and accuracy curves, a training log CSV and a confusion matrix of which species get confused.

## Files

| File | Contents |
| --- | --- |
| `Bravo_4_Image_Recognition.ipynb` | Final submitted notebook |
| `aFreshNewStart.ipynb` | v2 training pipeline |
| `notebooks/Assignment1_Jupyter_saved 1.ipynb` | v1 baseline |
| `Evaluation.ipynb` | Evaluation and predictions on custom images |
| `ExplanationOfImprovements.ipynb` | Write-up of v1 vs v2 |
| `*.png`, `training_log_fresh.csv` | Confusion matrices, prediction grids and the training log |

## Run it

```bash
git clone https://github.com/WengHongChua/DLYear2Sem3.git
cd DLYear2Sem3
conda create -n DLSem3 python=3.12 -y
conda activate DLSem3
conda install pytorch torchvision torchaudio pytorch-cuda=11.8 -c pytorch -c nvidia -y
conda install -y jupyter matplotlib tqdm pillow scikit-learn pandas
pip install kagglehub
jupyter lab
```

The notebooks download the dataset into `data/flowers102` with `kagglehub` on first run. Tested on an NVIDIA RTX 3070 locally and on Google Colab.

## Stack

Python, PyTorch, torchvision, scikit-learn, pandas, Matplotlib, Jupyter, Google Colab.
