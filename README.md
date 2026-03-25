# Synthetic_PBX
Hierarchical  approach to realistic PBX Generation
# Graph-Based Neural Network Model

## Overview
This project implements a Graph-Based Neural Network using PyTorch and NetworkX to model and generate structured data in the form of graphs. It leverages recurrent neural networks (RNNs) for both node and edge prediction, incorporating multi-head attention and normalization techniques.

## Features
- **Graph Neural Network (GNN)** for structured data modeling
- **Custom RNNs for nodes and edges** with multi-head attention
- **Graph normalization and denormalization** for improved training stability
- **Adaptive edge thresholding** to enhance connectivity
- **CUDA support** for GPU acceleration
- **Training with adaptive learning rates** and early stopping
- **Graph visualization** in 3D using Matplotlib
- **Checkpointing for model resumption**

## Dependencies
Ensure you have the following dependencies installed:
```bash
pip install numpy networkx torch matplotlib tqdm
```

## Model Components
### 1. **Node RNN (`CUSTOM_RNN_NODE`)**
   - Embeds node features
   - Uses a GRU layer for sequence modeling
   - Multi-head attention for capturing dependencies
   - Predicts node attributes such as position, scale, and curvature

### 2. **Edge RNN (`CUSTOM_RNN_EDGE`)**
   - Uses GRU to model edge connectivity
   - Embedding option for edge features
   - Predicts edge weights using sigmoid activation

## Training Process
### 1. **Data Preparation**
   - Load subgraph datasets (`MULTI_train_subgraphs.npz`, `MULTI_test_subgraphs.npz`)
   - Normalize node features and edge weights

### 2. **Training**
   - Uses `train_epoch()` for training node and edge RNNs
   - Implements mixed-precision training with `GradScaler`
   - Accumulates gradients to stabilize training
   - Saves model state and handles potential crashes

### 3. **Validation**
   - Uses `validate_epoch()` to evaluate models
   - Calculates MSE loss for node and edge predictions

### 4. **Graph Generation**
   - Generates new graphs using `generate_new_node()` and `add_node_to_graph()`
   - Ensures connectivity by enforcing BFS ordering
   - Outputs visualizations using `plot_graph_3d()`

Training includes:
- Resuming from a checkpoint if available
- Saving best model based on validation loss
- Generating and saving graph visualizations

## Model Checkpointing
The model automatically saves its state in:
```
output_folder/
  ├── node_rnn_final.pth
  ├── edge_rnn_final.pth
  ├── training_state.pth
  ├── loss.png
  ├── graph_epoch_*.png
```

## Output
- **Trained models** for node and edge prediction
- **Loss curves** for tracking training progress
- **Generated graphs** visualized in 3D
