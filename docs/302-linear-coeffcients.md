# Regression coefficients

**Aims**

- to clarify the interpretation of the fitted linear models

**Learning outcomes**

- to use lm() function to fit multiple linear regression model
- to be able to interpret the output of the model
- to be able to use lm() function to check for association between variables, group effects and interaction terms



## Interpreting and using linear regression models
- In previous section we have seen how to find estimates of model coefficients, using theorems and vector-matrix notations. 
- Now, we will focus on what model coefficient values tell us and how to interpret them
- And we will look at the common cases of using linear regression models
- We will do this via analyzing some examples


## Example: plasma volume

```r
# data
weight <- c(58, 70, 74, 63.5, 62.0, 70.5, 71.0, 66.0) # body weight (kg)
plasma <- c(2.75, 2.86, 3.37, 2.76, 2.62, 3.49, 3.05, 3.12) # plasma volume (liters)

# fit regression model
model <- lm(plasma ~ weight)

# plot the original data and fitted regression line
plot(weight, plasma, pch=19, xlab="weight [kg]", ylab="plasma [l]")
lines(weight, model$fitted.values, col="red") # fitted model in red
grid()
```



\begin{center}\includegraphics{302-linear-coeffcients_files/figure-latex/plasma-volume-plot-1} \end{center}

```r

# print model summary
print(summary(model))
## 
## Call:
## lm(formula = plasma ~ weight)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -0.27880 -0.14178 -0.01928  0.13986  0.32939 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)  
## (Intercept)  0.08572    1.02400   0.084   0.9360  
## weight       0.04362    0.01527   2.857   0.0289 *
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.2188 on 6 degrees of freedom
## Multiple R-squared:  0.5763,	Adjusted R-squared:  0.5057 
## F-statistic:  8.16 on 1 and 6 DF,  p-value: 0.02893
```

**Model:** 

- $Y_i = \alpha + \beta x_i + \epsilon_i$ where $x_i$ corresponds to $weight_i$

**Slope**

- The value of slope tells us how and by much the outcome changes with a unit change in $x$
- If we go up in weight 1 kg what would be our expected change in plasma volume? 
_ Answer: it would increase by 0.04 liter
- And if we go up in weight 10 kg what would be our expected change in plasma volume?
- Answer: it would increase by $0.04 \cdot 10 = 0.4$ liter

**Intercept**

- the intercept, often labeled the constant, is the value of Y when $x_i=0$
- in models where $x_i$ can be equal 0, the intercept is simply the expected mean value of response
- in models where $x_i$ cannot be equal 0, like in our plasma example no weight makes no sense for healthy men, the intercept has no intrinsic meaning
- the intercept is thus quite often ignored in linear models, as it is the value of slope that dictates the association between exposure and outcome


## Example: Galapagos Islands
Researchers were interested in biological diversity on the Galapagos islands. They've collected data on number of plant species (Species) and number of endemic species on 30 islands as well as some descriptors of the islands such as area [$\mathrm{km^2}$], elevation [m], distance to nearest island [km], distance to Santa Cruz [km] and the area of the adjacent island [$\mathrm{km^2}$].  

The preview of data is here: 

```r
# Data is available via faraway package
if(!require(faraway)){
    install.packages("faraway")
    library(faraway)
}

head(gala)
##              Species Endemics  Area Elevation Nearest Scruz Adjacent
## Baltra            58       23 25.09       346     0.6   0.6     1.84
## Bartolome         31       21  1.24       109     0.6  26.3   572.33
## Caldwell           3        3  0.21       114     2.8  58.7     0.78
## Champion          25        9  0.10        46     1.9  47.4     0.18
## Coamano            2        1  0.05        77     1.9   1.9   903.82
## Daphne.Major      18       11  0.34       119     8.0   8.0     1.84
```

And we can fit a linear regression model to model number of Species given the remaining variables. Let's keep aside for now that number of Species is actually a count variable, not a continuous numerical variable, we just want to estimate the number of Species for now. 

