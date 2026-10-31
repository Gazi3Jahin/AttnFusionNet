AttnFusionNet: Attention-Based Multimodal Learning for ASD Classification Using Resting-State fMRI

Overview

AttnFusionNet is an exploratory deep-learning framework developed to investigate the use of complementary spatial and functional connectivity representations for Autism Spectrum Disorder (ASD) classification using resting-state functional MRI (rs-fMRI).

The central idea is to examine whether information learned from brain-image representations and inter-regional functional connectivity can be integrated within a unified learning framework.

Instead of relying on a single representation of resting-state fMRI, the proposed architecture follows two parallel feature extraction pathways:

Spatial feature learning: A convolutional neural network (CNN) extracts image-based representations from fMRI-derived axial slices.

Functional connectivity learning: An MLP learns from connectivity features constructed using the Automated Anatomical Labeling (AAL) atlas.

The two representations are subsequently integrated through an attention-based feature fusion mechanism before classification.

Beyond model development, the project explores branch-specific contributions, attention behaviour, and qualitative model interpretation.

Research scope: The classification task is ASD versus typically developing controls. The project does not perform speech-delay diagnosis and is not intended for clinical use.

1. Research Motivation

Resting-state fMRI provides an opportunity to investigate patterns of spontaneous brain activity and functional relationships between different brain regions.

However, these characteristics can be represented in different ways.

Image-based representations preserve spatial information that can be learned through convolutional operations. Functional connectivity, on the other hand, describes statistical relationships between regional brain signals.

These representations capture different aspects of the data, and their relative usefulness may depend on the classification task and the characteristics of the cohort.

AttnFusionNet was developed to investigate this complementary feature-learning approach.

The methodological objectives are to:

Extract spatial features from fMRI-derived image representations.

Construct region-based functional connectivity representations.

Learn modality-specific features through parallel neural network branches.

Integrate the learned representations using attention-based fusion.

Compare the fused architecture against individual feature branches.

Examine learned attention patterns and spatial feature activations.

2. Dataset and Cohort Preparation

Dataset

The experiments use resting-state fMRI data from the Autism Brain Imaging Data Exchange II (ABIDE II).

The selected cohort consists of ASD participants and typically developing controls. Diagnostic grouping is based on the ABIDE diagnostic label, DX_GROUP.

The dataset includes participants from multiple acquisition sites, making it relevant for exploring the challenges associated with heterogeneous neuroimaging data.

Data preparation

The experimental workflow involves preparing participant-level imaging data and constructing the representations required by the two feature extraction branches.

The principal preparation stages include:

Organizing the selected resting-state fMRI volumes.

Applying nuisance regression to reduce non-neural signal contributions.

Performing temporal filtering on regional time series.

Extracting image-based spatial representations.

Constructing atlas-based regional connectivity features.

Organizing the resulting features for participant-level model training and evaluation.

A stratified participant-level training, validation, and testing strategy is used.

This ensures that multiple image slices from the same participant are not independently distributed across the experimental subsets.

3. Dual-Branch Feature Extraction

A central methodological component of AttnFusionNet is the use of separate feature extraction pathways.

Each branch processes a different representation of the same underlying neuroimaging data.

3.1 Spatial Feature Extraction

The spatial branch uses image representations derived from preprocessed resting-state fMRI volumes.

A temporal mean BOLD representation is generated, followed by the selection of axial slices. These slices are resized and represented as grayscale images before being passed to a custom CNN.

The CNN learns hierarchical spatial representations through convolutional feature extraction.

This branch is intended to capture image-level patterns that may contribute to distinguishing the diagnostic groups.

The use of a custom CNN provides a relatively straightforward approach to learning spatial features without introducing an unnecessarily complex image architecture.

3.2 Functional Connectivity Extraction

The connectivity branch focuses on relationships between anatomically defined brain regions.

The Automated Anatomical Labeling (AAL) atlas is used to define regional time series across the brain.

The connectivity construction procedure involves:

Extracting regional time series from the atlas-defined regions of interest.

Computing pairwise Pearson correlation coefficients between regional signals.

Applying Fisher's z-transformation to the correlation coefficients.

