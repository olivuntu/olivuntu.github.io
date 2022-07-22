% Ecological Data Analysis - Final assignement % University of La
Réunion - M1 BEST-T

------------------------------------------------------------------------

Duration : 2 h.

All answers must be justified.

# Case study

In the pumpkinseed fish (*Lepomis gibbosus*[1]), some individuals
display a colourful red spot on their operculum (gill cover, Fig. 1).
This phenotype has classically been associated with behavioural
dominance. In a recent study[2], scientists investigated the function of
the red spot in sampled pumpkinseed populations across Europe where the
species is invasive.

Several alternative mating strategies were observed in pumpkinseed
males. Some males are rather large-bodied and territorial. They build
nests, court females and care for the eggs that are laid in their nest.
Other males perform a ‘sneaky’[3] mating strategy: they enter the nest
of a territorial male during spawning to fertilise eggs laid by a female
courted by the nest-guarding male. The territorial male then cares for
eggs and young stages, whereas sneaker males perform no parental care.

The aim of the study was to determine whether the presence of the red
operculum spot functions as a signal of sex and/or mating strategy in
pumpkinseed. To do this males were categorised as territorials or
sneakers and the authors analyzed the probability of presence of a red
spot in relation with individual sexes and the different reproductive
strategies.

The collected data include individual fish sex (variable `sex`:
`M / F`), mating strategy (`'tactic'`: female `'fem'`, territorial male
`'terr'`, sneaker male `'sneak'`), fish length (`'sl'`, in mm), fish
weight (`'wt'`, in g), population (`'pop'`) and presence of a red spot
(`'spot'`). Sex and mating strategy was assigned by dissection of the
gonads as sneaker males tend to have larger testis size.

<div id="fig:lepo" class="subfigures">

<img src="assets/images/Lepomis_gibbosus_pinterest.jpg" title="fig:" id="fig:lepo1" style="width:40.0%" alt="a" /> <img src="assets/images/Lepomis_gibbosus_animal-sandiegozoo-org.jpg" title="fig:" id="fig:lepo2" style="width:35.0%" alt="b" />

Figure 1: Male individuals of pumpkinseed fish (*Lepomis gibbosus*) with
(a) or without (b) a red spot on the operculum.

</div>

# Analysis and questions

\[Q\]

\[Q=1\]

If the red spot on operculum is an actual sexual feature, which
relationship can be expected between its presence and the sex, and / or
male mating strategy?

\[S\]

If the red spot is a true and reliable signal for sexual selection,
following general principles governing sexual features in animals, it
could be expected that males only display such feature, not females. As
sneaky males do not provide parental care, that can have high costs, one
could also expect that sneaky males have larger or redder spots
displayed.

\[/S\]

\[Q=\]

\[P\]

\[P=2\]

Describe the nature of variables and explain which variable(s) can be
considered as the response and explicative variables.

\[S\]

In this case study, we are interested in undertanding how the presence
of the red spot varies in response to individual sex and mating
strategy. Hence, the presence of the red spot, a binary variable can be
considered as the response. Sex and mating strategy are two qualitative
variables and individual size (`sl`) is quantitative and continuous.

\[/S\]

\[P=1\\half\]

What type of analysis can be performed to address the objective of the
study?

\[S\]

The design of the study allows us to identify one response variable and
several potentially explicative variables. The general methodological
framework that can be used in such case is statistical modelling, more
precisely multiple regression (one response / several explicative
variables). Moreover, given the nature of the response variable (binary
variable, observed at individual level), the type of model that can be
estimated is the Generalized Linear Model.

\[/S\]

\[/P\]

To answer the objectives of the study, the authors fit a statistical
model defined by the following formula: `'spot ~ tactic * sl'` (or
equivalently `'spot ~ tactic + sl + tactic : sl'`).

\[Q=\]

\[P\]

\[P=1\]

Considering data exploration (Fig. **¿fig:pairs?**), explain why the
variables `'wt'` and `'sex'` are not included in the model.

\[S\]

The graph for data exploration shows both the univariate distribution of
the variable considered (here only explicative ariables are considered,
not the reponse) in the diagonal panels, and bivariate relationships for
all pairs of variables above and below the diagonal. The nature of the
graphs depends on the type of variables represented (boxplots for
continuous / qualitative types), correspond to series of histograms with
the types qualitative / continuous.