```r
# fit multiple linear regression and print model summary
model1 <- lm(Species ~ Area + Elevation + Nearest + Scruz + Adjacent, data = gala)
print(summary(model1))
## 
## Call:
## lm(formula = Species ~ Area + Elevation + Nearest + Scruz + Adjacent, 
##     data = gala)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -111.679  -34.898   -7.862   33.460  182.584 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept)  7.068221  19.154198   0.369 0.715351    
## Area        -0.023938   0.022422  -1.068 0.296318    
## Elevation    0.319465   0.053663   5.953 3.82e-06 ***
## Nearest      0.009144   1.054136   0.009 0.993151    
## Scruz       -0.240524   0.215402  -1.117 0.275208    
## Adjacent    -0.074805   0.017700  -4.226 0.000297 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 60.98 on 24 degrees of freedom
## Multiple R-squared:  0.7658,	Adjusted R-squared:  0.7171 
## F-statistic:  15.7 on 5 and 24 DF,  p-value: 6.838e-07
```

**Model**

- $Y_i = \beta_0 + \beta_1 Area_i + \beta_2 Area_i + \beta_3 Elevation_i + \beta_4 Scruz_i + \beta_5 Adjacent_i + \epsilon_i$

**Slope**

- Compare two islands, where the second island has an elevation one meter higher than the first one, what can we say about the number of species according to the model?
- Answer: the second island will have 0.32 species more than the first one
- How about if the difference is 100 m? 
- Answer: now the second island will have $0.32 \cdot 100 = 32$ more species

**Not so easy**

- Consider an alternative model where we only use evaluation to model the number of species

```r
model2 <- lm(Species ~ Elevation, data = gala)
print(summary(model2))
## 
## Call:
## lm(formula = Species ~ Elevation, data = gala)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -218.319  -30.721  -14.690    4.634  259.180 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 11.33511   19.20529   0.590     0.56    
## Elevation    0.20079    0.03465   5.795 3.18e-06 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 78.66 on 28 degrees of freedom
## Multiple R-squared:  0.5454,	Adjusted R-squared:  0.5291 
## F-statistic: 33.59 on 1 and 28 DF,  p-value: 3.177e-06
```

**Model**

- $Y_i = \beta_0 + \beta_1 Elevation_i + \epsilon_i$

**Slope**

- Compare two islands, where the second island has an elevation one meter higher than the first one, what can we say about the number of species according to the model?
- Answer: the second island will have 0.20 species more than the first one
- How about if the difference is 100 m? 
- Answer: now the second island will have $0.20 \cdot 100 = 20$ more species

**Specific interpretation**

- obviously there is difference between 32 and 20 species given the same elevation difference and using the two different models
- our interpretations need to be more specific and we say
- **a unit increase in $x$ with other predictors held constant will produce a change equal to $\hat{\beta}$ in the response $y$**
- it is of course often quite unrealistic to be able to control other variables and keep them constant and for our simple regression, model 2, a change in evaluation is most likely associated with other variables, even though they are not included in the model
- further, our explanation contains **no notation of causation**, even thought the two models are showing a strong association between elevation and number of species
- we will learn later how to assess models and select variables (feature selection), here, we continue focusing on learning how to interpret the coefficients given a model

## Example: Height and gender
- Data are available containing the weight [lbs] and height [inches] of 10000 men and women
- We want to compare the average height of men and women
- We can do that using linear regression and including gender as **binary variable**

```r
# read in data
htwtgen <- read.csv("data/lm/heights_weights_genders.csv")
head(htwtgen)
##   Gender   Height   Weight
## 1   Male 73.84702 241.8936
## 2   Male 68.78190 162.3105
## 3   Male 74.11011 212.7409
## 4   Male 71.73098 220.0425
## 5   Male 69.88180 206.3498
## 6   Male 67.25302 152.2122

# boxplot for females and males
boxplot(htwtgen$Height ~ htwtgen$Gender, xlab="", ylab="Height", col="lightblue")
```



\begin{center}\includegraphics{302-linear-coeffcients_files/figure-latex/height-gender-1} \end{center}

**Model**

