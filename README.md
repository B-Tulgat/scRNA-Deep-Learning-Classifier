# Main result

We demonstrate that a generative single-cell model (LDVAE) trained on small-scale transcriptomic data from primary motor cortex cells can learn biologically meaningful latent representations of cell states.
By synthetically perturbing gene expression levels associated with neurodegenerative diseases (ALS and Huntington’s disease), we simulated SNP-driven shifts in cell state and showed that these perturbations are captured in the learned latent space.
A simple neural network trained on the latent representations achieved 91% accuracy in distinguishing perturbed from unperturbed cell states, suggesting that the generative model effectively encodes disease-relevant transcriptomic variation.

# Approach

In this repositry we trained a generative model (LDVAE - Linearly Decoded Variational Autoencoder) on a single-cell transcriptomics of primary motor cortex cells (Normal) to learn latent representations of cell states.
We simulated SNP-driven shift by synthetically scale up/down gene expression related to gene expression that are up or down-reguled in Amyotrophic lateral sclerosis, ALS for downward and Huttington Disease, HD for upward.

Within our dataset we found 38 out of 41 down regulated genes for ALS and 68 out of 80 up regulated genes for HD.

The total data consisting of ~100k cells due to computational limitation we randomly selected 5000 cells and reduced computational demand. We then further synthesize data for ALS and HD
down and up scaling respectively from the base dataset and finally combine them into total 15000 cells of data.

We split the total data into 70/30 with stratification for training and testing before any training takes place for both LDVAE and Classifier Neural Network.

Using the training 9000 cells with 29651 genes (features) we trained a LDVAE compressing the data into only 36 dimensions. We visualize the clustering with UMAP and Leiden Algorithm:

<img width="711" height="423" alt="image" src="https://github.com/user-attachments/assets/ecb7c9c2-04f4-41b2-b049-06c1e66d2e9b" />

The training and validation loss is clear:

<img width="582" height="463" alt="image" src="https://github.com/user-attachments/assets/f344039b-87eb-44f8-b508-d70101c51dc7" />

Now instead of training 9000 cells with 29651 genes we use the latent representation, significantly reducing the input data into 9000 x 36.

In which a simple Neural network with 252 hidden parameters achieves 91% accuracy. We provide the confusion matrix below:

<img width="1118" height="565" alt="image" src="https://github.com/user-attachments/assets/5d38ea2e-e224-443e-8718-0b27a02c08f2" />




## Dataset
~30k gene expressions of ~114k cells within *Primary Motor Cortex Tissue* (M1).
- Data Page: https://data.humancellatlas.org/hca-bio-networks/nervous-system/atlases/cortex-v1-0

   - Atlas Name: Dissection: Primary motor cortex (M1)
   - Tissue: primary motor cortex
   - Disease: normal
   - Cells: 114.6k

## LDVAE

LDVAE enables interpretation of Variational Autoencoder (VAE) latent representations by associating higher coefficients (weights) with higher levels of gene expression for specific latent basis components. Our model’s latent representation is 36-dimensional.
The gene expression data were acquired from cells within the primary motor cortex (M1). 

- LDVAE research paper: https://academic.oup.com/bioinformatics/article/36/11/3418/5807606
- LDVAE scvi-tools documentation: https://docs.scvi-tools.org/en/1.3.2/api/reference/scvi.model.LinearSCVI.html#scvi.model.LinearSCVI
