% Ecological Data Analysis - Final assignement
%    University of La Réunion - M1 BEST-T

---
date: 2020 -- 2021
classoption: addpoints
printanswers: true
gradetable: false
fontsize: 11pt
numbered: false
lang: english
toc: false
logo: true
graphicspath:
   - /home/olive/Boulot/images/
   - /home/olive/Boulot/uni/figures/Biostats/
bibliography: /home/olive/Boulot/uni/master.bib
---
-------------------------------------------

Duration : 2 h.

All answers must be justified. 















# Case study

In the pumpkinseed fish (_Lepomis gibbosus_^[Perche arc-en-ciel]), some individuals display a colourful red spot on their operculum (gill cover, [@Fig:lepo]). This phenotype has classically been associated with behavioural dominance. In a recent study^[[@zieba_red_2018]], scientists investigated the function of the red spot in sampled pumpkinseed populations across Europe where the species is invasive.

Several alternative mating strategies were observed in pumpkinseed males. Some males are rather large-bodied and territorial. They build nests, court females and care for the eggs that are laid in their nest. Other males perform a ‘sneaky’^[to sneak in: entrer furtivement] mating strategy: they enter the nest of a territorial male during spawning to fertilise eggs laid by a female courted by the nest-guarding male. The territorial male then cares for eggs and young stages, whereas sneaker males perform no parental care.

The aim of the study was to determine whether the presence of the red operculum spot functions as a signal of sex and/or mating strategy in pumpkinseed. To do this males were categorised as territorials or sneakers and the authors analyzed the probability of presence of a red spot in relation with individual sexes and the different reproductive strategies. 

The collected data include individual fish sex (variable `sex`: `M / F`), mating strategy (`'tactic'`: female `'fem'`, territorial male `'terr'`, sneaker male  `'sneak'`), fish length (`'sl'`, in mm), fish weight (`'wt'`, in g), population (`'pop'`) and presence of a red spot (`'spot'`). Sex and mating strategy was assigned by dissection of the gonads as sneaker males tend to have larger testis size.