These graphs show that the two continuous variables related to size
(length, `sl`, and weight, `wt`) are strongly correlated. This is also
the case for the `sex` and `tactic` qualitative variables, as in fact
one class (‘F’) is present in both variables. These correlations among
explicative variables, also called (multi)colinearity, induce
*redundancy* among variables, which makes the effects of these variables
not separable from a statistical point of view: because these variables
are (strongly) correlated, they convey some common (redundant)
information about statistical units. Hence their effects cannot be
separated, otherly said, they cannot be correctly estimated when the
variables are included in the same model.

\[/S\]

\[P=\\half\]

Why did the authors include body size (`'sl'`) in the model?

\[S\]

It was not in the primary objective of the study to study the
relationship between individual size and the red spot presence. However,
because larger fishes tend to older (hence more mature than smaller
individuals), the analysis needed to control for body size while
simultaneously comparing the probability of red spots presence among
individuals of different sexes/mating strategies.

\[/S\]

\[/P\]

\[Q=2\]

Based on diagnostic graphics for the model (Fig. **¿fig:diag?**),
explain whether the model presents statistical issues or not.

\[S\]

The diagnostic graphics are not all informatiive here. The distribution
of residuals is not useful because of the nature of the response
variable that can take two different values only (0 / 1). The graphic
for Cook’s distance shows no particularly influential observation. The
most striking feature of the model diagnostic is the presence of two
observations for which the probability of presence of a red spot is
poorly estimated (‘58’ and ‘645’). One could remove these two
observations and fit the model again. The sample size being large here
(*n* = 900), removing two observations should not change the results and
the interpretation of the model.

\[/S\]

\[Q=\]

Table **¿tbl:anova?** presents the decomposition of deviance for the
calibrated model.

\[P\]

\[P=1\]

What is the proportion of deviance explained by the model?

\[S\]

The proportion of deviance (equivalent to the coefficient of
determination, *R*<sup>2</sup>) explained by the full model can be
calculated from table Tbl. **¿tbl:anova?**:
*R*<sup>2</sup> = 1 − 839/1199 = 0.3003.

\[/S\]

\[P=1\]

Among the tested effects, which variable explains the largest part of
deviance? Justify by calculating this proportion.

\[S\]

This question is similar to the previous one about the explicative power
of the model. Here the largest proportion of explained deviance is
obtained with the `sl` variable, it is equal to : 177,9 / 1199, that is
0.148.

The two other effects have associated smaller deviance.

\[/S\]

\[P=2\]

Interpret the results provided by the analysis of deviance
(Tbl. **¿tbl:anova?**). In particular, what does the result regarding
the effect designated as `'tactic:sl'` indicate?

\[S\]

The analysis of deviance shows that the three components of the
calibrated statistical model have significant effects: the model
contains the two principal effects for the two variables `tactic`
(qualitative) and `sl` (quantitative), and an interaction term between
these variables. Here the interaction is between a quantitative and a
qualitative (like in an ANCOVA), and not between two qualitative
variables (as in a two-way ANOVA, or ANOVA2): it allows to model an
effect of individual size (`sl`) that depends on the other variable
(mating strategy). The effect of size being modelled as a linear
relationship, this means that the slope of this relationship vary from
one class of the factor ‘mating strategy’ to another.

\[/S\]

\[/P\]

\[Q=\\half\]

What is the total number of observations in the model?

\[S\]

The total number of observations used in the model can be deduced from
the number of degrees of freedom. For instance, the null model has 899
associated degrees of freedom which is *n* − 1, where *n* is the total
sample size. Hence the total number of observations or sample size is
900.

\[/S\]

\[Q=\]

Table **¿tbl:summ?** presents statistical outputs summarizing the
calibrated model and effects.

\[P\]

\[P=1\\half\]

Indicate the numerical values that are replaced by the letters ‘A’, ‘B’,
and ‘C’ in table **¿tbl:anova?**.

\[S\]

The correct numerical values are:

*A* = 2, *B* = 896, *C* = 1031.7

-   *A* is the number of degrees of freedom associated with the
    explicative variable (factor) `tactic` that has 3 classes, and hence
    only 2 degrees of freedom associated (two independent classes).
-   *B* is the number of residuals degrees of freedom (not represented
    in explicative variables) of the model of the corresponding line,
    here 896 (one d.f. added compared to the model on the line above).
-   *C* is residual deviance of the model represented on the line, here
    it is the simple model with one explicative variable only
    (`tactic`). The null (maximal) deviance is indicated on the first
    line, the value of C is thus *C* = 1199 − 167.3 = 1031.7.

\[/S\]

\[P=1\]

What does the coefficient named `'tactisneak'` represent in the table?

\[S\]