$$Y_i = \alpha + \beta I_{x_i} + \epsilon_i$$
where
\begin{equation}
    I_{x_i} =
    \left\{
        \begin{array}{cc}
                1 & \mathrm{if\ } x_i=1 \\
                0 & \mathrm{if\ } x_i=0 \\
        \end{array}
    \right.
\end{equation}
for some coding, e.g. we choose to set "Female=1" and "Male=0" or vice versa. 


```r
# fit linear regression and print model summary
model1 <- lm(Height ~ Gender, data = htwtgen)
print(summary(model1))
## 
## Call:
## lm(formula = Height ~ Gender, data = htwtgen)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -10.6194  -1.8374   0.0088   1.9185   9.9724 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 63.70877    0.03933  1619.8   <2e-16 ***
## GenderMale   5.31757    0.05562    95.6   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 2.781 on 9998 degrees of freedom
## Multiple R-squared:  0.4776,	Adjusted R-squared:  0.4775 
## F-statistic:  9140 on 1 and 9998 DF,  p-value: < 2.2e-16
```

**Estimates**
$$\hat{\alpha} = 63.71$$
$$\hat{\beta} = 5.32$$

- The lm() function choose one of the category as baseline, here Females
- model summary prints the output of the model with the baseline category "hidden"
- notice the only label we have is "GenderMale"
- meaning that we ended-up having a model coded as below:
\begin{equation}
    I_{x_i} =
    \left\{
        \begin{array}{cc}
                1 & \mathrm{if\ } \quad person_i\;is\;male \\
                0 & \mathrm{if\ } \quad person_i\;is\;female \\
        \end{array}
    \right.
\end{equation}
- Consequently, if observation $i$ is male then the expected value of height is: 
$$E(Height_i|Male) = 63.71 + 5.32 = 69.03$$
- and if observation $i$ is female then the expected value of height is: 
$$E(Height_i|Male) = 63.71$$

## Example: Heigth, weight and gender I
- So there is a difference in height between the gender, as expected
- Is there a relationship between weight and height? 
- If so, does this relationship depend on gender? 


```r
library(ggplot2)

# plot the data separately for Male and Female
ggplot(data=htwtgen, aes(x = Weight, y=Height, col = Gender)) + 
  geom_point(alpha = 0.5)
```



\begin{center}\includegraphics{302-linear-coeffcients_files/figure-latex/htwtgen-plot-1} \end{center}

- From the plot we can see that height increases with weight
- Males have higher heights than females
- Males have higher weights than females
- The relationship between height and weight appears to be the same for males and females, i.e. height increass with weight for both men and women

**Model**

$$Y_i = \alpha + \beta I_{x_i} + \gamma x_{2,i} + \epsilon_i$$
where
\begin{equation}
    I_{x_i} =
    \left\{
        \begin{array}{cc}
                1 & \mathrm{if\ } \quad person_i\;is\;male \\
                0 & \mathrm{if\ } \quad person_i\;is\;female \\
        \end{array}
    \right.
\end{equation}

and $x_{2,i}$ is the weight of person $i$


```r
# Fit linear model and print model summary
model2 <- lm(Height ~ Gender + Weight, data = htwtgen) 
print(summary(model2))
## 
## Call:
## lm(formula = Height ~ Gender + Weight, data = htwtgen)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -5.4956 -0.9583  0.0126  0.9867  5.8358 
## 
## Coefficients:
##               Estimate Std. Error t value Pr(>|t|)    
## (Intercept) 47.0306678  0.1025161  458.76   <2e-16 ***
## GenderMale  -0.9628643  0.0474947  -20.27   <2e-16 ***
## Weight       0.1227594  0.0007396  165.97   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 1.435 on 9997 degrees of freedom
## Multiple R-squared:  0.8609,	Adjusted R-squared:  0.8609 
## F-statistic: 3.093e+04 on 2 and 9997 DF,  p-value: < 2.2e-16
```
**Estimates**
$$\hat{\alpha} = 47.031$$
$$\hat{\beta} = -0.963$$
$$\hat{\gamma} = 0.123$$


- therefore, for a male of weight 161.4 we would predict a height of:
$$E(Height_i|Male, Weight = 161.4) = 47.031 - 0.963 + (0.123 \cdot 161.4) = 65.9$$
- and for a female of weight 161.4 we would predict a height of 
$$E(Height_i|Female, Weight = 161.4) = 47.031 + (0.123 \cdot 161.4) = 66.9$$
- as much as our calculations are correct, the above example also shows the need of considering the data when interpreting coefficients. Our expected female height at weight 161.4 is 66.9, larger than the men height, 65.9. However, our data show that the the majority of our values for female lie between 100 and 175 and between 150 and 250 for males, i.e. in different ranges
- we should **not predict outside the data range**
- finally, we can plot our data and the fitted model

```r
# plot the data separately for Male and Female
# using ggplot() and geom_smooth()
ggplot(data=htwtgen, aes(x = Weight, y=Height, col = Gender)) + 
  geom_point(alpha = 0.1) + 
  geom_smooth(method=lm)
```



\begin{center}\includegraphics{302-linear-coeffcients_files/figure-latex/unnamed-chunk-3-1} \end{center}

## Example: Heigth, weight and gender II
- The fitted lines in the above example are parallel, the slope is modeled to be the same for male and females, and the intercept denotes the group differences
- It is also possible to allow both intercept and slope being fitted separately for each group
- This is done when we except that the relationships are different in different groups
- And we then talk about including **interaction effect**

**Model**

$$Y_{i,j} = \alpha_i + \beta_ix_{ij} + \epsilon_{i,j}$$ 
where:

- $Y_{i,j}$ is the height of person $j$ of gender $i$
- $x_{ij}$ is the weight of person $j$ of gender $i$
- $i=1$ corresponds to males in our example (keeping the same coding as above)
- $i=2$ corresponds to females


```r
# fit linear model with interaction 
model3 <- lm(Height ~ Gender * Weight, data = htwtgen)
print(summary(model3))
## 
## Call:
## lm(formula = Height ~ Gender * Weight, data = htwtgen)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -5.4698 -0.9568  0.0092  0.9818  5.7544 
## 
## Coefficients:
##                    Estimate Std. Error t value Pr(>|t|)    
## (Intercept)       47.347783   0.146325 323.579  < 2e-16 ***
## GenderMale        -1.683668   0.242119  -6.954 3.78e-12 ***
## Weight             0.120425   0.001067 112.903  < 2e-16 ***
## GenderMale:Weight  0.004493   0.001480   3.036   0.0024 ** 
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 1.435 on 9996 degrees of freedom
## Multiple R-squared:  0.861,	Adjusted R-squared:  0.861 
## F-statistic: 2.064e+04 on 3 and 9996 DF,  p-value: < 2.2e-16
```

Now, based on the regression output we would expect:

- for a male of weight $x$, a height of:
$$E(height|male\; and \; weight=x)=47.34778 - 1.68367 + 0.12043x + 0.00449x = 45.7 + 0.125x$$
- for a female of weight $x$, a height of $$E(height|female\; and \; weight=x)=47.34778 + 0.12043x$$

**Estimates**
$$\hat{\alpha_1} = 45.7$$
$$\hat{\beta_1} = 0.125$$

$$\hat{\alpha_2} = 47.34778$$
$$\hat{\beta_2} = 0.12043$$

- we can see from the regression output that the interaction term, "GenderMale:Weight,  is significant 
- and therefore the relationship between weight and height is different in males and females
- we can plot the fitted model now and see that the lines are no longer parallel
- we will see clearer example of interactions in exercises


```r
# ggiraphExtra makes it easy to visualize fitted models
if(!require(ggiraphExtra)){
    install.packages("ggiraphExtra")
    library(ggiraphExtra)
}

ggPredict(model3)
```



\begin{center}\includegraphics{302-linear-coeffcients_files/figure-latex/unnamed-chunk-5-1} \end{center}

-------

## Exercises: linear models II

\BeginKnitrBlock{exercise}
<span class="exercise" id="exr:lm-rerun"><strong>(\#exr:lm-rerun) </strong></span>
Given the "height-weight-gender" data:

a) repeat fitting the models with a) gender, b) weight and gender and c) interaction between weight and gender
b) given the model with the interaction term, what is expected height of males and females give weight of 120 lbs?
c) can you use predict() function to check your calculations?

