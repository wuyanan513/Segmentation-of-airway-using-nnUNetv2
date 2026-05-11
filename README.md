# Segmentation of Airway Using nnU‑Net v2

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) [![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.xxxxxx.svg)](https://doi.org/10.5281/zenodo.xxxxxx)

This repository contains the code and configuration files for training and evaluating **airway tree segmentation** models with [nnU-Net v2](https://github.com/MIC-DKFZ/nnUNet). The setup is designed to work with several publicly available airway CT datasets, such as the **ATM22** and **EXACT'09** challenges. No private data or manual request to any author is required to reproduce the results.

## ✨ Highlights

* 🧠 **Self‑configuring nnU‑Net v2** – Preprocessing, network topology, and post‑processing are automatically adapted by the nnU‑Net framework.

* 📂 **Multi‑dataset training** – Harmonised pipeline for combining multiple public airway datasets.

* 📈 **Reproducible benchmarks** – Exact data splits (cross‑validation folds) and training plans used in our publication are provided.

* 💾 **Pre‑trained model weights** – Available for download through a persistent Zenodo archive (see below).

## 🗂️ Public Datasets Used

The following public datasets are used in this project. You must download them separately from their original sources. After download, organise the raw images as described in [Data Preparation](#1-data-preparation).

| Dataset                         | Description                                                                          | Access / Citation                                                        |
| ------------------------------- | ------------------------------------------------------------------------------------ | ------------------------------------------------------------------------ |
| **ATM22**                       | Airway Tree Modeling Challenge 2022, 300+ chest CTs with detailed airway annotations | [https://atm22.grand-challenge.org/](https://atm22.grand-challenge.org/) |
| **EXACT'09**                    | EXACT'09 airway extraction challenge, 40 CT scans with reference segmentations       | [https://exact09.loni.usc.edu/](https://exact09.loni.usc.edu/)           |
| *[Add other datasets you used]* | *e.g., BREATH, or any other public airway set*                                       |                                                                          |

> ⚠️ All datasets are publicly accessible. Please cite the original challenge publications when using the data.

## 🛠️ Installation

### 1. Clone the repository

```bash
git clone https://github.com/your_username/Segmentation-of-airway-using-nnUNetv2.git
cd Segmentation-of-airway-using-nnUNetv2
```

### 2. Install nnU‑Net v2

Follow the official installation instructions. In short:

```bash
pip install nnunetv2
```

Creating a dedicated conda environment is recommended.

### 3. Set environment variables

The three environment variables tell nnU‑Net where to store raw data, preprocessed data, and training results. Choose persistent locations on your machine.

```bash
export nnUNet_raw="/path/to/your/nnUNet_raw"
export nnUNet_preprocessed="/path/to/your/nnUNet_preprocessed"
export nnUNet_results="/path/to/your/nnUNet_results"
```

Add these lines to your `~/.bashrc` or `~/.zshrc` for convenience.

## 🚀 Usage

### 1. Data preparation

After downloading each dataset, place the CT images and segmentations into the nnU‑Net raw data folder. We use dataset ID `055` with the task name `AirwaySeg`. The expected directory structure is:

```
$nnUNet_raw/Dataset055_AirwaySeg/
├── dataset.json
├── imagesTr/
│   ├── ATM22_001_0000.nii.gz
│   ├── ATM22_002_0000.nii.gz
│   └── ...
├── labelsTr/
│   ├── ATM22_001.nii.gz
│   ├── ATM22_002.nii.gz
│   └── ...
└── imagesTs/          (optional, for final test set)
```

The `_0000` suffix in the image filename indicates the first (and only) modality (CT).

Segmentation files must have the same base name as the corresponding image, without the modality suffix.

The `dataset.json` file should contain the task description, modality, and label definitions. An example is provided in this repository under `config/dataset.json`. Copy it and adjust the `numTraining` and file names accordingly.

### 2. Preprocessing

```bash
nnUNetv2_plan_and_preprocess -d 055 --verify_dataset_integrity
```

This step analyses the training cases, generates the standard nnU‑Net plans (`nnUNetPlans.json`), and preprocesses the data.

### 3. Training

Train a 3D full‑resolution model on each fold separately:

```bash
nnUNetv2_train 055 3d_fullres FOLD
```

Replace `FOLD` with `0`, `1`, `2`, `3`, or `4` (the default 5‑fold cross‑validation). Model checkpoints and logs will be saved under `$nnUNet_results/Dataset055_AirwaySeg/nnUNetTrainer__nnUNetPlans__3d_fullres/fold_X`.

### 4. Inference

Run inference on new CT images (place them in an input folder, each as a `.nii.gz` file):

```bash
nnUNetv2_predict -i /path/to/input_ct_images -o /path/to/predicted_airways -d 055 -c 3d_fullres -f FOLD
```

For an ensemble of all five folds:

```bash
nnUNetv2_predict -i /path/to/input -o /path/to/output -d 055 -c 3d_fullres -f 0 1 2 3 4
```

### 5. Evaluation

We evaluate airway segmentation using common metrics (Dice coefficient, tree‑length accuracy, false positive/negative branch counts). An evaluation script is provided in `scripts/evaluate_airway.py`. Usage:

```bash
python scripts/evaluate_airway.py --pred_dir /path/to/predictions --gt_dir /path/to/ground_truth
```

This script will output a table of per‑case metrics and summary statistics. If you did not include such a script, you may simply state that evaluation follows the metrics defined in the paper and refer to standard implementations.

## 📦 Pre‑trained Model Weights

We provide the final model weights trained on the combined public datasets (ATM22 + EXACT'09) through Zenodo:

🔗 **Download model from Zenodo** [Replace the URL/DOI with the actual one you generated after uploading the model]

To reproduce the exact results from our paper:

1. Download the `Dataset055_AirwaySeg.zip` archive from Zenodo.

2. Unzip it into your `$nnUNet_results` directory:

```bash
unzip Dataset055_AirwaySeg.zip -d $nnUNet_results/
```

3. Run inference as described above – no retraining needed.

## 📄 Data Availability & Code

* All source code is freely available in this repository under the MIT License.

* Pre‑trained model weights are openly hosted on Zenodo with the DOI shown above.

* Training and evaluation data are obtained exclusively from publicly accessible datasets (ATM22, EXACT'09, etc.). The download links and citation information are listed in the Datasets section.

* No data is provided "upon request to the corresponding author" – every component required to reproduce the results is either included in this repository, on Zenodo, or at the original dataset portals.

## 📝 References

Please cite the following works if you use this code or the pre‑trained models:

1. Our associated paper (submitted, details will be updated upon acceptance).

2. Isensee, F., et al. (2021). nnU-Net: a self‑configuring method for deep learning‑based biomedical image segmentation. *Nature Methods*, 18, 203–211.

3. ATM22 Challenge: cite the challenge overview paper or the challenge website.

4. Lo, P., et al. (2012). Extraction of Airways From CT (EXACT'09). *IEEE Transactions on Medical Imaging*, 31(11), 2093–2107.

5. Add other dataset citations as appropriate.

## 📧 Contact

For code‑related questions or issues, please open an issue on this repository rather than emailing authors individually. This ensures that solutions are visible to everyone.
