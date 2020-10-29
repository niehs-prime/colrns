# `colrns.glm`

Correlation Learning for Variable Selection Generalized Linear Model (COLRNS-GLM)


## Description

This function allows you to implement COLRNS-GLM for high dimensional data analysis with binary responses.
 This package is based on the 'glmnet' package (Friedman et al., 2019)


## Usage

```r
colrns.glm(
  x,
  y,
  penalty.factor = rep(1, nvars),
  nfolds = 10,
  type.measure = "deviance",
  alpha = seq(0.1, 0.9, by = 0.1),
  lambda.min = TRUE,
  cutoff = 0.5,
  r.group = 0.3,
  niter = 150
)
```


## Arguments

Argument      |Description
------------- |----------------
`x`     |     Design matrix including predictors and confounders
`y`     |     Response (binary)
`penalty.factor`     |     Separate penalty factor can be applied to each coefficient. It can be 0 for some variables. If 0, corresponding variables are always included in the model. Default is 1 for all variables.
`nfolds`     |     Number of folds that is used in cross-validation. Default is 10.
`type.measure`     |     Measurement to be optimized in cross-validation. Default is "deviance", which uses squared-error for gaussian models (a.k.a type.measure="mse"). Mean absolute error "mae" is another option.
`alpha`     |     Grid for the parameter alpha, the proportion between the lasso penalty (alpha=1) and the ridge penalty (alpha=0). Default is from 0.1 to 0.9 with the interval of 0.1.
`lambda.min`     |     Whether to estimate the coefficients at lambda.min or lambda.1se. Using lambda.1se tends to have a more sparse solution. Default is lambda.min.
`cutoff`     |     Cutoff to construct the lead cluster. Default is 0.5.
`r.group`     |     Minimum ratio of predictors to be included in the lead cluster. The minimum number of predictors recruited in the lead cluster d can be n-1 or n/log(n) (Fan and Lv, 2008), or 2n/log(n) (Zhong and Zhu, 2015). The minimum ratio can be d/number of total predictors*100. When sample size n is high, we recommend to use between 0.2 and 0.4. Default is 0.3.
`niter`     |     Number of iterative procedures. Default is 150.


## Value

coefficients: A named vector of coefficients.
 
 select.vars: A list of selected variables, that is, the variables with non-zero coefficients.


## References

Hastie, T., Tibshirani, R., and Friedman, J. (2009). The Elements of Statistical Learning: Data Mining, Inference, and Prediction, 2nd Edition. Springer, New York
 
 Hastie, T., Tibshirani, R., and Wainwright, M. (2015). Statistical Learning with Sparsityy: the LASSO and Generalizations. Boca Raton, FL, USA: CRC Press.
 
 Friedman, J., Hastie, T., Tibshirani, R., Narasimhan, B., Simon, N., and Qian, J. (2019). Lasso and elastic-net regularized generalized linear models.
 
 Jang, J. (2020). Variable Selection in Presence of Strong Collinearity with Application to Environmental Mixtures. PhD Dissertation, University of Illinois at Chicago


## Examples

```r
### Simulated Data
library(MASS)
set.seed(1)
n=500
p=30
Cov.x <- matrix(0.2, ncol=p, nrow=p)
gp.cov <- matrix(0.6, ncol=10, nrow=10)
diag(gp.cov) <- 1
for(i in 1:3){
Cov.x[{10*(i-1)+1}:{10*i},{10*(i-1)+1}:{10*i}] <- gp.cov
}
x_mu = matrix(0,p,1)
x = mvrnorm(n, x_mu, Cov.x)
colnames(x)<-paste("x",1:p,sep="")
beta=rep(c(1,1,1,rep(0,7)),3)
prob=exp(x%*%beta)/(1+exp(x%*%beta))
y=rbinom(n, 1, prob)
### Fit the model
colrns.glm(x, y, lambda.min=FALSE)
```