\EndKnitrBlock{exercise}


\BeginKnitrBlock{exercise}
<span class="exercise" id="exr:lm-trout"><strong>(\#exr:lm-trout) </strong></span>
When the behavior of a group of trout is studied, some fish are observed to become dominant and others to become subordinate. Dominant fish have freedom of movement whereas subordinate fish tend to congregate in the periphery of the waterway to avoid crossing the path of the dominant fish. Data on energy expenditure and ration of blood obtained were collected as part of a laboratory experiment for 20 trout. Energy and ration is measured in calories per kilo-calorie per trout per day.

Use the below code to load the data to R and use linear regression models to answer:

- a) is there a relationship between ration obtained and energy expenditure
- b) is the relationship between ration obtained and energy expenditure different for each type of fish?
- Hint: it is good to start with some explanatory plots between every pair of variable

\EndKnitrBlock{exercise}


```r
# read in data and show preview
trout <- read.csv("data/lm/trout.csv")

# recode the Group variable and treat like categories (factor)
trout$Group <- factor(trout$Group, labels=c("Dominant", "Subordinate"))
```

\BeginKnitrBlock{exercise}
<span class="exercise" id="exr:lm-blooddrug"><strong>(\#exr:lm-blooddrug) </strong></span>A clinical trial

A clinical trial has been carried out to compare three drug treatments which are intended to lower blood pressure in hypertensive patients. The data contains initial values fo systolic blood pressure (bp) in mmHg for each patient and the reduction achieved during the course of the trial. For each patient, allocation to treatment (drug) was carried out randomly and conditions such as the length of the treatment and dose of the drug were standardized as far as possible. 

Use linear regression to answer questions:

a) is there an association between the reduction in blood pressure and initial blood pressure 
b) is reduction in blood pressure different across the treatment (in three drug groups)?
c) is reduction in blood pressure different across the treatment when accounting for initial blood pressure? 
d) is reduction in blood pressure chaining differently under different treatment? 
Hint: here we have three categories which can be seen as expalnding the models with tow categories by an additional one: one category will be treated as baseline

