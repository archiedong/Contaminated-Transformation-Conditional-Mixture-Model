<p align="center">
  <img src = "https://user-images.githubusercontent.com/60518209/219705580-ffb94e46-e520-45ac-9ec6-58bab4196e17.png" width = "630" />
</p>

# Contaminated-Transformation-Conditional-Mixture-
We propose a contaminated transformation conditional mixture model and demonstrate on a series of simulation studies that it can effectively account for skewness and heavy tails.

## Abstract
Overparameterization is a serious concern for multivariate mixture models as it can lead to
model overfitting and, as a result, mixture order underestimation. Parsimonious modeling is
one of the most effective remedies in this context. In Gaussian mixture models, the majority
of parameters is associated with covariance matrices and parsimonious models based on factor
analyzers and spectral decomposition of dispersion parameters are the most popular in literature.
Some drawbacks of these models include the lack of flexibility in imposing different
covariance structures for individual components and limitations in modeling compact clusters.
Recently introduced conditional mixture models provide substantial flexibility in addressing
these concerns. The components of such mixtures are formulated as a product of conditional
distributions with univariate Gaussian densities being the primary choice. However, the presence
of heavy tails or skewness in any dimension can lead to fitting problems. We propose
a flexible model that is free of the above-mentioned limitations and name it a contaminated
transformation conditional mixture model and demonstrate on a series of simulation studies
that it can effectively account for skewness and heavy tails. Applications to real-life data sets
show good results and highlight the promise of the proposed model. 


## Methodology
Let $X_1, \cdots, X_n$ represents a random sample consisting of $n$ $p$-variate observations distributed according to a finite mixture model with pdf of the form,

```math
g(x; \Theta) = \sum \pi_k h(x; \vartheta_k).
```
In this expression, $K$ represents the number of components $h(x;\vartheta_k)$ in the mixture, $\vartheta_k$ stands for the component-specific parameter vector, and $\pi_k$ is the $k^{th}$ mixing proportion, subject to restrictions $\sum \pi_k = 1$ and $0 < \pi_k \le 1$. 
Popular choices of the component $h(x; \vartheta_k)$ include multivariate Gaussian, t, skew-normal, and skew-t. All these distributions include a covariance matrix with a potentially large number of parameters. As discussed, potential overparameterization is one of the major concerns in the mixture modeling framework. It can lead to model overfitting and, as a result, mixture order underestimation. Traditional parsimonious models aim at reducing the number of model parameters by considering specific structures of covariance matrices. 

Previous work has been done on an alternative parameterization with the primary attention paid to location rather than scale parameters. Note that the joint pdf $h(x;\vartheta_k)$ can be written as the product of a marginal and $p - 1$ conditional distributions $f_1(x_1; \zeta_{k1}) \prod f_j(x_j|x_1, \cdots, x_{j-1};\zeta_{kj})$, where $\zeta_{kj}$ is the parameter vector of the $j^{th}$ distribution within the $k^{th}$ component. While various forms of the pdf $f_j$ can be considered, one natural and mathmatically convenient choice is the univariate normal pdf. The proposed mixture model can be written as

```math
g(x; \Theta) = \sum_{k=1} ^{K} \pi_k \prod_{j=1}^p \phi(x_j;\tilde{x}_j^\top \beta_{kj}, \sigma_{kj}^2).
```
The estimation of the parameter vector $\Theta = {\pi_1, \cdots, \pi_{K-1}, \vartheta_1, \cdots, \vartheta_K}$ is traditionally performed by means of the the expectation-maximization (EM) algorithm. At the E-step, the conditional expected value of the complete-data log-likelihood function is estimated. This expectation is traditionally referred to as the $Q$ function. At the M step, the $Q$ function is maximized with respect to $\Theta$. The procedure stops when some pre-specified convergence criterion is met, yielding the maximum likelihood estimate $\hat{\Theta}$ and estimated posterior probabilities $\hat{\tau}_{ik}$ for $i = 1, \cdots, n, k = 1, \cdots, K$. Often times the mixture order $K$ is unknown and also required to be assessed; consequently a common approach of choosing an appropriate $K$ is based on minimizing the Bayesian information criterion (BIC) over different mixture orders.