The coefficient named `tacticsneak` represents the effect of the ‘sneak’
class on the probability of presence of a red spot. It quantifies the
difference in the probability of a red spot presence (on the logit
scale) between the ‘sneak’ mating strategy and the other classes.

\[/S\]

\[P=1\]

Same question for the `'sl'` line ? and for `'tacticsneak:sl'` line?

\[S\]

The coefficient named `'sl'` represents the effect of individual size on
the probability of presence of a red spot: it quantifies the slope of
the (linear) relationship between the variable `sl` and the probability
of presence (transformed by ‘logit’, i.e. on logit scale).

The coefficient named `'tacticsneak:sl'` refers to the interaction term
included in the model, but this interaction here is between a
qualitative and a quantitative variable. This coefficient represents the
slope of the relationship between individual length (`sl`) and the
probability of a red spot presence *for the ‘sneak’ class of the
`tactic` variable.*

\[/S\]

\[P=1\]

Why is there no coefficient indicated for the `'fem'` class of the
`'tactic'` variable (i.e. no line named `'tacticfem'`) in this table?

\[S\]

As seen in a previous question, the number of degrees of freedom
associated with ther `tactic` is the number of classes minus one, here 3
- 1 = 2 independant coefficients to be estimated to calibrate the model.
By default, when coefficients representing class effects are estimated,
the coefficient for the first class (here ‘fem’ in alphabetic order) is
constrained and set to 0, the other coefficients can then be estimated
and interpreted as relative effects in comparison with the first class.
This is called calculating ‘contrasts by treatment’.

\[/S\]

\[P=1\]

Which classes of the `tactic` factor have the highest and lowest effect
on the probability of presence of a red spot? (coefficients were
calculated ‘by treatment’).

\[S\]

By convention, when the contrasts (between classes) are calculated by
treatment, the first class (here ‘fem’) gets a coefficient of value 0.
Here in the calibrated model, the two other coeficients are estimated to
the following values (see @\[Tbl:summ\]): 2.896 for the ‘sneak’ class
and -0.3064’ for the ‘terr’ class. Hence, the class that gets the
highest estimated (relative) effect is the ‘sneak’ class, where the
lowest effect corresponds to the ‘terr’, females having an (arbitrary)
estimated value of 0. Note however that the two estimated coefficients
are poorly estimated so that their values are not significantly
different from 0 (see p-values in Tbl. **¿tbl:summ?**).

\[/S\]

\[/P\]

\[Q=1\]

Which other explicative variable, not included in the model but present
in the dataset, could have an effect on the probability of a red dot
presence? Explain why.

\[S\]

As seen in the exploratory graphic, the dataset contains an unsed
variable indicating the population of origin in which each individual
had been sampled. Depending on the characteristics of the biotope
hosting the population, one can assume that there is some variability in
the probability of presence of a red spot due to local environmental
conditions. The population variable ‘pop’ could thus be included in the
model.

\[/S\]

\[Q=1\]

Interpret the differences in the estimated relationships as shown on
Fig. **¿fig:fits?** in relation with the statistical results.

\[S\]

Fig. **¿fig:fits?** shows the estimated relationships between individual
length and the probability of red spot presence for the three different
classes of mating strategies. The form of the curves depend on the
estimated coefficients: the model contains a general intercept and a
general effect of individual size (coeff. `sl`). Then for each class of
mating strategy, it also specifies a particular constant term that adds
to the general intercept (coeff. of the principal effect of `tactic`),
and an additional coefficient that modifies the slope for a given class
(due to the interaction between `tactic` and `sl`).

The figure shows that the effect is less strong in females that in the
other classes. The lowest inflexion point and steepest slope is obtained
for the sneaker class.

\[/S\]

\[Q=2\]

Synthesis: according to this analysis and these results, which
conclusion(s) can be drawn on the role of the red spot for reproduction
in the pumpkinseed fish?

\[S\]

Being mostly a male attribute, the red spot likely serves as attracting
signal in sexual selection for reproduction. It is overall a signal of
male maturation and body size. The presence of a red spot in females is
likely due to pleiotropic genetic effects. The red spot allows smaller
males that are not territorial and do not invest in parental care to be
identified as potential partners by females. Overall the results show
that territorial males are generally larger in size than sneaking males
and females, whereas females are also generally larger than sneaking
males. There is no effect of the males mating strategy on the presence
of a red spot, but the relationship with fish size changes with mating
strategy. Sneaking males have a stronger probability of displaying of
red spot at smaller size compared to territorial males.

\[/S\]

\[/Q\]

[1] Perche arc-en-ciel

[2] \[@zieba\_red\_2018\]

[3] to sneak in: entrer furtivement
