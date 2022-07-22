------------------------------------------------------------------------

``` {.r}
> i <- 1
```

# Case study

## Reproduction system and dimorphism

We study the floral and reproductive biology of a endemic tree species
of Reunion island (*Dombeya ciliata*, Malvaceae) in three different
populations: Mare Longue (ML, elevation: 400 m), Maido (MA, 1200 m) and
Takamaka (TA, 700 m).

We address the following questions:

-   What is the apparent reproduction system of the focal studies?
-   Is there sexual dimorphism in the number of flowers per
    inflorescence and petal length?
-   Does the reproduction system and sexual dimorphism of sexual
    characters vary according to populations?

```{=html}
<!--

\begin{figure}
\centering
\begin{tabular}{cc}
a. & b. \\
\includegraphics[height=5cm]{Biostats/dombeya_ciliata_11_reference} & 
\includegraphics[height=5cm]{Biostats/dombeya_ciliata_41_reference}
\end{tabular}
\caption{a) Individual tree of {\em Dombeya ciliata} (Malvaceae), b) Inflorescence of {\em Dombeya ciliata} (src : \url{http://arbres-reunion.cirad.fr}.}
\end{figure}

-->
```
::: {#fig:domcil .subfigures}
![a](Biostats/dombeya_ciliata_11_reference "fig:"){height="200px"} ![b](Biostats/dombeya_ciliata_41_reference "fig:"){height="200px"}

Figure 1: Individual tree of *Dombeya ciliata* (Malvaceae) (a),
Inflorescence of *Dombeya ciliata* (src :
`\url{http://arbres-reunion.cirad.fr}`{=tex} (b)..
:::

## Data import and mining

### Packages

Load datasets required for the analysis

``` {.r}
> # To perfrom grouping in multiple comparison tests
> # install.packages("multcompView")
> library(multcompView)
> # To perform ANOVA2 by permutation (balanced design)
> #install.packages("coin", dependencies = T)
> library(coin)
```

### Data description

In each of the three studied populations, the following measures have
been made on individuals *a priori* identified as males and females:

-   petal length (mm, `lpet`)
-   number of flowers per inflorescence (`flinf`)
-   number of fruits produced / number of flowers per inflorescence
    (`fruit`)

For each individual in the dataset,the reported values are means
calculated per individual using five repeated measures (hence numbers of
flowers per inflorescence are not integers). These variables will be
considered as response variables here.

The dataset also contains two explicative factors:

-   `pop`: the population is which the individuals were sampled,
-   `sex`: the sex of the individual.

### Data import

Using you RStudio project for `Ecodata`, import the data file in
`\Rlogo`{=tex}.

``` {.r}
> # Import
> domcil <- read.table("data/ciliata.txt", head = T, sep = "\t")
> domcil$pop <- as.factor(domcil$pop)
> domcil$sex <- as.factor(domcil$sex)
> 
> # Column names
> names(domcil)
```

    ## [1] "pop"   "sex"   "flinf" "lpet"  "fruit"

-   Data summary (populations and sexes altogether) :

``` {.r}
> summary(domcil)
```

    ##  pop     sex        flinf            lpet       
    ##  MA:36   f:46   Min.   :14.20   Min.   : 3.453  
    ##  ML:28   m:48   1st Qu.:18.90   1st Qu.: 7.218  
    ##  TA:30          Median :20.74   Median : 9.130  
    ##                 Mean   :20.79   Mean   : 8.841  
    ##                 3rd Qu.:22.60   3rd Qu.:10.627  
    ##                 Max.   :29.33   Max.   :12.775  
    ##      fruit       
    ##  Min.   :0.0000  
    ##  1st Qu.:0.0000  
    ##  Median :0.2055  
    ##  Mean   :0.3107  
    ##  3rd Qu.:0.5627  
    ##  Max.   :0.8250

### Graphical exploration

-   Using `ggplot2`:

``` {.r}
> # Boxplots
> ggplot(domcil) + 
+     aes(x = pop, y = lpet, fill = sex) +
+     geom_boxplot() +
+     facet_wrap(~ sex) 
```

```{=html}
<embed src="../figures/TP-box-1.pdf" style="display: block; margin: auto;" type="application/pdf" />
```
``` {.r}
> # Points + statistical summary:
> # mean +/- standard error, see argument 'fun.data'
> p <- position_dodge(width = 0.2)
> ggplot(domcil) +
+     aes(x = pop, y = lpet, colour = sex) +
+     geom_point() + 
+     stat_summary(aes(group = sex), fun.data = "mean_se",
+     col = "darkgray", position = p) # argument 'position' avoids overlay
> 
> # Other version with 'geom = crossbar'
> 
> ggplot(domcil) +
+     aes(x = pop, y = lpet, colour = sex) +
+     geom_point(position = p, alpha = 0.7) + 
+     stat_summary(aes(group = sex), geom = "crossbar",
+       fun.data = "mean_se", size = 1, position = p, col = "darkgray")
```

```{=html}
<embed src="../figures/TP-pts-1.pdf" style="display: block; margin: auto;" type="application/pdf" />
```
```{=html}
<embed src="../figures/TP-pts-2.pdf" style="display: block; margin: auto;" type="application/pdf" />
```
-   Using `plot.design`:

``` {.r}
> plot.design(lpet ~ pop * sex, data = domcil)
```

```{=html}
<embed src="../figures/TP-design-1.pdf" style="display: block; margin: auto;" type="application/pdf" />
```
-   Evidence of an interaction?

``` {.r}
> # Function 'with' allows to define a command whose arguments
> # are interpreted in the scope of the first argument of 'with' (here data frame domcil)
> par(mfcol = c(1, 2))
> with(domcil, interaction.plot(x.factor = pop, trace.factor = sex, response = lpet))
> with(domcil, interaction.plot(x.factor = sex, trace.factor = pop, response = lpet))
```

```{=html}
<embed src="../figures/TP-interac-1.pdf" style="display: block; margin: auto;" type="application/pdf" />
```
```{=html}
<!-- Q01 -->
```
-   *Q1: What is the nature of the response variables studied here?*

```{=html}
<!-- Q02 -->
```
-   *Q2: Are the following arugments true or false ? The sampling design
    is complete in the present case study. The sampling design is
    balanced in the present case study. *

# Data analysis

## Mean values and variance

*Calculate descriptive statistics of position and dispersion in each
combination of the two explicative factors* `pop` *and* `sex`.\_

``` {.r}
> # Calculate mean values and standard deviation of "flinf"
> # for each population and each sex
> m <- tapply(domcil$lpet, list(domcil$sex, domcil$pop), mean)
> st.dev <- tapply(domcil$lpet, list(domcil$sex, domcil$pop), sd)
```

## Two-way ANOVA (ANOVA2)

### Model calibration

The ANOVA with two explicative factors (ANOVA2) is performed with the
function `lm` (to compare with `aov`) that estimates the model
parameters. The `anova` function is then used to perform the complete
decomposition of variance.

The definition of the model is contained in the formula used in the
function:

``` {.r}
> # Full model with interaction
> # 'pop * sex' equivalent to 'pop + sex + pop:sex'
> lpet.lm <- lm(lpet ~ pop * sex, data = domcil, na.action = na.omit)
> lpet.aov  <-  aov(lpet ~ pop * sex, data = domcil, na.action = na.omit)
```

### Model diagnostic

#### Graphical diagnosis

After estimating a linear model (here of type ANOVA2), we need to check
that the hypotheses specified in the model definition (independance,
normality, homosedasticity). For this purpose, we first used diagnosis
graphics provided in `\Rlogo`{=tex}.

``` {.r}
> # Diagnostic graphs
> par(mfcol = c(2, 2))
> plot(lpet.lm)
```

```{=html}
<embed src="../figures/TP-diag1-1.pdf" style="display: block; margin: auto;" type="application/pdf" />
```
After classical plots, it is wise to inspect the relationships between
the resisuals and the explicative variables (two factors here).

``` {.r}
> # Other diagnostic graphs:
> # Residuals vs explicative variables
> # First, add model residuals to the data frame
> domcil$resid <- resid(lpet.lm)
> 
> par(mfcol = c(1, 2))
> boxplot(resid ~ pop, domcil)
> boxplot(resid ~ sex, domcil)
```

```{=html}
<embed src="../figures/TP-diag2-1.pdf" style="display: block; margin: auto;" type="application/pdf" />
```
```{=html}
<!-- Q03 -->
```
-   *Q3: Which tests allow to validate the ANOVA2 on petal length
    analyzed across populations and sexes?*

#### Normality of the residuals?

``` {.r}
> # Shapiro normality test on residuals
> shapiro.test(domcil$resid)
```

    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  domcil$resid
    ## W = 0.99363, p-value = 0.9376

#### Homogeneity of the error variance?

In order to validate (or not) the hypothesis of variance homogeneity
(here across classes), the Bartlett's test can be used. Because the
model includes an interaction term (between `pop` and `sex`), we need to
test all existing combination. For this purpose, we construct a `popsex`
factor combining both factors.

``` {.r}
> # Variance homogeneity test (Bartlett)
> bartlett.test(domcil$resid, interaction(domcil$pop, domcil$sex, drop=TRUE))
```

    ## 
    ##  Bartlett test of homogeneity of variances
    ## 
    ## data:  domcil$resid and interaction(domcil$pop, domcil$sex, drop = TRUE)
    ## Bartlett's K-squared = 3.2617, df = 5, p-value =
    ## 0.6597

``` {.r}
> # or
> domcil$popsex <- with(domcil, paste(pop, sex, sep = "."))
> bartlett.test(resid ~ popsex, data = domcil)
```

    ## 
    ##  Bartlett test of homogeneity of variances
    ## 
    ## data:  resid by popsex
    ## Bartlett's K-squared = 3.2617, df = 5, p-value =
    ## 0.6597

### Model interpretation

#### Decomposition of variance and summary

``` {.r}
> # Variance decomposition de la variance dans le cas du modèle avec interaction
> anova(lpet.lm)
```

    ## Analysis of Variance Table
    ## 
    ## Response: lpet
    ##           Df  Sum Sq Mean Sq F value    Pr(>F)    
    ## pop        2 237.574 118.787 110.024 < 2.2e-16 ***
    ## sex        1 129.497 129.497 119.944 < 2.2e-16 ***
    ## pop:sex    2  30.096  15.048  13.938 5.516e-06 ***
    ## Residuals 88  95.009   1.080                      
    ## ---
    ## Signif. codes:  
    ## 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

The output of the `summary` function differs between the two `lm`
(object 'lpet.lm') and `aov` classes (lpet.aov)

``` {.r}
> # Model interpretation
> # See the differences between summaries for objects of classes 'lm' and 'aov'
> summary(lpet.lm)
```

    ## 
    ## Call:
    ## lm(formula = lpet ~ pop * sex, data = domcil, na.action = na.omit)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -2.4581 -0.7289  0.0738  0.7763  2.6677 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)   5.1704     0.2520  20.517  < 2e-16 ***
    ## popML         5.2758     0.3750  14.069  < 2e-16 ***
    ## popTA         2.7822     0.3681   7.559 3.66e-11 ***
    ## sexm          3.4119     0.3469   9.836 7.82e-16 ***
    ## popML:sexm   -2.7263     0.5240  -5.203 1.27e-06 ***
    ## popTA:sexm   -0.7824     0.5141  -1.522    0.132    
    ## ---
    ## Signif. codes:  
    ## 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 1.039 on 88 degrees of freedom
    ## Multiple R-squared:  0.807,  Adjusted R-squared:  0.796 
    ## F-statistic: 73.57 on 5 and 88 DF,  p-value: < 2.2e-16

``` {.r}
> summary(lpet.aov)
```

    ##             Df Sum Sq Mean Sq F value   Pr(>F)    
    ## pop          2 237.57  118.79  110.02  < 2e-16 ***
    ## sex          1 129.50  129.50  119.94  < 2e-16 ***
    ## pop:sex      2  30.10   15.05   13.94 5.52e-06 ***
    ## Residuals   88  95.01    1.08                     
    ## ---
    ## Signif. codes:  
    ## 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

The summary for the `aov` classes is in fact the variance decomposition
table.

```{=html}
<!-- Q04 -->
```
-   *Q4: What is the proportion of the variance in petal length is
    explained by the two factor 'pop' and 'sex'?*

```{=html}
<!-- Q05 -->
```
-   *Q5: Rank the three tested effects in decreasing order of
    contribution to explained variance in the ANOVA2 for petal length.*

```{=html}
<!-- Q06 -->
```
-   *Q6: What is the number of degrees of freedom associated with the
    residuals in this model?*

```{=html}
<!-- Q07 -->
```
-   *Q7: According to the results of the analysis of petal length, in
    which populations are male and females trees different for this
    trait?*

#### Multiple comparison tests

When effets are significant, the next step of the ANOVA(2) is to perform
multiple comparison tests to compare classes and detect pairwise
significant differences.

``` {.r}
> lpet.mpc <- pairwise.t.test(domcil$lpet,
+     interaction(domcil$pop, domcil$sex, drop = TRUE), 
+   p.adjust.method = "BH") 
> lpet.mpc
```

    ## 
    ##  Pairwise comparisons using t tests with pooled SD 
    ## 
    ## data:  domcil$lpet and interaction(domcil$pop, domcil$sex, drop = TRUE) 
    ## 
    ##      MA.f    ML.f    TA.f    MA.m    ML.m 
    ## ML.f < 2e-16 -       -       -       -    
    ## TA.f 9.1e-11 9.4e-09 -       -       -    
    ## MA.m 2.9e-15 2.7e-06 0.097   -       -    
    ## ML.m < 2e-16 0.097   4.6e-12 1.2e-09 -    
    ## TA.m < 2e-16 0.726   1.2e-09 4.1e-07 0.169
    ## 
    ## P value adjustment method: BH

It is possible to define groups that show similar responses (not
significantly different). All classes sharing a common letter can be
grouped together. The matrix of *p-values* for all pairs of classes is
used to produce a vector, as required in the function `multcompLetters`.

``` {.r}
> # Vector of p-values
> pvals  <-  as.vector(lpet.mpc$p.v)
> # Names of factor classes combinations
> r.nam <- rownames(lpet.mpc$p.v)
> c.nam <- colnames(lpet.mpc$p.v)
> # Numbers of columns and rows
> c.nbr <- ncol(lpet.mpc$p.v)
> r.nbr <- nrow(lpet.mpc$p.v)
> # Define vector names
> names(pvals)  <-  paste(rep(r.nam, times = c.nbr),
+   rep(c.nam, each = r.nbr), sep = "-")
> pvals  <-  pvals[! is.na(pvals)]
> names(pvals)
```

    ##  [1] "ML.f-MA.f" "TA.f-MA.f" "MA.m-MA.f" "ML.m-MA.f"
    ##  [5] "TA.m-MA.f" "TA.f-ML.f" "MA.m-ML.f" "ML.m-ML.f"
    ##  [9] "TA.m-ML.f" "MA.m-TA.f" "ML.m-TA.f" "TA.m-TA.f"
    ## [13] "ML.m-MA.m" "TA.m-MA.m" "TA.m-ML.m"

``` {.r}
> multcompLetters(pvals, compare = "<", threshold = 0.05, Letters = letters,
+     reversed = FALSE)
```

    ## ML.f TA.f MA.m ML.m TA.m MA.f 
    ##  "a"  "b"  "b"  "a"  "a"  "c"

The ANOVA2 is completed once multiple comparisons have been performed.

## Applications to other response variables

*Perform similar analyses for the number of flowers per infloresence and
the number of fruits produced per flower per infloresence.*

```{=html}
<!-- Q08 -->
```
-   *Q8: Is the ANOVA2 model valid for the number of flowers per
    infloresence?*

```{=html}
<!-- Q09 -->
```
-   *Q9: Which proportion of the variance in the number of flowers per
    inflorescence is explained by the model?*

``` {.r}
> flinf.lm <- lm(flinf ~ pop * sex, data = domcil, na.action = na.omit)
> 
> # Residuals in column
> domcil$resid2 <- resid(flinf.lm)
> 
> flinf.aov  <-  aov(flinf ~ pop + sex + pop : sex, data = domcil)
> 
> # Diagnostic
> par(mfcol = c(2, 2))
> plot(flinf.aov)
> 
> # Shapiro's test
> shapiro.test(domcil$resid2)
```

    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  domcil$resid2
    ## W = 0.98815, p-value = 0.5643

``` {.r}
> # Bartlett's test
> plot(resid2 ~ pop * sex, data = domcil)
> bartlett.test(domcil$resid2, interaction(domcil$pop, domcil$sex, drop = TRUE))
```

    ## 
    ##  Bartlett test of homogeneity of variances
    ## 
    ## data:  domcil$resid2 and interaction(domcil$pop, domcil$sex, drop = TRUE)
    ## Bartlett's K-squared = 5.8149, df = 5, p-value =
    ## 0.3246

``` {.r}
> # Interpretation
> anova(flinf.lm)
```

    ## Analysis of Variance Table
    ## 
    ## Response: flinf
    ##           Df Sum Sq Mean Sq F value Pr(>F)
    ## pop        2  11.30  5.6489  0.7042 0.4973
    ## sex        1   0.81  0.8068  0.1006 0.7519
    ## pop:sex    2   5.78  2.8923  0.3605 0.6983
    ## Residuals 88 705.93  8.0220

``` {.r}
> summary(flinf.lm)
```

    ## 
    ## Call:
    ## lm(formula = flinf ~ pop * sex, data = domcil, na.action = na.omit)
    ## 
    ## Residuals:
    ##    Min     1Q Median     3Q    Max 
    ## -6.209 -2.130  0.185  1.883  7.571 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  20.5818     0.6869  29.962   <2e-16 ***
    ## popML         1.1775     1.0222   1.152    0.252    
    ## popTA        -0.1731     1.0033  -0.173    0.863    
    ## sexm          0.2177     0.9456   0.230    0.818    
    ## popML:sexm   -1.1556     1.4283  -0.809    0.421    
    ## popTA:sexm   -0.1830     1.4013  -0.131    0.896    
    ## ---
    ## Signif. codes:  
    ## 0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 2.832 on 88 degrees of freedom
    ## Multiple R-squared:  0.02471,    Adjusted R-squared:  -0.0307 
    ## F-statistic: 0.446 on 5 and 88 DF,  p-value: 0.8151

```{=html}
<embed src="../figures/TP-modflinf-1.pdf" style="display: block; margin: auto;" type="application/pdf" />
```
```{=html}
<embed src="../figures/TP-modflinf-2.pdf" style="display: block; margin: auto;" type="application/pdf" />
```
```{=html}
<!-- Q10 -->
```
-   *Q10: What is (are) the statistical issue(s) regarding the model for
    the 'fruit' variable?*

```{=html}
<!-- Q11 -->
```
-   *Q11: What is the minimum observed values for this variable?*

``` {.r}
> fruit.lm <- lm(fruit ~ pop * sex, data = domcil, na.action = na.omit)
> domcil$resid3 <- resid(fruit.lm)
> par(mfcol = c(2, 2))
> plot(fruit.lm)
> shapiro.test(domcil$resid3)
```

    ## 
    ##  Shapiro-Wilk normality test
    ## 
    ## data:  domcil$resid3
    ## W = 0.9629, p-value = 0.009108

``` {.r}
> bartlett.test(domcil$resid3, interaction(domcil$pop, domcil$sex, drop = TRUE))
```

    ## 
    ##  Bartlett test of homogeneity of variances
    ## 
    ## data:  domcil$resid3 and interaction(domcil$pop, domcil$sex, drop = TRUE)
    ## Bartlett's K-squared = Inf, df = 5, p-value < 2.2e-16

```{=html}
<embed src="../figures/TP-modfruit-1.pdf" style="display: block; margin: auto;" type="application/pdf" />
```
``` {.r}
> # Calculate mean values and standard deviation of "fruit"
> # for each population and each sex
> m <- tapply(domcil$fruit, list(domcil$sex, domcil$pop), mean)
> m
```

    ##          MA        ML        TA
    ## f 0.7134118 0.4553571 0.5386667
    ## m 0.0000000 0.1021429 0.0798000

``` {.r}
> st.dev <- tapply(domcil$fruit, list(domcil$sex, domcil$pop), sd)
> st.dev
```

    ##          MA         ML         TA
    ## f 0.0810579 0.10038741 0.16609750
    ## m 0.0000000 0.07196138 0.06735535

```{=html}
<!-- Q12 -->
```
-   *Q12: What is the value of the variance for the variable for males
    in Mare Longue?*

# Non-parametric ANOVA

When the assumptions of the linear model are not fulfilled, one can opt
for a non-parametric ANOVA. This analysis is based on observation ranks
and does not require any assumption on the data. Here we apply the
procedure for the third response variable `fruit`.

## Non-parametric one- and two-way ANOVA

``` {.r}
> # Calculate mean values and standard deviation of "fruit"
> # for each population and each sex
> m <- tapply(domcil$fruit, list(domcil$sex, domcil$pop), mean)
> m
```

    ##          MA        ML        TA
    ## f 0.7134118 0.4553571 0.5386667
    ## m 0.0000000 0.1021429 0.0798000

``` {.r}
> st.dev <- tapply(domcil$fruit, list(domcil$sex, domcil$pop), sd)
> st.dev
```

    ##          MA         ML         TA
    ## f 0.0810579 0.10038741 0.16609750
    ## m 0.0000000 0.07196138 0.06735535

The usual function for the non-parametric ANOVA (or Kruskal-Wallis test)
only accepts one explicative factor. First, we conduct ANOVA1 for each
of the tested factors.

``` {.r}
> fr.pop.kw <- kruskal.test(fruit ~ pop, data = domcil, na.action = na.omit)
> fr.pop.kw
```

    ## 
    ##  Kruskal-Wallis rank sum test
    ## 
    ## data:  fruit by pop
    ## Kruskal-Wallis chi-squared = 0.20063, df = 2, p-value
    ## = 0.9046

``` {.r}
> fr.sex.kw <- kruskal.test(fruit ~ sex, data = domcil, na.action = na.omit)
> fr.sex.kw
```

    ## 
    ##  Kruskal-Wallis rank sum test
    ## 
    ## data:  fruit by sex
    ## Kruskal-Wallis chi-squared = 71.418, df = 1, p-value
    ## < 2.2e-16

It is possible to conduct an 'indirect' ANOVA2 by using a facotr
defining all possible combinations (`popsex`).

``` {.r}
> domcil$popsex <- with(domcil, paste(pop, sex, sep = "."))
> fr.ps.kw <- kruskal.test(fruit ~ popsex , data = domcil, na.action = na.omit)
> fr.ps.kw
```

    ## 
    ##  Kruskal-Wallis rank sum test
    ## 
    ## data:  fruit by popsex
    ## Kruskal-Wallis chi-squared = 81.749, df = 5, p-value
    ## = 3.612e-16

## Non-parametric multi-comparison tests

As in the parametric case, a significant result needs to to be completed
by multiple comparison tests.

Risk of type-I errors are adjusted, because of the multiple comparisons,
using the Benjamini-Hochberg method (avoid Bonferoni).

```{=html}
<!-- Q13 -->
```
-   *Q13: In which population, females are significantly different from
    females of the two other populations?*

-   Comparison of populations (both sexes considered):

``` {.r}
> pairwise.wilcox.test(domcil$fruit, domcil$pop, p.adjust.method = "BH")
```

    ## Warning in wilcox.test.default(xi, xj, paired =
    ## paired, ...): cannot compute exact p-value with ties

    ## Warning in wilcox.test.default(xi, xj, paired =
    ## paired, ...): cannot compute exact p-value with ties

    ## Warning in wilcox.test.default(xi, xj, paired =
    ## paired, ...): cannot compute exact p-value with ties

    ## 
    ##  Pairwise comparisons using Wilcoxon rank sum test with continuity correction 
    ## 
    ## data:  domcil$fruit and domcil$pop 
    ## 
    ##    MA   ML  
    ## ML 0.95 -   
    ## TA 0.95 0.95
    ## 
    ## P value adjustment method: BH

-   Comparison of sexes (all populations considered):

``` {.r}
> pairwise.wilcox.test(domcil$fruit, domcil$sex, p.adjust.method = "BH")
```

    ## Warning in wilcox.test.default(xi, xj, paired =
    ## paired, ...): cannot compute exact p-value with ties

    ## 
    ##  Pairwise comparisons using Wilcoxon rank sum test with continuity correction 
    ## 
    ## data:  domcil$fruit and domcil$sex 
    ## 
    ##   f     
    ## m <2e-16
    ## 
    ## P value adjustment method: BH

-   Comparison of factor combinations (`pop * sex`):

``` {.r}
> fr.nmpc <- pairwise.wilcox.test(domcil$fruit, domcil$popsex, p.adjust.method = "BH") 
```

    ## Warning in wilcox.test.default(xi, xj, paired =
    ## paired, ...): cannot compute exact p-value with ties

    ## Warning in wilcox.test.default(xi, xj, paired =
    ## paired, ...): cannot compute exact p-value with ties

    ## Warning in wilcox.test.default(xi, xj, paired =
    ## paired, ...): cannot compute exact p-value with ties

    ## Warning in wilcox.test.default(xi, xj, paired =
    ## paired, ...): cannot compute exact p-value with ties

    ## Warning in wilcox.test.default(xi, xj, paired =
    ## paired, ...): cannot compute exact p-value with ties

    ## Warning in wilcox.test.default(xi, xj, paired =
    ## paired, ...): cannot compute exact p-value with ties

    ## Warning in wilcox.test.default(xi, xj, paired =
    ## paired, ...): cannot compute exact p-value with ties

    ## Warning in wilcox.test.default(xi, xj, paired =
    ## paired, ...): cannot compute exact p-value with ties

    ## Warning in wilcox.test.default(xi, xj, paired =
    ## paired, ...): cannot compute exact p-value with ties

    ## Warning in wilcox.test.default(xi, xj, paired =
    ## paired, ...): cannot compute exact p-value with ties

    ## Warning in wilcox.test.default(xi, xj, paired =
    ## paired, ...): cannot compute exact p-value with ties

    ## Warning in wilcox.test.default(xi, xj, paired =
    ## paired, ...): cannot compute exact p-value with ties

``` {.r}
> # Vector of p-values
> pvals  <-  as.vector(fr.nmpc$p.v)
> # Names of factor classes combinations
> names(pvals)  <-  paste(rep(rownames(fr.nmpc$p.v), times = ncol(fr.nmpc$p.v)),
+   rep(colnames(fr.nmpc$p.v), each = nrow(fr.nmpc$p.v)), sep = "-")
> pvals  <-  pvals[! is.na(pvals)]
> multcompLetters(pvals, compare = "<", threshold = 0.05, Letters = letters, reversed = FALSE)
```

    ## MA.m ML.f ML.m TA.f TA.m MA.f 
    ##  "a"  "b"  "c"  "b"  "c"  "d"

# ANOVA by permutation

An alternative to non-parametric ANOVA is the ANOVA by permuation.

As in non-parametric ANOVA, ANOVA by permutation does not require any
particular condition of application. This analysis is however more
robust than the non-parametric `kruskal-vallis` test. Similar to a
bootstraping approach, the ANOVA by permutation simulates random
replicates of the original dataset. This set of replicates represent
cases where the original relationships present in the observed dataset
are "broken", they thus represent multiple cases issued from the null
model. ANOVA statistics can be calculated from these simulated
replicates and the observed statistics are then compared to the
distribution of simulated values to detect possible significant
deviations between the observed case and the null model.

The following analysis is an example of an ANOVA1 for the `fruit`
variable.

``` {.r}
> fr.perm.pop <- independence_test(fruit ~ pop, data = domcil)
> fr.perm.pop
```

    ## 
    ##  Asymptotic General Independence Test
    ## 
    ## data:  fruit by pop (MA, ML, TA)
    ## maxT = 0.69652, p-value = 0.7654
    ## alternative hypothesis: two.sided

Multiple comparisons in the ANOVA by permutation require a particular
function f the `rcompanion` package..

``` {.r}
> PM  <-  pairwisePermutationMatrix(x = domcil$fruit, g = domcil$pop, method = "BH")
> PM 
```

    ## $Unadjusted
    ##    MA     ML     TA
    ## MA NA 0.4469 0.7278
    ## ML NA     NA 0.6197
    ## TA NA     NA     NA
    ## 
    ## $Method
    ## [1] "BH"
    ## 
    ## $Adjusted
    ##        MA     ML     TA
    ## MA 1.0000 0.7278 0.7278
    ## ML 0.7278 1.0000 0.7278
    ## TA 0.7278 0.7278 1.0000

``` {.r}
> multcompLetters(PM$Adjusted, compare = "<", threshold = 0.05, Letters = letters,
+     reversed = FALSE)
```

    ## $Letters
    ##  MA  ML  TA 
    ## "a" "a" "a" 
    ## 
    ## $LetterMatrix
    ##       a
    ## MA TRUE
    ## ML TRUE
    ## TA TRUE

# Synthesis

```{=html}
<!-- Q14 -->
```
-   *Q14: What is the reproductive system of the studied species *Dombya
    ciliata*?*

```{=html}
<!-- Q15 -->
```
-   *Q15: Is there evidence of sexual dimorphism?*

```{=html}
<!-- Q16 -->
```
-   *Q16: Is there evidence that reproduction system and sexual
    dimorphism of sexual characters vary according to populations?*