With the Yeo and Johnson transformation in Equation~\ref{eq:Yeo}), the joint density function in Equation becomes 
```math
\begin{split}
f(x;\theta)  =& [\delta \phi(\mathcal{T}(x; \lambda);\mu, \Sigma) + (1 - \delta) \phi(\mathcal{T}(x; \lambda);\mu, \alpha \Sigma)]  J_{\mathcal{T}}(x;\lambda) \\ 
& = [\delta \prod_{j=1}^p \phi(\mathcal{T}(x_{j}; \lambda_j);\{\hat{x}_{j}^m\}^\top \beta_{j}, \sigma^2_j) + (1 - \delta) \prod_{j=1}^p \phi(\mathcal{T}(x_{j}; \lambda_j);\{\hat{x}_{j}^m\}^\top \beta_{j}, \alpha \sigma^2_j)] \prod_{j = 1}^P J_{\mathcal{T}}(x_j;\lambda_j) \\ 
& = [\delta \phi_1 (\mathcal{T}(x_1; \lambda_1);\{\hat{x}_1^m\}^\top \beta_1, \sigma_1^2) \cdots \phi_p(\mathcal{T}(x_p; \lambda_p); \{\hat{x}_p^m\}^\top \beta_p, \sigma_p^2) \\ 
&+ (1 - \delta) \phi_1 (\mathcal{T}(x_1; \lambda_1); \{\hat{x}_1^m\}^\top \beta_1, \alpha \sigma_1^2) \cdots \phi_p(\mathcal{T}(x_p; \lambda_p); \{\hat{x}_p^m\}^\top \beta_p, \alpha \sigma_p^2)] 
\prod_{j = 1}^P J_{\mathcal{T}}(x_j;\lambda_j),
\end{split}
```
Assuming all membership labels $Z_i$, $i = 1, \ldots, n$ are known, i.e. the complete data is given by $(X_i, Z_i)$, the corresponding complete-data likelihood function can be then rewritten as

```math
L(\Psi) = \prod_{i = 1}^n \prod_{k = 1}^K \prod_{w = 1}^2 [ \pi_k [ \delta_{kw} \phi ( \mathcal{T} ( x_i; \lambda_{ jk } ); \mu_k, \Sigma_{kw} ) J_{ \mathcal{T} } ( x_i; \lambda_{ jk } ) ]^{ I(W_i = w) }]^{I(Z_i = k) },
```
where $I(Z_i = k) = 1$ if $Z_i$ belongs to the $k$th component and 0 otherwise; similarly $I(W_i = w)$ indicates if the ith observation from the kth component is contaminated. At the E-step of the algorithm, the conditional expectation of the complete-data log-likelihood function obtained from Eq requires updating posterior probabilities according to the following expressions:

```math
\ddot{\tau}_{ik} = \frac{\dot{\pi}_k [\dot{\delta}_k \prod \phi(\mathcal{T}(x_{ij}; \dot{\lambda}_{jk}); \{\tilde{x}_{ij}^m\}^\top \dot{\beta}_{jk}, \dot{\sigma}_{jk}^2) + (1 - \dot{\delta}_k) \prod \phi(\mathcal{T}(x_{ij}; \dot{\lambda}_{jk});\{\tilde{x}_{ij}^m\}^\top \dot{\beta}_{jk}, \dot{\alpha}_k \dot{\sigma}_{jk}^2)]  \prod J_{\mathcal{T}}(x_{ij};\lambda_{jk})} {\sum_{r=1}^K \dot{\pi}_k [\dot{\delta}_r \prod \phi(\mathcal{T}(x_{ij}; \dot{\lambda}_{jr});\{\tilde{x}_{ij}^m\}^\top \dot{\beta}_{jr}, \dot{\sigma}_{jr}^2) + (1 - \dot{\delta}_r) \prod \phi(\mathcal{T}(x_{ij}; \dot{\lambda}_{jr});\{\tilde{x}_{ij}^m\}^\top \dot{\beta}_{jr}, \dot{\alpha}_r \dot{\sigma}_{jr}^2)]  \prod J_{\mathcal{T}}(x_{ij};\lambda_{jr})}
```
and

