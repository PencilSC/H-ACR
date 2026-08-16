# H-ACR

Research code accompanying the manuscript:

**Real-Calibrated Reliability Fusion for Synthetic Image-Label Pair Selection in Left Atrial MRI Segmentation**

H-ACR stands for **Hierarchical Anatomy-Consistency Reliability**. This repository implements a post-generation framework for assessing, ranking, and selecting synthetic image-label pairs for downstream segmentation.

Repository: <https://github.com/PencilSC/H-ACR>

## Introduction

H-ACR is a framework for reliability-aware selection of synthetic medical image-label pairs for downstream segmentation. This repository contains the implementation associated with the H-ACR study, with experiments focusing on left atrial MRI segmentation.

## Environment Requirements

Python 3.10 or later is recommended. The principal code paths use:

- PyTorch
- NumPy
- SciPy
- Pillow
- `segmentation-models-pytorch`, used as an implementation dependency by selected verifier and downstream segmentation scripts.

Install a PyTorch build appropriate for your hardware, then install the remaining dependencies in an isolated environment. For example:

```bash
conda create -n hacr python=3.10
conda activate hacr
pip install numpy scipy pillow segmentation-models-pytorch
```

Install PyTorch according to the official instructions for your hardware configuration. Research scripts may require additional packages for optional analyses or figure generation. Inspect the imports of the selected script before running it.

## Project Structure

```text
H-ACR/
├── h_acrnet/
│   ├── data/          # Data processing
│   ├── metrics/       # Evaluation metrics
│   ├── models/        # Model definitions
│   └── scoring/       # Reliability scoring
├── scripts/           # Training and evaluation scripts
├── configs/           # Experiment configurations
├── manifests/         # Dataset and candidate lists
├── calibration/       # Calibration outputs
├── scores/            # Reliability scores
├── subsets/           # Selected candidate subsets
├── reports/           # Experiment reports
└── README.md
```

## Usage

Clone the repository and run commands from its root:

```bash
git clone https://github.com/PencilSC/H-ACR.git
cd H-ACR
```

The repository provides separate scripts for the main stages of the H-ACR workflow. Update local data and output paths as needed before running the scripts.

### 1. Data preparation

Prepare the dataset information and LA-specific inputs:

```bash
python scripts/build_manifest.py
python scripts/prepare_la_data.py
```

### 2. Reliability-module training

Train the shape-restoration model and the LA verifier:

```bash
python scripts/train_shape_restoration.py
python scripts/train_verifier_la.py
```

### 3. Candidate scoring

Calibrate the reliability evidence using real data and score the synthetic candidates:

```bash
python scripts/calibrate_reliability.py
python scripts/score_synthetic.py
```

### 4. Ranking and subset construction

Rank candidate pairs and construct selected subsets:

```bash
python scripts/build_subsets.py
```

### 5. Downstream evaluation

Run the downstream LA segmentation experiment:

```bash
python scripts/run_downstream.py --mode formal
```

H-ACR scoring and downstream segmentation are performed separately. The selected candidate subsets are used to augment the real training data in the downstream experiment.

## Dataset

The current experiments use the MRI component of the **Multi-Modality Whole Heart Segmentation (MM-WHS) 2017** dataset for left atrial segmentation.

Obtain the dataset from the official source and follow its access, licensing, and usage conditions.

## Citation

If you use this repository in your research, please cite:

```bibtex
@misc{hacr2026,
  title  = {Real-Calibrated Reliability Fusion for Synthetic Image-Label Pair Selection in Left Atrial MRI Segmentation},
  author = {Author Names},
  year   = {2026},
  note   = {Manuscript under review}
}
```

The citation information will be updated after the paper is formally published.

## License

A standalone repository license is not currently included. Unless a license is added, the code should not be assumed to grant permission for redistribution or reuse. Please contact the authors for clarification. Third-party libraries and datasets remain subject to their respective licenses and terms of use.
