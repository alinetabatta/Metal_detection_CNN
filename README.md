# Automated-Screening-for-Metallic-Implants-in-MRI-Scout-using-Deep-Learning

This repository contains the official implementation and experimental framework for the automated detection of metallic artifacts during the initial localizer ("scout") phase of knee Magnetic Resonance Imaging (MRI) exams.
By identifying metallic interference at the earliest inception of the clinical workflow, this framework provides a proactive trigger for technicians to instantly deploy specialized Metal Artifact Reduction Sequences (MARS, MAVRIC SL, WARP, O-MAR). Shifting the detection trigger from the final diagnostic stage to the scout scan drastically minimizes operational inefficiency, patient re-imaging rates, and associated healthcare costs.

📌 Project Overview Objective: 

Early-stage prevention of metal-induced image degradation (signal voids, pixel displacement) using automated deep learning tools on 1.5T knee MRI localizer scans.  Architectures Evaluated: A comparative paradigm analysis between YOLOv8n (Nano) (a fast single-stage object detector emphasizing spatial localization) and ResNet18 (a classic residual network focused on global classification). 

Dataset: 1,254 balance-curated images (Sagittal, Coronal, and Axial planes) sourced from multi-vendor clinical data (Siemens Healthineers and GE Healthcare). Patient-level splitting was enforced to prevent data leakage.

Key Finding: YOLOv8 achieved an exceptional 99.4% classification accuracy under near-perfect conditions and demonstrated standout resilience against raw frequency-domain data anomalies ("spike" artifacts). 

## 📂 Dataset

The dataset utilized in this study, **Knee-MRI-Metal-Scout**, consists of 1,254 knee MRI localizer slices balanced across sagittal, coronal, and axial planes. 

The dataset is publicly hosted on **Roboflow Universe**. You can view the annotations, explore the images interactively, and download the dataset directly in the native YOLOv8 format (or any other computer vision format):


👉 **[Download the Knee-MRI-Metal-Scout Dataset on Roboflow](https://app.roboflow.com/tabatta/yolo-dataset-rml4j/1/images)**

### Dataset Structure Example (YOLOv8 Format)
If you download the dataset via the link above, it will be organized as follows:
```text
├── dataset/
│   ├── train/
│   │   ├── images/   # 891 training slices
│   │   └── labels/   # YOLOv8 format bounding box coordinates
│   ├── valid/
│   │   ├── images/   # 185 validation slices
│   │   └── labels/   
│   └── test/
│       ├── images/   # 178 testing slices
│       └── labels/
```

⚙️ Experimental WorkflowThe methodological pipeline is systematically split into four core stages: 

Data Acquisition & Curation: Specialist manual screening of knee localizer scans, including exact spatial bounding-box tagging using Roboflow.  

Model Development: Parallel training implementation using Transfer Learning weights from COCO (YOLOv8) and ImageNet (ResNet18). 

Stress Testing & Robustness Analysis: Strict validation against incrementally injected Gaussian Noise (SNR 40 down to 5) and simulated k-space spike artifacts.

Performance Evaluation: Comprehensive quantitative metrics benchmarking paired with qualitative interpretability via Grad-CAM saliency mapping.  

### 📊 Performance Summary

The quantitative performance of both models on the test set is summarized below. [cite_start]A statistical validation via patient-level bootstrapping was conducted to establish 95% confidence intervals, ensuring results are robust and unbiased.

| Model | Accuracy [95% CI] | Sensitivity [95% CI] | Specificity [95% CI] |
| :--- | :---: | :---: | :---: |
| **ResNet18 (Baseline)** | 95.1% [90.5% - 98.9%] | 95.7% [87.0% - 100%] | 94.4% [90.0% - 98.4%] |
| **YOLOv8 (Proposed)** | **99.4% [98.1% - 100%]** | **98.9% [96.0% - 100%]** | **100.0% [100% - 100%]** |

Stress Test InsightsSpike Artifact Resilience: 

When trained on a 25% data corruption mix, YOLOv8 maintained 97% accuracy, heavily outperforming ResNet18's drop to 66%. 

Noise Sensitivity: ResNet18 demonstrated superior stability in extreme sub-diagnostic noise thresholds, sustaining 90% accuracy at an SNR of 5.  

Spatial Awareness: YOLOv8 achieved a mean Intersection over Union (mIoU) of 0.7021, confirming its capacity to reliably approximate the boundaries of irregular artifact shapes.  

## 🎓 Citation
This work has been submitted to the Brazilian Conference on Biomedical Engineering (2026). The citation will be provided after the review process.