```math
\ddot{\nu}_{i|k} = \frac{\dot{\delta}_k \prod \phi(\mathcal{T}(x_{ij}; \dot{\lambda}_{jk});\{\tilde{x}_{ij}^m\}^\top \dot{\beta}_{jk}, \dot{\sigma}_{jk}^2)} { \dot{\delta}_k \prod \phi(\mathcal{T}(x_{ij}; \dot{\lambda}_{jk});\{\tilde{x}_{ij}^m\}^\top \dot{\beta}_{jk}, \dot{\sigma}_{jk}^2) + (1 - \dot{\delta}_k) \prod \phi(\mathcal{T}(x_{ij}; \dot{\lambda}_{jk});\{\tilde{x}_{ij}^m\}^\top \dot{\beta}_{jk}, \dot{\alpha}_k \dot{\sigma}_{jk}^2)},
```
where $\tau_{ik}$ is the probability that $x_i$ originates from the $k^{th}$ mixture component and $\nu_{i|k}$ is the probability that $x_i$ belongs to the primary distribution within the $k^{th}$ component. In other words, $\nu_{i|k}$ is the probability that $x_i$ is not contanimated in the $k^{th}$ component. One dot and two dots on the top of parameters stand for estimates at the previous and current iterations, respectively. 
In the M-step, taking partial derivatives of this Q-function with respect to $\pi_k$ and $\delta_k$ leads to the following expressions

```math
\ddot{\pi}_k = \frac{\sum_{i = 1} ^ n \ddot{\tau}_{ik}}{n} \text{ and } \ddot{\delta_k} = \frac{\sum_{i=1}^n \ddot{\tau_{ik}} \ddot{\nu}_{i|k}}{\sum_{i = 1} ^ n \ddot{\tau}_{ik}}.
```
The $\pi$ and $\delta$ can be obtained from the MLE of posterior probabilities $\pi \equiv \pi_{ik}(\dot{\Psi})$ according to the Bayes decision rule. The other parameters except for $\lambda$ all have closed forms that improve the efficacy of the EM algorithm, and they can be derived by

```math
\ddot{\beta}_{jk} = \left(\sum \ddot{\tau}_{ir} \left( 1 + \ddot{\nu}_{i|r} (\dot{\alpha}_r - 1) \right)  \tilde{x}_{ij}^m  \{\tilde{x}_{ij}^m\}^\top \right)^{-1} {\sum \ddot{\tau}_{ik} \left(1 + \ddot{\nu}_{i|k} (\dot{\alpha}_k - 1)\right) \mathcal{T}(x_{ij}; \ddot{\lambda}_{jk}) \tilde{x}_{ij}^m}
```

```math
\ddot{\sigma}_{jk}^2 = \frac{\sum \ddot{\tau}_{ik} \left(1 + \ddot{\nu}_{i|k} (\dot{\alpha}_k - 1) \right) \left( \mathcal{T}(x_{ij}; \ddot{\lambda}_{jk}) - \{\tilde{x}_{ij}^m\}^\top \ddot{\beta}_{jk} \right)^2}{\sum \ddot{\tau}_{ik} \dot{\alpha}_k}
```
and

```math
\ddot{\alpha}_k = \frac{\sum \sum \ddot{\tau}_{ik} ( 1 - \ddot{\nu}_{i|k}) \ddot{\sigma}_{jk}^{-2} \left( \mathcal{T}(x_{ij}; \ddot{\lambda}_{jk}) - \{\tilde{x}_{ij}^m\}^\top \ddot{\beta}_{jk} \right)^2}{p \sum \ddot{\tau}_{ik} ( 1- \ddot{\nu}_{i|k})}
```
The $\lambda$'s can be obtained numerically by a variety of optimization procedures. The EM algorithm continues until the convergence criterion is met. In this paper, we employed a stopping criterion based on the relative change of log-likelihood values from two consecutive iterations of the EM algorithm.

# simulation
In this section, we provide experimental validation for the proposed method and compare its performace with other four stata-of-the-art model-based clustering techniques developed in the following R packages: HDclassif, mixggm, mclust and cmbClust. To assess the clustering performance, adjusted Rand index (ARI) is employed as it measures the degree of agreement between two partitions. Note that the upper bound of ARI is $1$, which represents the perfect match in the partitions. In our experiments, we assessed the similarity of the original and estimated classifictions. We considered cases when $p= 4$ and assume three-component conditional mixture models ($K = 3$) with parameters provided in the Supplement. To increase the complexity of model selection, many linear model coefficients were set to zero to observe if the trivial predictors could be discarded. To investigate the potential effect of the sample size $n$ on the performance of the algorithms, $200$ data sets of each size $300$ and $1000$ were simulated. 