Excluding self-connections from the resulting representation.

Preparing the connectivity features for neural network processing.

The resulting connectivity matrix represents statistical relationships between brain regions.

An MLP is employed to learn a compact feature representation from these connectivity patterns.

This branch provides a network-level representation that complements the image-based spatial features learned by the CNN.

4. Attention-Based Multimodal Feature Fusion

4.1 Feature Integration

After independent feature extraction, the spatial and connectivity representations are passed to the fusion component.

The objective is to allow the model to learn how much importance to assign to each feature branch during classification.

Rather than assuming that spatial and connectivity information contribute equally, the attention mechanism learns a relative weighting of their representations.

This provides a way to investigate whether the model relies more heavily on image-derived features or functional connectivity patterns.

4.2 Classification

The fused representation is passed to the classification component, which generates the final binary prediction.

The architecture therefore follows a progression from modality-specific feature learning to joint representation and classification.

4.3 Architectural Rationale

The dual-branch design was selected to preserve the distinct characteristics of the two input representations before combining them.

The spatial CNN learns from image-based patterns, while the connectivity MLP processes relationships between brain regions.

Attention-based fusion provides a mechanism for integrating these learned representations and examining their relative contribution to the prediction.

The overall methodology is intended as an investigation of multimodal feature learning rather than an assumption that fusion will necessarily outperform individual branches.

5. Experimental Design

The experimental workflow includes model training, validation-based monitoring, held-out testing, and branch-specific comparisons.

Model training

The network is trained using the training subset, while the validation subset is used to monitor learning behaviour and select the best-performing checkpoint.

Early stopping is incorporated to limit unnecessary training when validation performance no longer improves.

Training and validation histories are retained for examining the relationship between model fitting and generalization.

Branch-specific comparison

To investigate the contribution of the individual representations, the multimodal architecture is compared with two branch-specific baselines:

Spatial-only model: Uses the CNN-based spatial representation.

Connectivity-only model: Uses the atlas-derived functional connectivity representation.

This comparison helps determine whether the fusion mechanism provides an advantage over learning from either representation independently.

Evaluation

The classification experiments are assessed using standard binary classification measures, including accuracy, precision, recall, F1-score, and ROC-AUC.

Confusion matrices and ROC curves are also generated to examine prediction behaviour from different perspectives.

An additional exploratory five-fold cross-validation experiment is conducted for the connectivity-only baseline to investigate variation across different data partitions.

The cross-validation experiment is treated separately from the held-out evaluation and is not presented as independent external validation.

6. Experimental Observations

The experiments provided several useful observations about the behaviour of the proposed architecture.

Functional connectivity was a stronger individual representation. The connectivity-only baseline performed better than the spatial-only branch in the current held-out experiment, suggesting that the atlas-derived connectivity features contained more useful predictive information for this particular cohort.

Feature fusion did not automatically improve performance. The fused architecture did not outperform the connectivity-only baseline. This highlights an important challenge in multimodal learning: combining representations does not necessarily produce a more effective predictive model.

The fusion mechanism favoured connectivity information. Analysis of the learned attention distribution showed a strong preference for the connectivity branch over the spatial branch. This behaviour was consistent with the relative performance of the individual baselines.

Generalization remains a challenge. The training and validation histories indicated a considerable gap between model fitting and validation performance, suggesting overfitting and the need for stronger regularization and improved generalization strategies.

These observations informed the interpretation of the architecture and identified areas for further methodological improvement.

7. Model Interpretation and Explainability

Interpretability was explored through attention-weight analysis and Gradient-weighted Class Activation Mapping (Grad-CAM).

7.1 Attention Analysis

The learned attention weights were examined to understand how the fusion mechanism distributed importance between the spatial and connectivity branches.

The analysis indicated that functional connectivity received substantially greater attention than the spatial representation.

This provides insight into the model's internal weighting behaviour and offers a useful comparison with the branch-specific experimental results.

However, attention weights should not be interpreted as definitive measures of biological importance, causal influence, or clinical relevance.

7.2 Grad-CAM

Grad-CAM was applied to the CNN branch to examine spatial feature activations.