\EndKnitrBlock{exercise}


```r
blooddrug <- read.csv("data/lm/bloodrug.csv")
blooddrug$drug <- factor(blooddrug$drug)
head(blooddrug)
```

```
##   initial redn drug
## 1     158    4    1
## 2     176   21    1
## 3     174   36    1
## 4     168   14    1
## 5     174   34    1
## 6     186   37    1
```


## Answers to selected exercises (linear models II) {-}

Exr. \@ref(exr:lm-rerun)

a)

```r
htwtgen <- read.csv("data/lm/heights_weights_genders.csv")
head(htwtgen)
##   Gender   Height   Weight
## 1   Male 73.84702 241.8936
## 2   Male 68.78190 162.3105
## 3   Male 74.11011 212.7409
## 4   Male 71.73098 220.0425
## 5   Male 69.88180 206.3498
## 6   Male 67.25302 152.2122

# a)
model1 <- lm(Height ~ Gender, data = htwtgen)
model2 <- lm(Height ~ Gender + Weight, data = htwtgen)
model3 <- lm(Height ~ Gender * Weight, data = htwtgen)

# print(summary(model1))
# print(summary(model2))
# print(summary(model3))
```

b) use equations to find the height for men and women respectively:
$$E(height|male\; and \; weight=x)=47.34778 - 1.68367 + 0.12043x + 0.00449x = 45.7 + 0.125x$$
$$E(height|female\; and \; weight=x)=47.34778 + 0.12043x$$

c)

```r
# for men
new.obs <- data.frame(Weight=120, Gender="Male")
predict(model3, newdata = new.obs)
##        1 
## 60.65427

# for female
new.obs <- data.frame(Weight=120, Gender="Female")
predict(model3, newdata = new.obs)
##        1 
## 61.79882
```

Exr. \@ref(exr:lm-trout)


```r
# read in data and show preview
trout <- read.csv("data/lm/trout.csv")

# recode the Group variable and treat like categories (factor)
trout$Group <- factor(trout$Group, labels=c("Dominant", "Subordinate"))
head(trout)
##   Energy Ration    Group
## 1  44.26  81.35 Dominant
## 2  67.16  91.68 Dominant
## 3  48.15  58.00 Dominant
## 4  34.53  58.63 Dominant
## 5  67.93  91.93 Dominant
## 6  72.45  96.56 Dominant

# plot data
# boxplots of Energy and Ration per group
boxplot(trout$Energy ~ trout$Group, xlab="", ylab="Energy")
```



