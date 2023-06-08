# PCAAE-with-Distance-Correlation
Improved PCAAE using distance correlation instead of Pearson correlation


## PCA-AE
PCA-AE is one of autoencoders which decomposes input data like principal component analysis (PCA), but in a non-linear way. You can organize the latent space of a given data to be linearly uncorrelated, figure out the meaning of each component of the space and manipulate the space intuitively through the model. 

## Limitations of PCAAE
However, PCAAE also has limitations.
  1. The original model uses covariance loss as regularization. Since this loss function is theoretically equivalent to Pearson correlation, it does not gaurantees independency between each component of latent space.
  2. PCAAE consists of encoder with several sub-encoders of 1-dimensional output and ordinary decoder. At the k-th step, there are k sub-encoders and 1 new decoder that takes k-dimensional input. Other decoders trained before the k-th step are not used at the k-th step. This training procedure is very time consuming and computational resource intensive.

