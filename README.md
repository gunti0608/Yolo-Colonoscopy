# Comparative Analysis of YOLOv8 and YOLOv11 for Real-Time Polyp Detection


This repository presents a comprehensive benchmarking study between **YOLOv8s** (the industry baseline) and **YOLOv11s** (the 2024 challenger) for the detection of colorectal polyps. This project was developed as part of an MSc Dissertation to evaluate clinical sensitivity and architectural robustness in computer-aided diagnosis (CADe).

## Motivation
Colorectal cancer is a leading cause of cancer deaths globally. Early detection during colonoscopy can reduce mortality by up to 70%, but human miss-rates remain at ~26%. This project aims to validate a real-time AI solution that operates at **>50 FPS**, providing a "second set of eyes" for surgeons to minimize False Negatives (missed polyps).

## Dataset: Kvasir-SEG
*   **Original Data:** 1,000 high-resolution endoscopic images with polygonal masks.
*   **Data Engineering:** Developed a custom Python pipeline to convert segmentation masks into YOLO-normalized bounding boxes (`[class, x_center, y_center, width, height]`).
*   **Split:** 70% Training | 20% Validation | 10% Unseen Test.

## Experimental Setup
*   **Hardware:** Benchmarked on **NVIDIA Tesla T4 GPU** (Google Colab Cloud Compute).
*   **Optimizer:** **AdamW** (Decoupled Weight Decay) to enhance generalization in low-contrast medical environments.
*   **Training:** 100 Epochs with a patience of 25 (Early Stopping) and Automatic Mixed Precision (AMP).

## Final Results

| Metric (Test Set) | YOLOv8s (Baseline) | YOLOv11s (Challenger) | Clinical Verdict |
| :--- | :--- | :--- | :--- |
| **mAP50 (Accuracy)** | **0.8894** | 0.8437 | **v8 is 5.4% more accurate** |
| **Recall (Sensitivity)** | **0.8209** | 0.7711 | **v8 is safer (lower miss rate)** |
| **Precision** | **0.8146** | 0.7598 | v8 has fewer false alarms |
| **Inference Speed** | **60.1 FPS** | 59.8 FPS | Both exceed 30 FPS target |

### Key Findings
1.  **The Clinical Winner:** Despite YOLOv11 being a newer architecture, **YOLOv8s outperformed it** in all clinical metrics for this dataset.
2.  **The Learning:** YOLOv11s showed a massive **402% improvement** from pilot to final training, indicating high capacity for larger datasets (e.g., Hyper-Kvasir).
3.  **The Robustness:** Using Test-Time Augmentation (TTA) to simulate surgical artifacts, YOLOv8s accuracy actually **increased from 0.82 to 0.84**, proving exceptional resilience to motion blur and glare.