<div id="fig:lepo">
![](Lepomis_gibbosus_pinterest){#fig:lepo1 width=40%}\ ![](Lepomis_gibbosus_animal-sandiegozoo-org){#fig:lepo2 width=35%}

Male individuals of pumpkinseed fish (_Lepomis gibbosus_) with (a) or without (b) a red spot on the operculum 
</div>


\newpage

# Analysis and questions

[Q]

[Q=1]

If the red spot on operculum is an actual sexual feature, which relationship can be expected between its presence and the sex, and / or male mating strategy?

[S]

If the red spot is a true and reliable signal for sexual selection, following general principles governing sexual features in animals, it could be expected that males only display such feature, not females. As sneaky males do not provide parental care, that can have high costs, one could also expect that sneaky males have larger or redder spots displayed.

[/S]


[Q=]

[P]

[P=2]

Describe the nature of variables and explain which variable(s) can be considered as the response and explicative variables.


[S]

In this case study, we are interested in undertanding how the presence of the red spot varies in response to individual sex and mating strategy.
Hence, the presence of the red spot, a binary variable can be considered as the response. Sex and mating strategy are two qualitative variables and individual size (`sl`) is quantitative and continuous.

[/S]

[P=1\\half]

What type of analysis can be performed to address the objective of the study?

[S]

The design of the study allows us to identify one response variable and several potentially explicative variables. The general methodological framework that can be used in such case is statistical modelling, more precisely multiple regression (one response / several explicative variables). Moreover, given the nature of the response variable (binary variable, observed at individual level), the type of model that can be estimated is the Generalized Linear Model. 

[/S]


[/P]

\bigskip

To answer the objectives of the study, the authors fit a statistical model defined by the following formula: `'spot ~ tactic * sl'` (or equivalently `'spot ~ tactic + sl + tactic : sl'`).


[Q=]

[P]

[P=1]

Considering data exploration ([@Fig:pairs]), explain why the variables `'wt'` and `'sex'` are not included in the model. 


[S]

The graph for data exploration shows both the univariate distribution of the variable considered (here only explicative ariables are considered, not the reponse) in the diagonal panels, and bivariate relationships for all pairs of variables above and below the diagonal. The nature of the graphs depends on the type of variables represented (boxplots for continuous / qualitative types), correspond to series of histograms with the types qualitative / continuous.

These graphs show that the two continuous variables related to size (length, `sl`, and weight, `wt`) are strongly correlated. This is also the case for the `sex` and `tactic` qualitative variables, as in fact one class ('F') is present in both variables. These correlations among explicative variables, also called (multi)colinearity, induce _redundancy_ among variables, which makes the effects of these variables not separable from a statistical point of view: because these variables are (strongly) correlated, they convey some common (redundant) information about statistical units. Hence their effects cannot be separated, otherly said, they cannot be correctly estimated when the variables are included in the same model.

[/S]


[P=\\half]

Why did the authors include body size (`'sl'`) in the model?


[S]

It was not in the primary objective of the study to study the relationship between individual size and the red spot presence. However, because larger fishes tend to older (hence more mature than smaller individuals), the analysis needed to control for body size while simultaneously comparing the probability of red spots presence among individuals of different sexes/mating strategies.

[/S]

[/P]


[Q=2]

Based on diagnostic graphics for the model ([@Fig:diag]), explain whether the model presents statistical issues or not.

[S]

The diagnostic graphics are not all informatiive here. The distribution of residuals is not useful because of the nature of the response variable that can take two different values only (0 / 1). The graphic for Cook's distance shows no particularly influential observation. The most striking feature of the model diagnostic is the presence of two observations for which the probability of presence of a red spot is poorly estimated ('58' and '645'). One could remove these two observations and fit the model again. The sample size being large here ($n=900$), removing two observations should not change the results and the interpretation of the model.

[/S]

[Q=]

Table [-@tbl:anova] presents the decomposition of deviance for the calibrated model. 


[P]

[P=1]

What is the proportion of deviance explained by the model?

[S]

The proportion of deviance (equivalent to the coefficient of determination, $R^2$) explained by the full model can be calculated from table [@tbl:anova]: $R^2= 1 - 839 / 1199$ = 0.3003.

[/S]


[P=1]

Among the tested effects, which variable explains the largest part of deviance? Justify by calculating this proportion.



[S]

This question is similar to the previous one about the explicative power of the model. Here the largest proportion of explained deviance is obtained with the `sl` variable, it is equal to : 177,9 / 1999, that is 0.089.

The two other effects have associated smaller deviance.

[/S]


[P=2]

Interpret the results provided by the analysis of deviance (@Tbl:anova). In particular, what does the result regarding the effect designated as `'tactic:sl'` indicate?

[S]


The analysis of deviance shows that the three components of the calibrated statistical model have significant effects: the model contains the two principal effects for the two variables `tactic` (qualitative) and `sl` (quantitative), and an interaction term between these variables. Here the interaction is between a quantitative and a qualitative: it allows to model an effect of individual size (`sl`) that depends on the other variable (mating strategy). The effect of size being modelled as a linear relationship, this means that the slope of this relationship vary from one class of the factor 'mating strategy' to another. 

[/S]


[/P]




[Q=\\half]

What is the total number of observations in the model?


[S]

The total number of observations used in the model can be deduced from the number of degrees of freedom. For instance, the null model has 899 associated degrees of freedom which is $n-1$, where $n$ is the total sample size. Hence the total number of observations or sample size is 900.

[/S]


[Q=]

Table [-@tbl:summ] presents statistical outputs summarizing the calibrated model and effects.

[P]

[P=1\\half]

Indicate the numerical values that are replaced by the letters 'A', 'B', and 'C' in table [-@tbl:anova].


[S]

The correct numerical values are: 

$A = 2, B = 896, B = 1031.7$

* $A$ is the number of degrees of freedom associated with the explicative variable (factor) `tactic` that has 3 classes, and hence only 2 degrees of freedom associated (two independent classes).
* $B$ is the number of residuals degrees of freedom (not represented in explicative variables) of the model of the corresponding line, here 896 (one d.f. added compared to the model on the line aobve).
* $C$ is residual deviance of the model represented on the line, here it is the simple model with one explicative variable only (`tactic`). The null (maximal) deviance is indicated on the first line, the value of C is thus $C = 1199 - 167.3 = 1031.7$.

[/S]





[P=1]

What does the coefficient named `'tactisneak'` represent in the table?

[S]

The coefficient named `tacticsneak` represents the effect of the 'sneak' class on the probability of presence of a red spot: 

[/S]


[P=1]

Same question for the `'sl'` line ? and for `'tacticsneak:sl'` line?

[P=1]

Why is there no coefficient indicated for the `'fem'` class of the `'tactic'` variable (i.e. no line named `'tacticfem'`) in this table?

[P=1]

Which classes of the `tactic` factor have the highest and lowest effect on the probability of presence of a red spot? (coefficients were calculated 'by treatment').

[/P]


[Q=1]

Which other explicative variable, not included in the model but present in the dataset, could have an effect on the probability of a red dot presence? Explain why.

[Q=1]

Interpret the differences in the estimated relationships as shown on [@Fig:fits] in relation with the statistical results.

[Q=2]

Synthesis: according to this analysis and these results, which conclusion(s) can be drawn on the role of the red spot for reproduction in the pumpkinseed fish?


[/Q]










\vspace{2cm}escaped_line_breaks



Table: Deviance decomposition table (output of `anova` function). {#tbl:anova}


&nbsp;      | Df |  Deviance |  Resid.Df  |  Resid. Dev |  Pr(>Chi)
------------|:--:|:---------:|:----------:|:-----------:|:----------
   **Null** |899 |         1199  |   &nbsp;  | &nbsp;            
  **tactic**  |   A |    167.3   |     897 |     C   |     4.73e-37  
    **sl**  |   1  |   177.9   |     B    |      853.9   |   1.374e-40 
 **tactic:sl** |  2  |   14.91   |     894  |       839    |   0.000578  



Table: Statistical information as provided by the `summary` function. {#tbl:summ} 


&nbsp;        | Estimate  | Std. Error  | $z$ value  | Pr(>$\mid z\mid$)
--------------|:---------:|:-----------:|:--------:|------------
**(Intercept)** | -5.007 | 0.5312 | -9.425 | 4.292e-21
**tacticsneak** | 2.896 | 1.963 | -1.475 | 0.1401 | 
**tacticterr** | -0.3064 | 0.8656 | -0.354 | 0.7234 | 
**sl** | 0.04262 | 0.005955 | 7.156 | 8.31e-13
**tacticsneak:sl** | 0.0819 | 0.03105 | 2.637 | 0.008354
**tacticterr:sl** | 0.03057 | 0.01069 | 2.859 | 0.004247


* Null deviance: 1199  on 899  degrees of freedom          
* Residual deviance: 839  on 894  degrees of freedom          




<!--


------------------------------------------------------------------
       &nbsp;         Estimate   Std. Error   z value   Pr(>|z|)  
-------------------- ---------- ------------ --------- -----------
  **(Intercept)**      -5.007      0.5312     -9.425    4.292e-21 

  **tacticsneak**      -2.896      1.963      -1.475     0.1401   

   **tacticterr**     -0.3064      0.8656     -0.354     0.7234   

       **sl**         0.04262     0.005955     7.156    8.31e-13  

 **tacticsneak:sl**    0.0819     0.03105      2.637    0.008354  

 **tacticterr:sl**    0.03057     0.01069      2.859    0.004247  
------------------------------------------------------------------


(Dispersion parameter for  binomial  family taken to be  1 )


-------------------- --------------------------
   Null deviance:     1199  on 899  degrees of 
                              freedom          

 Residual deviance:   839  on 894  degrees of  
                              freedom          
-------------------- --------------------------

-->







\newpage



<div class="figure" style="text-align: center">
<img src="../../../docs/figures/exam-pairs-1.png" alt="Exploratory figure showing the bivariate relationships between variables (except 'spot') in graphs above and below the diagonal, and variable distributions in the diagonal." width="300px" />
<p class="caption">Exploratory figure showing the bivariate relationships between variables (except 'spot') in graphs above and below the diagonal, and variable distributions in the diagonal.</p>
</div>


<div class="figure" style="text-align: center">
<img src="../../../docs/figures/exam-diag-1.png" alt="Diagnostic plots for the full model (with interaction)." width="300px" />
<p class="caption">Diagnostic plots for the full model (with interaction).</p>
</div>



<div class="figure" style="text-align: center">
<img src="../../../docs/figures/exam-fits-1.png" alt="Observed data and estimated probability of the presence of a red spot as function of fish length (\texttt{sl}) for the three classes of mating strategy." width="300px" />
<p class="caption">Observed data and estimated probability of the presence of a red spot as function of fish length (\texttt{sl}) for the three classes of mating strategy.</p>
</div>

