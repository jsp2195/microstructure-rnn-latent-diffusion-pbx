# microstructure-rnn-latent-diffusion-pbx
Two-stage generative AI framework for topology-preserved synthetic PBX microstructures (GraphRNN + conditional latent diffusion).

This repository accompanies the manuscript:

**Poliner, J., Sun, W., Alshibli, K. A., & Regueiro, R. A. (2026)**  
*Deep Generative Graphs for Synthesizing Microstructure of Topology-Preserved Polymer-Bonded Explosives*  
Engineering with Computers.

We introduce a hierarchical generative artificial intelligence framework for synthesizing statistically and topologically consistent three-dimensional polymer-bonded explosive (PBX) microstructures. The framework decomposes the generative task into two coupled sub-problems:

1. **Topological generation** of grain networks via a modified GraphRNN (MicrostructureRNN).
2. **Geometric generation** of individual grains via a conditional latent denoising diffusion probabilistic model (DDPM).

This separation enables control over both global connectivity and local morphology while avoiding the curse of dimensionality associated with direct voxel-based synthesis.

---

## 1. Hierarchical Generative Framework

The workflow consists of three principal components:

### (a) MicrostructureRNN — Topological Generation

Each PBX subdomain is represented as a node/edge weighted graph  
\( G = (V, E, X, E_w) \), where node features include:

- Centroid position  
- Effective radius  
- Oriented Bounding Box (OBB)  
- Roughness (mean curvature measure)  

Edges encode inter-grain spacing via continuous edge weights.

An autoregressive recurrent neural network sequentially generates nodes and edges using heteroscedastic Gaussian likelihoods. Breadth-first search (BFS) ordering ensures consistent sequence representation of graphs.

---

### (b) Conditional Latent Diffusion — Grain Geometry

Each grain surface is represented as a point cloud and embedded into a 32×32 latent grid via a geometry-aware autoencoder.

A conditional latent DDPM then synthesizes grain geometry using:

- OBB extents  
- OBB orientation (Euler angles)  
- Effective radius  
- Mean curvature descriptor  

Classifier-free guidance enforces geometric consistency while preserving stochastic variability.

---

### (c) Reconstruction and Meshing

Generated point clouds are:

1. Rescaled to physical units  
2. Reconstructed via Poisson surface reconstruction  
3. Remeshed into watertight crystal grains  
4. Assembled into subdomains  
5. Voxelized for simulation-ready geometry  

---

## 2. Visual Summary of the Framework

### Topology → OBB → Grain Geometry

<p align="center">
  <img src="assets/subgraph_idox_notitle_AArrow_20.png" width="900">
</p>

---

### Conditional Latent Diffusion Process

<p align="center">
  <img src="assets/Encoded_OBB_Reconstruction_rows_ab.png" width="900">
</p>

The OBB and curvature descriptors constrain the reverse diffusion process, ensuring geometric compatibility with topological prescriptions.

---

### Point Cloud to Mesh Reconstruction

<p align="center">
  <img src="assets/meshing.png" width="900">
</p>

Point clouds are converted into watertight remeshed crystal grains suitable for numerical simulation.

---

### Representative Generated Subgraph Clusters

<p align="center">
  <img src="assets/generated_subgraphs.png" width="1000">
</p>

The figure above illustrates representative synthetic PBX subgraphs composed of:

- Generated graph topology  
- Synthesized grains  
- Remeshed crystal geometry  

---

## 3. Included Generated Dataset

This repository includes:

### 100 Generated Subgraph Clusters

Each subgraph contains:

- Generated topology (node-weighted graph)
- Synthesized grain point clouds
- Remeshed crystal grains reconstructed from point clouds
- Spatially assembled subdomain

The assemblies preserve statistical fidelity relative to micro-CT benchmark data across:

- Grain size distribution  
- Surface mean curvature  
- OBB dimensions and orientations  
- Orientation tensor descriptors (compactness, flakiness, elongation)  
- Edge-weight statistics (inter-grain spacing)  
- Degree distribution  
- Clustering coefficient  
- Graph density  
- Polymer-to-grain phase ratio  

---

## 4. Statistical Validation

The generative model was validated using:

- Empirical cumulative distribution functions (eCDFs)  
- Orientation tensor analysis  
- Graph-theoretic metrics  
- Volumetric phase fraction comparison  

The synthetic microstructures reproduce both geometric and topological statistics of the micro-CT IDOX assemblies within acceptable deviation, with minor regularization-induced compression in distribution tails.

---

## 5. Code Release

The complete training and inference codebase will be released in staged updates, including modules for:

