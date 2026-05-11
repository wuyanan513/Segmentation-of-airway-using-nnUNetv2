# Segmentation of Airway Using nnU-Net v2

This repository provides the code and configuration for training and evaluating **airway tree segmentation** models based on [nnU-Net v2](https://github.com/MIC-DKFZ/nnUNet). It is designed to work seamlessly with multiple public **airway tree datasets**, such as **ATM22**, **QIN Lung CT**, and others, enabling robust and reproducible airway analysis.

## ✨ Highlights

- 🧠 **Plug-and-play nnU-Net v2** – Leverages the self‑configuring framework to automatically adapt preprocessing, network topology, and post‑processing.
- 📂 **Multi‑dataset support** – Prebuilt setup for harmonised training/inference across several widely‑used public airway datasets.
- 📈 **Reproducible benchmarks** – Includes the exact fold splits and training plans used in our publication.
- 💾 **Pre‑trained model weights** – Available through a public repository with a persistent DOI (see below).

## 🗂️ Datasets

The following public datasets are used in this project. You must download them separately from their respective sources and place the raw images under `data/raw/` as described in the usage section.

| Dataset | Description | Access |
|---------|-------------|--------|
| **ATM22** | Airway Tree Modeling Challenge 2022, 300+ annotated chest CTs | [https://atm22.grand-challenge.org/](https://atm22.grand-challenge.org/) |
| **QIN‑Lung** | QIBA Lung CT collection from The Cancer Imaging Archive (TCIA) with airway annotations | [https://wiki.cancerimagingarchive.net/display/Public/QIN+LUNG+CT](https://wiki.cancerimagingarchive.net/display/Public/QIN+LUNG+CT) |
| **LIDC‑IDRI** (optional) | Nodule‑focused CTs with partial airway annotations, useful for domain expansion | [https://wiki.cancerimagingarchive.net/display/Public/LIDC-IDRI](https://wiki.cancerimagingarchive.net/display/Public/LIDC-IDRI) |
| *Other …* | (Add any additional dataset you used, e.g., EXACT’09, BREATH, etc.) | |

> ⚠️ All datasets are publicly available. Please cite the original publications (see [References](#references)) when using them.

## 🛠️ Installation

1. **Clone the repository**
   ```bash
   git clone https://github.com/your_username/Segmentation-of-airway-using-nnUNetv2.git
   cd Segmentation-of-airway-using-nnUNetv2
