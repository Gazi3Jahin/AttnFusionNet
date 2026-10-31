# AttnFusionNet

## Multimodal Deep Learning for ASD Classification Using Resting-State fMRI

AttnFusionNet is an independent research and portfolio project exploring
multimodal deep learning for autism spectrum disorder (ASD) classification
using resting-state functional MRI data from ABIDE II.

### Project overview

The framework explores two feature branches:

- Spatial branch: CNN-based representations of functional MRI volumes.
- Connectivity branch: AAL-atlas-based functional connectivity features.

An attention-based fusion mechanism combines the learned representations
for binary ASD-versus-control classification.

### Dataset

The project uses a selected cohort of 126 participants from ABIDE II,
including 65 ASD participants and 61 typically developing controls.

The data are not included in this repository. Access ABIDE II through
its official data-sharing process.

### Methodology

- fMRI preprocessing and participant selection
- AAL-based functional connectivity extraction
- Spatial feature extraction
- PyTorch-based multimodal architecture
- Attention-based feature fusion
- Held-out evaluation and exploratory cross-validation
- Grad-CAM and connectivity-based analysis

### Results and limitations

The held-out test set contained 19 participants.

| Metric | Result |
|---|---:|
| Accuracy | 57.89% |
| Precision | 57.14% |
| Recall | 80.00% |
| F1-score | 66.67% |
| ROC-AUC | 0.678 |

The experiments showed substantial overfitting and variability in
performance. The exploratory five-fold connectivity baseline evaluation
should not be interpreted as independent confirmation of the held-out
test results.

This is an experimental portfolio project, not a clinically validated
diagnostic system. The dataset's diagnostic labels distinguish ASD from
typically developing controls; they do not establish clinical speech-delay
diagnoses.

### Repository structure

- `notebooks/`: Research notebook(s)
- `results/figures/`: Generated visualizations
- `results/metrics/`: Aggregate evaluation results
- `model/`: Saved model checkpoint, if included

### Reproducibility

The original data must be obtained separately. Paths and environment
settings may need adjustment before rerunning the notebook.

### Responsible use

This project is for research and educational purposes only.
It is not intended for clinical diagnosis or medical decision-making.
