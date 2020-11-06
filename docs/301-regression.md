# (PART) Regression {-}

# Simple linear regression

**Aims**

- to introduce concept of linear regression

**Learning outcomes**

- to understand simple linear regression model incl. terminology
- to be able to state linear model in the general vector-matrix notation
- to use the general vector-matrix notation to numerically estimate model parameters


## Linear Regression

- Linear regression is a simple approach for supervised learning, when numerical outcome and numerical exposure are concerned
- The method of linear regression is used to model the statistical relationship between two variables (the outcome and the exposure)
- In practice, it results in an estimate of the best-fitting straight line to describe the relationship, also called the association


With linear regression we can answer questions such as:

  - is there a relationship between exposure and outcome, e.g. body weight and plasma volume?
  - how strong is the relationship between the two variables?
  - what will be a predicted value of the outcome given a new set of exposure values?
  - how accurately can we predict outcome?

<br />  


## Statistical vs. deterministic relationship

Relationships in probability and statistics can generally be one of three things: deterministic, random, or statistical:

- a **deterministic** relationship involves **an exact relationship** between two variables, for instance Fahrenheit and Celcius degrees is defined by an equation $Fahrenheit=\frac{9}{5}\cdot Celcius+32$
- there is **no relationship** between variables in the **random relationship**, for instance number of succulents Olga buys and time of the year as Olga keeps buying succulents whenever she feels like it throughout the entire year
- **a statistical relationship** is a **mixture of deterministic and random relationship**, e.g. the savings that Olga has left in the bank account depend on Olga's monthly salary income (deterministic part) and the money spent on buying succulents (random part)


\begin{figure}

{\centering \includegraphics{301-regression_files/figure-latex/regression-deterministic-1} 

}

