# PCAAE with Distance Correlation
Improved PCAAE using distance correlation instead of Pearson correlation


## PCA-AE
PCA-AE[1, 2] is one of autoencoders which decomposes input data like principal component analysis (PCA), but in a non-linear way. You can organize the latent space of a given data to be linearly uncorrelated, figure out the meaning of each component of the space and manipulate the space intuitively through the model. 


## Limitations of PCAAE
However, PCAAE also has limitations.
  1. The original model uses covariance loss as regularization. Since this loss function is theoretically equivalent to Pearson correlation, it does not guarantees independence between each component of the latent space.
  2. PCAAE consists of encoder with several sub-encoders of 1-dimensional output and ordinary decoder. At the k-th step, there are k sub-encoders and 1 new decoder that takes k-dimensional input. Other decoders trained before the k-th step are not used at the k-th step. This training procedure is very time consuming and computational resource intensive.


## Improvement
To solve the first limitation, I applied distance correlation[3] to the regularization function of the basic model. Unlike Pearson correlation, one of important properties of distance correlation is 

    for scalar random variables X&Y, 
    dCor(X,Y) = 0 if and only if X&Y are independent.[4]
 

## Used Data
The input images are composed with white ball's angle of polar coordinate(1D, unit: 2pi/72) and brightness of the green center ball(1D, range: [128, 255]). A total of 72 * 128 = 9216 cases. 

![data_1color_and_1circle](https://github.com/disin7c9/PCAAE-with-Distance-Correlation/assets/94789911/6f2e225b-000c-467a-8a10-bf31ef39704e)

**Figure1:** *There are white revolving ball and smaller fixed green ball in the center.*
   
    
## Results
![interpolation_AEs](https://github.com/disin7c9/PCAAE-with-Distance-Correlation/assets/94789911/4672386b-65cc-4a1f-b82d-72e7edbb4879)

**Figure2:** *Interpolations in the latent space of simple AE, basic PCAAE and PCAAE with distance correlation*

Only the modified model assigned the location information of the white ball to a single component in the latent space and learned the brightness of green ball.


## Conclusion
The modified model could recognize non-linear relationships between the prior fixed component in the latent space and later non-fixed component under training. This was demonstrated in both qualitative and quantitative results(refer to the "main.ipynb").

Of course, distance correlation is not the only option that is better than Pearson correlation coefficient. There are other powerful coefficients like maximal correlation coefficient[5] or the maximal information coefficient[6]. However, these coefficients are not perfect in all cases.

Maximal correlation coefficient has some undesirable properties like,
  1. it is quite harder to compute than Pearson's R or distance correlation.
  2. the coefficient output depends on the ACE algorithm's smoother such as histogram or kernel regression.
  3. when using the ACE algorithm, there are some cases that for random variables X&Y, even mCor(X,Y)=1 does not implicate perfect determinisic relation between X and Y.[4]

The maximal information coefficient is based on mutual information of information theory. To calculate MI between continuous distributions, samples need to be quantized into an appropriate grid. This method uses dynamic programming to efficiently approximate the normalized true MI value, MIC. It is the most effective but complicate method than others.

Furthermore, if someone wants to apply maximal correlation or MIC to deep learning, the one need to draw computational graph and find derivatives of backward propagation, even though there are packages for those coefficients in R studio or python. From that point of view, distance correlation is very simple to use. Just write the forward formula, and frameworks like PyTorch or TensorFlow do the calculation for you.


## References
- [1] Pham, Chi-Hieu, Saïd Ladjal, and Alasdair Newson. "PCA-AE: Principal Component Analysis Autoencoder for Organising the Latent Space of Generative Networks." Journal of Mathematical Imaging and Vision 64.5 (2022): 569-585.
- [2] https://github.com/chieupham/PCAAE/tree/main
- [3] Székely, Gábor J., Maria L. Rizzo, and Nail K. Bakirov. "Measuring and testing dependence by correlation of distances." (2007): 2769-2794.
- [4] https://www.stat.cmu.edu/~ryantibs/datamining/lectures/12-cor3-marked.pdf
- [5] Gebelein, Hans. "Das statistische Problem der Korrelation als Variations‐und Eigenwertproblem und sein Zusammenhang mit der Ausgleichsrechnung." ZAMM‐Journal of Applied Mathematics and Mechanics/Zeitschrift für Angewandte Mathematik und Mechanik 21.6 (1941): 364-379.
- [6] Reshef, David N., et al. "Detecting novel associations in large data sets." science 334.6062 (2011): 1518-1524.