\begin{center}\includegraphics{302-linear-coeffcients_files/figure-latex/unnamed-chunk-10-1} \end{center}

```r
boxplot(trout$Ration ~ trout$Group, xlab="", ylab="Ration")
```



\begin{center}\includegraphics{302-linear-coeffcients_files/figure-latex/unnamed-chunk-10-2} \end{center}

```r

# scatter plot of Ration vs. Energy
plot(trout$Ration, trout$Energy, pch=19, xlab="Ration", ylab="Energy")
```



\begin{center}\includegraphics{302-linear-coeffcients_files/figure-latex/unnamed-chunk-10-3} \end{center}

- From the exploratory plots we see that there is some sort of relationship between ratio and energy, i.e. energy increase while ration obtained increases
- From boxplots we see that the ration obtained may be different in two groups


```r
# Is there a relationship between ration obtained and energy expenditure
model1 <- lm(Energy ~ Ration, data = trout)
print(summary(model1))
## 
## Call:
## lm(formula = Energy ~ Ration, data = trout)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -18.704  -4.703  -0.578   2.432  33.506 
## 
## Coefficients:
##             Estimate Std. Error t value Pr(>|t|)    
## (Intercept)   4.3037    12.5156   0.344 0.734930    
## Ration        0.7211     0.1716   4.203 0.000535 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 12.05 on 18 degrees of freedom
## Multiple R-squared:  0.4953,	Adjusted R-squared:  0.4673 
## F-statistic: 17.66 on 1 and 18 DF,  p-value: 0.0005348
# from the regression output we can see that yes, a unit increase in ratio increase energy expenditure by 0.72

# Is there a relationship between ration obtained and energy expenditure different for each type of fish?
# we first check if there is a group effect
model2 <- lm(Energy ~ Ration + Group, data = trout)
print(summary(model2))
## 
## Call:
## lm(formula = Energy ~ Ration + Group, data = trout)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -13.130  -5.139  -0.870   2.199  25.622 
## 
## Coefficients:
##                  Estimate Std. Error t value Pr(>|t|)    
## (Intercept)      -24.8506    13.3031  -1.868  0.07910 .  
## Ration             1.0109     0.1626   6.218 9.36e-06 ***
## GroupSubordinate  17.0120     5.1075   3.331  0.00396 ** 
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 9.647 on 17 degrees of freedom
## Multiple R-squared:  0.6946,	Adjusted R-squared:  0.6587 
## F-statistic: 19.33 on 2 and 17 DF,  p-value: 4.182e-05
ggPredict(model2)
```



\begin{center}\includegraphics{302-linear-coeffcients_files/figure-latex/unnamed-chunk-11-1} \end{center}

```r

# and whether there is interaction effect
model3 <- lm(Energy ~ Ration * Group, data = trout)
print(summary(model3))
## 
## Call:
## lm(formula = Energy ~ Ration * Group, data = trout)
## 
## Residuals:
##      Min       1Q   Median       3Q      Max 
## -12.7951  -6.0981  -0.1554   3.9612  23.5946 
## 
## Coefficients:
##                         Estimate Std. Error t value Pr(>|t|)    
## (Intercept)              -9.2330    15.9394  -0.579 0.570483    
## Ration                    0.8149     0.1968   4.141 0.000767 ***
## GroupSubordinate        -18.9558    22.6934  -0.835 0.415848    
## Ration:GroupSubordinate   0.5200     0.3204   1.623 0.124148    
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 9.214 on 16 degrees of freedom
## Multiple R-squared:  0.7378,	Adjusted R-squared:  0.6886 
## F-statistic:    15 on 3 and 16 DF,  p-value: 6.537e-05
ggPredict(model3)
```



\begin{center}\includegraphics{302-linear-coeffcients_files/figure-latex/unnamed-chunk-11-2} \end{center}

Based on the regression output and plots we can say:

- there is relationship between ration obtained and energy expenditure
- that this relationship is the same in the two groups although the energy expenditure is higher in the dominant fish



