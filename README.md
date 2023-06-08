# PCAAE with Distance Correlation
Improved PCAAE using distance correlation instead of Pearson correlation


## PCA-AE
PCA-AE is one of autoencoders which decomposes input data like principal component analysis (PCA), but in a non-linear way. You can organize the latent space of a given data to be linearly uncorrelated, figure out the meaning of each component of the space and manipulate the space intuitively through the model. 

## Limitations of PCAAE
However, PCAAE also has limitations.
  1. The original model uses covariance loss as regularization. Since this loss function is theoretically equivalent to Pearson correlation, it does not gaurantees independency between each component of latent space.
  2. PCAAE consists of encoder with several sub-encoders of 1-dimensional output and ordinary decoder. At the k-th step, there are k sub-encoders and 1 new decoder that takes k-dimensional input. Other decoders trained before the k-th step are not used at the k-th step. This training procedure is very time consuming and computational resource intensive.

## Improvement
To solve the first limitation, I applied distance correlation to the regularization function of the basic model. Unlike Pearson correlation, one of important properties of distance correlation is 

    for scalar random variables X&Y, 
    dCor(X,Y) = 0 if and only if X&Y are independent.

## Other coefficients
Of course, distance correlation is not the only option that is better than Pearson correlation coefficient. There are other effective coefficients like maximal correlation coefficient or maximal information coefficient. However, these coefficients are not perfect in all cases.

Maximal correlation coefficient has some undesirable properties like,
  1. it is quite harder to compute than Pearson's R or distance correlation.
  2. the result depends on the ACE algorithm's smoother like histogram or kernel regression.
  3. when using the ACE algorithm, there are some cases that for random variables X&Y, even mCor(X,Y)=1 does not implicate perfect determinisic relation between X and Y.

The maximal information coefficient is based on mutual information of information theory. To calculate MI between continuous distributions, samples need to be quantized into an appropriate grid. This method uses dynamic programming to efficiently approximate the normalized true MI value, MIC. It is the most effective but complicate method than others.
