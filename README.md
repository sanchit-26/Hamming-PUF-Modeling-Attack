[README.md](https://github.com/user-attachments/files/29646426/README.md)
# Hamming-PUF
Breaking the Hamming PUF (CS771)
Overview
This repository contains my code and analysis for compromising a Hamming Physically Unclonable Function (PUF) using machine learning. By mapping the underlying hardware logic into a higher-dimensional space, I built a linear classifier capable of cracking the cryptographic boundary with near-perfect accuracy.

Repository Structure
submit.py: The core Python script. Contains the custom 289-D feature embedding logic (my_map) and the tuned hyperparameters for the SVM (my_params).

Final_Report.pdf: The full write-up. Covers the mathematical proof for the feature expansion, hyperparameter ablation studies, and sample complexity analysis using VC-dimension theory.

How it Works
Standard Arbiter PUFs are relatively straightforward to model, but this Hamming PUF uses non-linear low-level operations (products of partial Hamming weights). To break it using a linear model, I had to expand the 32-bit raw challenges into a custom feature space.

1. Feature Engineering (D = 289)
I expanded the input space to capture all the linear terms and cross-correlations between the even and odd challenge bits. The breakdown:

1 Bias term (to capture the secret threshold)

32 Linear terms (the raw bits)

256 Cross-product terms (even_bit * odd_bit)

2. Model Optimization
I used an sklearn LinearSVC with squared_hinge loss and an L2 penalty. Because we have way more samples than features, I forced the model to use the primal solver (dual=False), which drastically cuts down convergence time.

Results
Peak Accuracy: Hit 99.7% on unseen Challenge-Response Pairs (CRPs).

Sample Efficiency: Proved mathematically and empirically that we only need about 5,000 CRPs to reliably bypass the hardware security (>99.3% accuracy), without needing massive datasets.

Speed: The NumPy vectorization in the feature map keeps execution extremely fast and lightweight.

Usage
This code is formatted to hook directly into the CS771 automated evaluation pipeline.

my_map(X) takes the raw (N, 32) challenges and outputs the (N, 289) embeddings.

my_params() passes the optimized configurations straight to the LinearSVC instance.