The resulting visualization provides a qualitative view of the image regions that influence the spatial branch's learned representations.

Since this implementation applies Grad-CAM to the spatial branch, it does not fully explain the final multimodal classification decision.

The visualizations should therefore be understood as branch-specific exploratory explanations rather than complete explanations of the fused model.

8. Limitations and Methodological Considerations

Several limitations are important when interpreting the experimental findings.

Cohort size: The selected cohort is relatively small, limiting the stability of the evaluation and the generalizability of the findings.

Multisite variability: The dataset contains participants from different acquisition sites, introducing potential variation in imaging characteristics.

Site-independent generalization: The current evaluation uses participant-level splitting rather than site-held-out validation. Generalization to entirely unseen acquisition sites has not been established.

Overfitting: The training and validation behaviour indicates that the current architecture requires further attention to regularization and generalization.

Unequal feature contribution: The fusion mechanism strongly favoured connectivity features, and the fused model did not outperform the connectivity-only baseline.

Spatial representation: The use of temporal mean BOLD images and selected axial slices simplifies the original four-dimensional fMRI data and does not preserve the full temporal dynamics.

Interpretability: Attention and Grad-CAM provide useful views of model behaviour, but they do not establish biological mechanisms or causal relationships.

These limitations are particularly relevant when working with small, heterogeneous neuroimaging cohorts.

9. Future Research Directions

The current experiments suggest several opportunities for further investigation:

Incorporating temporal representations to preserve more information from resting-state fMRI.

Investigating improved cross-modal feature alignment and fusion strategies.

Exploring regularization methods to reduce overfitting in limited-data settings.

Evaluating performance using site-independent and external validation.

Examining the stability of attention distributions across different data partitions.

Investigating prediction calibration and uncertainty estimation.

Conducting more rigorous statistical analyses of regional connectivity patterns.

These directions could help determine whether more carefully designed multimodal architectures can provide consistent benefits over individual neuroimaging representations.

10. Repository Structure

AttnFusionNet/
│
├── README.md
│
├── model/
│   └── best_attnfusionnet.pth
│
├── results/
│   ├── figures/
│   └── metrics/
│
└── requirements.txt

The repository includes the trained model checkpoint, available experimental outputs, and the dependencies used in the implementation.

The results directory contains visualizations and evaluation outputs generated during the experiments.

11. Reproducibility and Data Availability

This project uses data from the Autism Brain Imaging Data Exchange II.

Researchers interested in reproducing the experiments should obtain the dataset through the official ABIDE resource and comply with its access and data-use requirements.

The repository provides the trained checkpoint and available experimental outputs. Reproducing the complete experimental workflow also requires the corresponding preprocessing and model implementation, together with access to the original input data.

Participant-level neuroimaging data are not distributed through this repository.

The reported observations are specific to the current implementation and experimental protocol. Independent reproduction and external validation are necessary before drawing broader conclusions.

12. Technologies and Methods

Programming and deep learning

Python

PyTorch

Convolutional Neural Networks

Multilayer Perceptrons

Attention-based feature fusion

Neuroimaging and feature processing

Resting-state functional MRI

AAL anatomical parcellation

Regional time-series analysis

Pearson correlation

Fisher's z-transformation

Temporal filtering and nuisance regression

Evaluation and interpretation

Binary classification evaluation

ROC analysis

Confusion matrix analysis

Exploratory cross-validation

Attention-weight analysis

Grad-CAM

13. Acknowledgements and References

This project uses data made available through the Autism Brain Imaging Data Exchange (ABIDE) initiative.

Dataset reference

Di Martino, A., et al. (2017). Enhancing studies of the connectome in autism using the Autism Brain Imaging Data Exchange II. Scientific Data, 4, 170010.

https://doi.org/10.1038/sdata.2017.10

Disclaimer

AttnFusionNet is an independent experimental research project developed for methodological exploration and portfolio demonstration.

The model has not been clinically validated and must not be used for diagnosis, screening, treatment decisions, or other medical purposes.

The findings are specific to the experimental cohort and evaluation protocol. Further independent validation is required before drawing conclusions about clinical or population-level applicability.

Author: Gazi Tahsina Sharmin Jahin
