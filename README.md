# rnaseq_tissue
 Tissue Classification using Bulk RNA-Seq data from DepMap

 data_prep.ipynb

    Data: 1402 samples and 18292 genes (removed samples with no label)
    The data has been log normalized and I removed genes with low variability(housekeeping genes) and z-score normalized and learning batchnorm1d in the network
    Labels: categorical labels for 41tissues (testes and matched normal tissue have no collected samples) so 39 tissues, got their one hot encoding

    This script processes the data and creates train/val/test split dataframes that it saves at pickles

    Some features have alot of zeros = some genes are only expressed in few samples
    Some tissues have few collected samples
    Got age, tissue, disease distribution and found the sparsity of the matrix(~0.17)
    And used Jaccard Similarity to check how correlated the features are
 
 baseline.ipynb

    This file uses kernelPCA, random forest calssifier and SVC to set a baseline for the deeplearning models.
    We find that SVC on the uncompressed data results in a ~0.4 accuracy

 autoencoder.ipynb

    This file defines the model and the training loop for an autoencoder with a latent space of size 512
    The model was too large to load on my GPU so I used google colab
 
 classify_embeddings.ipynb
    
    This file uses the latent space representation of the data for classification by KNN,RF and SVC
    The prediction accuracy is about the same ~0.4

 dl_tissue_classifier.ipynb

    This file defines the model and the training loop for a tissue classifier
    The model architecture is an encoder prior to an MLP and the loss function is cross-entropy

 data folder
   
    This folder contains the pickled dataframes
    doesn't include the data from DepMap (they were too large)

 results folder
   
    This folder contains some training/validation loss plots inside of plotting.ipynb
    distribution of the data is also shown in the images

 embeddings folder

    this folder contains the embeddings made by autoencoder.ipynb
 
 Issues:

    The autoencoder didn't seem like it learned anything past a few epochs (the validation loss stabilized)
    The DL classifier never really learned anything even with different experiemnts: small data set(memorizing), small learning rate, decrease the depth and add L1 regularization

 Future:

    use bayes opt for hyperparameter tuning (started building a genetic algorithm for doing the hyperparameter tuning instead of gridsearch but then found bayes opt)
    try convolutional layers (i don't have an intuition ofr how this might work but I would like to try it!)
    *visualize the latent space colored by the classifications*