The results of the experiments involving competing procedures are presented in Table 2. Notice that we did not assume that the correct mixture order was known, and hence the means of BICs and ARIs were provided for a arange of $K$ values. The results corresponding to the best BIC were highlighted with the bold font. The last column in the table provided the rank of the best BIC and corresponding ARI values for individual five competing procedures. In all settings, ctcmbClust employed the order selection procedure outlined in Section 2.5, i.e., the correct order was assumed to be unknown and the search over all possible conditioning orders was not carried out. As observed from BIC values, ctcmbClust ourperformed the competitors in all settings dramatically. It was also capable of detecting the correct number of the components. The other procedures, with the exception of cmbClust in the second setting, chose six-component mixtures. This is at least partially related to the presence of the skewness data and outliers. ARI values produced by ctcmbClust were consistently the highest. In the majority of the settings, mclust and mixggm showed relatively similar performances. The conducted simulation study demonstrated that the proposed procedure was highly competitive in the considered settings.

<p align="center">
  <img src="https://user-images.githubusercontent.com/60518209/219717035-16721026-7153-42d2-97f7-1f949eba7373.png" width = "630" />
</p>

<p align="center">
  <img src="https://user-images.githubusercontent.com/60518209/219717167-10e82e29-f5be-4e4d-ba74-60c846b82723.png" width = "630" />
</p>

## Application


In this section, we compare the performance of the five procedures on three well-known classification data sets. The sets are chosen to have various numbers of clusters and dimensionality. The results of the study are provided in Table 4. As before, parameters $p$ and $n$ represent the dimensionality and sample size. $\tilde{K}$ denotes the presumed number of clusters based on the available class information.

The Banknote Authentication dataset is a well-known set of benchmark data for binary clustering problems. The data waere extracted from images that were taken from genuine and forged banknote-like specimens. For digitization, an industrial camera usually used for print inspection was used. The final images have $400 \times 400$ pixels. Due to the object lens and distance to the investigated object, gray-scale pictures with a resolution of about $660$ dpi were gained. Wavelet transformation tool were used to extract features from images, such as variance, skwness, curtosis of wavelet transformed and image entropy. The dataset set consists of $1372$ items and the goal is to predict whether a banknote is authentic/real/geniune or a forgery/fake based on the four predictor values. From the correlation map and pairplot, we can see there is a multicolliniraty between curtosis and skewness of wavelet transformed. From the histgram, there exists skewness and outliers, especially in entropy and curtosis columns.
The results provided in Table 2 indicate that the best performer is ctcmbClust with $\hat{K} = 3$. The correponding BIC is $23673.14$ and associated ARI is $1$ and is best among the competitors. Despite the other performers with lower BIC, all could not correctly find $\hat{K} = 2$.

<p align="center">
  <img src="https://user-images.githubusercontent.com/60518209/219717699-1c33758e-cc48-4492-8b15-9ef738a47ff1.png" width = "630" />
</p>

<p align="center">
  <img src="https://user-images.githubusercontent.com/60518209/219717783-210efca7-b9d0-4dfa-9227-983eccf3c5eb.png" width = "630" />
</p>

The wine data set consists of $13$ different parameters of wine such as alcohol and ash content which was measured for $178$ wine samples. These wines were grown in the same region in Italy but derived from three different cultivars; therefore there are three different classes of wine. The goal here is to find a model that can predict the class of wine given the $13$ measured parameters. Table 2 represents the results of applying the five models considered in this paper. The best model is detected by ctcmbClust for a $\hat{K} = 2$ (BIC $6566.58$). From the corresponding ARI value of 1, the wine data has been perfectly clustered by the proposed method. The second best is cmbClust with three components of BIC $6418.03$ and ARI $0.982$. The next performer is mixggm with BIC $6624.35$ and ARI $0.869$ also for a three-component mixture. The next performer is mclust (VVE parameterization) with BIC $6849.39$ and ARI $0.967$. The estimated mixture order is $\hat{K} = 2$ as well. Finally, HDclassif detects a six-component mixture with BIC $11624.74$ and ARI $0.220$. In terms of BIC, this solution is much worse than the models chose by the other three procedures.
