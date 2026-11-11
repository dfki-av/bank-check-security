# 🏦 Enhanced Bank Check Security: Novel Dataset & Transformer-Based Signature Verification

> 📌 *Accepted at the Document Analysis Systems (DAS) Workshop at ICDAR 2024*
> 📄 [Read the Paper](https://link.springer.com/chapter/10.1007/978-3-031-70442-0_3)
> ✍️ Muhammad Saif Ullah Khan*, Tahira Shehzadi*, Rabeya Noor, Didier Stricker & Muhammad Zeshan Afzal
> *(* Equal contribution)*

---

## 🔍 Overview

Automated **signature verification** on **bank checks** is crucial for fraud prevention and transaction security. However, real-world bank checks contain **complex layouts** with both textual and graphical elements, making this task particularly challenging.

This repository provides:

- **Synthetic Signature Bankcheck Images (SSBI) Dataset**: a realistic dataset with genuine and forged signatures on bank checks.
- **Detection-Based Signature Verification Approach**: treats genuine and forged signatures as separate object detection classes.
- **DINO-Based Transformer Model with a Dilation Module**: achieves **99.2% AP** for genuine and **99.4% AP** for forged signatures.
- **Data Processing Scripts**: easily analyze, visualize, and expand the dataset.

---

## 📊 SSBI Dataset

The **SSBI Dataset** contains **4,360** annotated check images (**3,360 genuine** and **1,000 forged**) from **19 different signers**. Each check also includes bounding boxes for:

- **Signatures** (genuine and forged)
- **Date field**
- **Amount fields** (legal and courtesy)
- **Payee field**

📥 **Download Dataset:** [SSBI v1.0.0](https://github.com/saifkhichi96/ssbi-dataset/releases/tag/v1.0.0)
📌 **Extract to:** `data/ssbi`

### 🗂 Dataset Statistics

**Table 1: Data split for genuine and forged signatures**

| Signature Type | Train | Validation | Total |
| -------------- | ----- | ---------- | ----- |
| Genuine        | 2,352 | 1,008      | 3,360 |
| Forged         | 700   | 300        | 1,000 |
| **Total**      | 3,052 | 1,308      | 4,360 |

**Table 2: Object size distribution**

| Category            | Train (Small/Medium/Large) | Val (Small/Medium/Large) |
| ------------------- | -------------------------- | ------------------------ |
| Amount (Legal)      | 440 / 1,881 / 731          | 169 / 813 / 326          |
| Amount (Courtesy)   | 0 / 1,142 / 1,910          | 0 / 481 / 827            |
| Date                | 163 / 1,837 / 754          | 57 / 777 / 334           |
| Payee               | 0 / 994 / 717              | 0 / 387 / 300            |
| Signature (Forged)  | 2 / 304 / 394              | 0 / 122 / 178            |
| Signature (Genuine) | 4 / 915 / 1,433            | 1 / 358 / 649            |

---

## 📌 How to Use

### 1️⃣ Installation

```bash
git clone https://github.com/dfki-av/bank-check-security.git
cd bank-check-security
python3 -m venv venv
source venv/bin/activate
pip install -U pip wheel
pip install -r requirements.txt
```

(`created_dataset.py` itself only needs `opencv-python` and `numpy`; `fiftyone` and `notebook` are used by the statistics script and notebooks.)

### 2️⃣ Dataset Statistics

```bash
python print_statistics.py
```

### 3️⃣ Dataset Structure (released SSBI)

```
data/
├── ssbi/             # NOTE: download & extract the dataset here
│   ├── annotations/  # COCO-style JSON annotations
│   ├── train/        # Training check images (genuine & forged signatures)
│   ├── val/          # Validation check images
```

### 🔄 Generating Synthetic Data

If the download link is unavailable, or you wish to create more synthetic data, use `created_dataset.py`. It takes one argument, `forged` or `genuine` (default: `forged`):

```bash
python3 created_dataset.py forged     # -> data/generated/forged_checks/   (1,000 images)
python3 created_dataset.py genuine    # -> data/generated/genuine_checks/  (3,360 images)
```

Each run writes its own `data/` folder of JPEG images and a COCO-style `labels.json` in its output directory, so the two sets never overwrite each other.

**How it works.** Every signature crop from the source collection sheets is composited onto **all 10 check templates** in `data/sources/checks`, in a randomly chosen pen colour (black, gray, blue, red or green) and at a random position inside the signature box. The date, amount, legal amount and payee fields are filled with random handwritten text crops from `data/sources/dummy_text`. This gives 100 forged signatures × 10 checks = 1,000 forged images and 336 genuine signatures × 10 checks = 3,360 genuine images.

**Notes**

- Pen colour, signature position and pasted text are random, so output is equivalent in count and structure to the released dataset, not pixel-identical. Add `random.seed(0)` near the top of the script for reproducible runs.
- The script does not create the train/validation split. The released dataset uses a 70/30 split (see Table 1); you need to split the generated images and annotations yourself.
- Generation takes a few minutes per set (the genuine set is larger).

The script relies on the source data in `data/sources`:

```
data/
├── sources/                # underlying sources for the synthetic dataset
│   ├── checks/             # original check images (10 templates) + labels.json
│   ├── dummy_text/         # dummy text for date, amount and payee fields + labels.json
│   ├── signatures/         # original collection sheets with signature data
│   │   ├── genuine/        # genuine signatures (19 signers)
│   │   ├── forged/         # skilled forgeries
├── generated/              # created by created_dataset.py
│   ├── forged_checks/
│   └── genuine_checks/
```

---

## 📜 Citation

If you use this dataset or method, please cite our paper:

```bibtex
@inproceedings{khan2024enhanced,
  title={Enhanced Bank Check Security: Introducing a Novel Dataset and Transformer-Based Approach for Detection and Verification},
  author={Khan, Muhammad Saif Ullah and Shehzadi, Tahira and Noor, Rabeya and Stricker, Didier and Afzal, Muhammad Zeshan},
  booktitle={International Workshop on Document Analysis Systems},
  pages={37--54},
  year={2024},
  organization={Springer}
}
```

## 📧 Contact

For questions, please reach out to Muhammad Saif Ullah Khan ([saifkhichi96](https://github.com/saifkhichi96)).

## License

The code and dataset in this repository are licensed under the [CC BY-NC 4.0](https://creativecommons.org/licenses/by-nc/4.0/) license as described in the [LICENSE](LICENSE) file.
