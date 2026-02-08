Adversarially-Robust Deep Triplet Networks (ART-AL)

This repository contains the implementation of the Adversarially-Robust Triplet with Adaptive Loss (ART-AL) framework. The project explores the intersection of deep metric learning and adversarial robustness, specifically extending the Separately Constrained Triplet Loss (SCTL) to withstand gradient-based perturbations.

Project Overview

Standard triplet networks are often vulnerable to small adversarial perturbations. This project implements a progressive defense framework that maintains the geometric organization of an embedding space even under attack. The core innovation is a three-component loss that balances intra-class compactness, inter-class separation, and adversarial consistency through adaptive coefficients.

Model Variants

The repository includes three distinct stages of model development used for comparative analysis:

1. Baseline A-SCTL: A standard triplet learning implementation using the Adaptive Separately Constrained Triplet Loss. It uses a ResNet-18 backbone to learn a metric space where similar samples are clustered and dissimilar samples are separated without adversarial considerations.

2. Deep A-SCTL Refinement: An enhanced version of the baseline that fully exploits the capacity of the ResNet-18 architecture. This model incorporates hard negative mining and aggressive regularization to establish a strong metric backbone on clean data.

3. ART-AL (Adversarially-Robust Triplet): The full framework that introduces an adversarial consistency term to the loss function. It utilizes a progressive adversarial curriculum to gradually harden the model against stronger attacks while maintaining clean data performance.

Key Features

1. Adaptive Separately Constrained Loss (A-SCTL)

Unlike traditional triplet loss which uses a single relative margin, this approach decouples the objectives. It penalizes positive distances that are too large and negative distances that are too small using separate thresholds and an adaptive weighting factor () that shifts emphasis based on training dynamics.

2. Progressive Adversarial Curriculum

To prevent training instability, adversarial examples are introduced in stages. The perturbation strength () and the adversarial weighting coefficient () are increased over time, allowing the network to learn a stable metric structure before being subjected to full-strength Projected Gradient Descent (PGD) attacks.

3. Two-Phase Training Strategy

Phase 1 (Encoder Pretraining): The ResNet-18 embedding network is trained using the ART-AL objective to shape a robust feature space.

Phase 2 (Partial Adversarial Finetuning): The embedding network is frozen, and a lightweight MLP prediction head is trained on a mixture of clean and adversarial samples. This preserves the learned geometric structure while allowing the classifier to adapt to perturbations.

4. Adversarial Consistency Term

A third loss component measures the Euclidean discrepancy between the embeddings of clean inputs and their perturbed counterparts. This encourages the network to map both versions of the same sample to nearby points in the embedding space.

Implementation Details

Backbone: ResNet-18.

Attack Method: Multi-step PGD-20 bounded in the  norm.

Loss Function: A weighted combination of intra-class, inter-class, and adversarial consistency terms controlled by dual adaptive hyperparameters.

Environment: Developed using Python 3.11 and PyTorch 2.6.
