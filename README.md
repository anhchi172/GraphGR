# GraphGR

GraphGR is a tool to predict drug (kinase inhibitors ONLY for now) response on a specific cell line given genetic information of the cell line, binding affinity of the drug to the proteins, and the disease association scores of the proteins. It utilizes heterogeneous data to construct a graph for each cell-line-drug combination (instance). The trained model can be used to predict the effect (currently a binary classification) of a drug on a given cell line.

If you find this tool useful, please cite our paper :)

Singha M, Pu L, Shawky M, Busch K, Wu H-C, Ramanujam J, Brylinski M (2020) GraphGR: A graph neural network to predict the effect of pharmacotherapy on the cancer cell growth. BioRxiv. https://www.biorxiv.org/content/10.1101/2020.05.20.107458v1

This README file is written by Limeng Pu.

# Prerequisites

1. Python 3.7.*
2. Pytorch 1.4.0 or higher
3. PyTorch Geometric latest version (https://pytorch-geometric.readthedocs.io/en/latest/notes/installation.html)
4. Pandas 1.0 or higher
5. Networkx 2.4 or higher
6. CUDA if you'd like to run on GPU(s)

For the installation instruction please refer to the corresponding project site.

# Usage

This repo provides data processing, prediction, and training modules if you'd like to train on your own dataset. 

# Data Preprocessing

The input data required for the model comes in the form of graphs, which consists of a list of nodes and a list of edges (edge features are not supported in the currently implementation), which is provided in this repo based on the STRING databse (https://string-db.org/). The processing steps include: graph generation with node feaures, graph reduction, and graph to matrix conversion. The detailed information can be found in `./preprocessing`.

1. Graph generation: 

    Generate a graph representation of the input data based on the node table provided by the user and the edge table provided in the repo. The resulting graph representation will be used for the graph reduction in the next step.

    input --> node table (.csv) Note that we use the ensemble id as the node id.
    
    output --> graph (.gexf, can be read in Networkx).

2. Graph reduction:

    Reduce the original graph representation to a more feature rich form for better learning performance.

    input --> node table (.csv), graph (.gexf), additional reduction rules.

    output --> reduced node table (.csv), reduced edge table (.csv), reduced graph (.gexf), reduction records (.csv).

3. Graph to matrix conversion

    Convert the graph to matrix reprsentation as the final input to the model.

    input --> reduced node table (.csv), reduced graph (.gexf).

    output --> matrices (.h5) including node features matrix (N x d), adjacency matrix (N x N), edge indices (2 x E).

# Prediction

Once one finished generating the input data, the prediction can be produced by 