\caption{Deterministic vs. statistical relationship: a) deterministic: equation exactly describes the relationship between the two variables e.g. Ferenheit and Celcius relationship ; b) statistical relationship between $x$ and $y$ is not perfect (increasing relationship), c)  statistical relationship between $x$ and $y$ is not perfect (decreasing relationship), d) random signal}(\#fig:regression-deterministic)
\end{figure}
  

## The best-fitting straight line

Let's look at the example data containing body weight (kg) and plasma volume (liters) for eight healthy men to see what it the best-fitting straight line is. 

Example data:

```r
weight <- c(58, 70, 74, 63.5, 62.0, 70.5, 71.0, 66.0) # body weight (kg)
plasma <- c(2.75, 2.86, 3.37, 2.76, 2.62, 3.49, 3.05, 3.12) # plasma volume (liters)
```


\begin{figure}

{\centering \includegraphics{301-regression_files/figure-latex/fig-intro-example-1} 

}

\caption{Scatter plot of the data shows that high plasma volume tends to be associated with high weight and *vice verca*.}(\#fig:fig-intro-example)
\end{figure}


\begin{figure}

{\centering \includegraphics{301-regression_files/figure-latex/fig-intro-example-reg-1} 

}

\caption{Scatter plot of the data shows that high plasma volume tends to be associated with high weight and *vice verca*. Linear regression gives the equation of the straight line (red) that best describes how the outcome changes (increase or decreases) with a change of exposure variable}(\#fig:fig-intro-example-reg)
\end{figure}

The equation for the red line is:
$$Y_i=0.086 +  0.044 \cdot x_i \quad for \;i = 1 \dots 8$$ 
and in general: 
$$Y_i=\alpha + \beta \cdot x_i \quad for \; i = 1 \dots n$$ 

## Simple linear regression model 
- In other words, by finding the best-fitting straight line we are **building a statistical model** to represent the relationship between plasma volume ($Y$) and explanatory body weight variable ($x$)
- If were to use our model $Y_i=0.086 + 0.044 \cdot x_i$ to find plasma volume given a weight of 58 kg (our first observation, $i=1$), we would notice that we would get $Y=0.086 +  0.044 \cdot 58 = 2.638$, not exactly $2.75$ as we have for our first man in our dataset that we started with, i.e. $2.75 - 2.638 = 0.112 \neq 0$. 
- We thus add to the above equation an **error term** to account for this and now we can write our regression model more formally as:

\begin{equation}
Y_i=\alpha + \beta \cdot x_i + \epsilon_i
(\#eq:regression-linear)
\end{equation}

- where we call $\alpha$ and $\beta$ **model coefficients**
- where we call $\epsilon_i$ **error terms**


## Least squares
- in the above body weight - plasma volume example, the values of $\alpha$ and $\beta$ have just appeared
- in practice, $\alpha$ and $\beta$ values are unknown and we use data to estimate these coefficients, noting the estimates with a hat, $\hat{\alpha}$ and $\hat{\beta}$
- **least squares** is one of the methods of parameters estimation, i.e. finding $\hat{\alpha}$ and $\hat{\beta}$

\begin{figure}

{\centering \includegraphics{301-regression_files/figure-latex/regression-errors-1} 

}

\caption{Scatter plot of the data shows that high plasma volume tends to be associated with high weight and *vice verca*. Linear regrssion gives the equation of the straight line (red) that best describes how the outcome changes with a change of exposure variable. Blue lines represent error terms, the vertical distances to the regression line}(\#fig:regression-errors)
\end{figure}


<br />  
Let $\hat{y_i}=\hat{\alpha} + \hat{\beta}x_i$ be the prediction $y_i$ based on the $i$-th value of $x$:

- Then $\epsilon_i = y_i - \hat{y_i}$ represents the $i$-th **residual**, i.e. the difference between the $i$-th observed response value and the $i$-th response value that is predicted by the linear model
- RSS, the **residual sum of squares** is defined as: $$RSS = \epsilon_1^2 + \epsilon_2^2 + \dots + \epsilon_n^2$$ or
equivalently as: $$RSS=(y_1-\hat{\alpha}-\hat{\beta}x_1)^2+(y_2-\hat{\alpha}-\hat{\beta}x_2)^2+...+(y_n-\hat{\alpha}-\hat{\beta}x_n)^2$$
- the least squares approach chooses $\hat{\alpha}$ and $\hat{\beta}$ **to minimize the RSS**. With some calculus we get: 


<br />  
\BeginKnitrBlock{theorem}\iffalse{-91-76-101-97-115-116-32-115-113-117-97-114-101-115-32-101-115-116-105-109-97-116-101-115-32-102-111-114-32-97-32-115-105-109-112-108-101-32-108-105-110-101-97-114-32-114-101-103-114-101-115-115-105-111-110-93-}\fi{}
<span class="theorem" id="thm:unnamed-chunk-2"><strong>(\#thm:unnamed-chunk-2)  \iffalse (Least squares estimates for a simple linear regression) \fi{} </strong></span>
$$\hat{\beta} = \frac{S_{xy}}{S_{xx}}$$
$$\hat{\alpha} = \bar{y}-\frac{S_{xy}}{S_{xx}}\cdot \bar{x}$$
  
where:
  
- $\bar{x}$: mean value of $x$
- $\bar{y}$: mean value of $y$
- $S{xx}$: sum of squares of $X$ defined as $S_{xx} = \displaystyle \sum_{i=1}^{n}(x_i-\bar{x})^2$
- $S{yy}$: sum of squares of $Y$ defined as  $S_{yy} = \displaystyle \sum_{i=1}^{n}(y_i-\bar{y})^2$
- $S{xy}$: sum of products of $X$ and $Y$ defined as $S_{xy} = \displaystyle \sum_{i=1}^{n}(x_i-\bar{x})(y_i-\bar{y})$
  
\EndKnitrBlock{theorem}



We can further re-write the above sum of squares to obtain

- sum of squares of $X$, $$S_{xx} = \displaystyle \sum_{i=1}^{n}(x_i-\bar{x})^2 = \sum_{i=1}^{n}x_i^2-\frac{(\sum_{i=1}^{n}x_i)^2}{n})$$
- sum of products of $X$ and $Y$

$$S_{xy} = \displaystyle \sum_{i=1}^{n}(x_i-\bar{x})(y_i-\bar{y})=\sum_{i=1}^nx_iy_i-\frac{\sum_{i=1}^{n}x_i\sum_{i=1}^{n}y_i}{n}$$
<br />  


#### Example

Let's try least squares method to find coefficient estimates in our body weight and plasma volume example

```r
# initial data
weight <- c(58, 70, 74, 63.5, 62.0, 70.5, 71.0, 66.0) # body weight (kg)
plasma <- c(2.75, 2.86, 3.37, 2.76, 2.62, 3.49, 3.05, 3.12) # plasma volume (liters)

# rename variables for convenience
x <- weight
y <- plasma

# mean values of x and y
x.bar <- mean(x)
y.bar <- mean(y)

# Sum of squares
Sxx <-  sum((x - x.bar)^2)
Sxy <- sum((x-x.bar)*(y-y.bar))

# Coefficient estimates
beta.hat <- Sxy / Sxx
alpha.hat <- y.bar - Sxy/Sxx*x.bar

# Print estimated coefficients alpha and beta
print(alpha.hat)
## [1] 0.08572428
print(beta.hat)
## [1] 0.04361534
```

In R we can use `lm`, the built-in function,  to fit a linear regression model and we can replace the above code with one line


```r
lm(plasma ~ weight)
## 
## Call:
## lm(formula = plasma ~ weight)
## 
## Coefficients:
## (Intercept)       weight  
##     0.08572      0.04362
```

