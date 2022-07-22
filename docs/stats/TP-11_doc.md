------------------------------------------------------------------------

# Cas d'étude

On s'intéresse à la fécondité de deux espèces de Protéacées (*Protea
lepidocarpodendron* et *Protea repens*, [@Higgins2008]), famille
botanique très diversifiée du Fynbos, écosystème emblématique d'Afrique
du Sud (Fig. 1). Le fynbos est présent en climat méditerranéen (province
du Cap) et très dépendant de la dynamique naturelle des incendies. Parmi
les Protéacées, une adaptation répandue est la sérotinie, la capacité de
produire des cônes ligneux qui restent fermés à maturité mais qui
s'ouvrent sous l'action du feu, permettant ainsi la dispersion des
graînes après perturbation de l'écosystème par les incendies.

::: {#fig:protea .subfigures}
```{=tex}
\centering
```
![a](assets/images/fynbos.jpg "fig:"){height="7cm"} ![b](/assets/images/rep-specimen.jpg "fig:"){height="7cm"}

Figure 1: Vue du fynbos en Afrique du Sud (a, noter le sol sableux),
Individu adulte de *Protea repens*, une des deux espèces étudiées (b)..
:::

```{=tex}
\justify
```
Vous souhaitez savoir si :

1.  il existe une relation allométrique entre la taille des individus et
    la fécondité,
2.  l'allométrie varie entre les deux espèces étudiées ?

## Import et exploration des données

Inspecter la structure et le résumé du tableau.

```{=tex}
\begin{knitrout}\small
\definecolor{shadecolor}{rgb}{0.969, 0.969, 0.969}\color{fgcolor}\begin{kframe}
\begin{verbatim}
## 'data.frame':    97 obs. of  4 variables:
##  $ name  : chr  "P. repens" "P. repens" "P. repens" "P. repens" ...
##  $ label : chr  "R" "R" "R" "R" ...
##  $ height: int  400 100 200 180 150 60 80 60 60 100 ...
##  $ cones : int  1000 9 230 260 75 8 6 5 10 15 ...
##      name              label               height          cones        
##  Length:97          Length:97          Min.   : 50.0   Min.   :   2.00  
##  Class :character   Class :character   1st Qu.:100.0   1st Qu.:  15.00  
##  Mode  :character   Mode  :character   Median :140.0   Median :  27.00  
##                                        Mean   :141.2   Mean   :  62.53  
##                                        3rd Qu.:170.0   3rd Qu.:  60.00  
##                                        Max.   :400.0   Max.   :1000.00
\end{verbatim}
\end{kframe}
\end{knitrout}
```
### Transformation des données

Le tableau, relativement simple, est sous un format **large** :
plusieurs colonnes (variables) pour une même unité statistique
(individu). On peut le transformer au format **long** grâce à la
fonction `melt` du *package* `reshape2`.

Inspecter la structure du nouvel objet au format long.

## Exploration graphique

Utiliser la grammaire graphique pour l'exploration suivante :

1.  Représenter les distributions univariées des variables chez les deux
    espèces.

```{=tex}
\begin{knitrout}\small
\definecolor{shadecolor}{rgb}{0.969, 0.969, 0.969}\color{fgcolor}\begin{kframe}


{\ttfamily\noindent\itshape\color{messagecolor}{\#\# `stat\_bin()` using `bins = 30`. Pick better value with `binwidth`.}}\end{kframe}
\includegraphics[width=\maxwidth]{../figures/TP-hist-1} 
\end{knitrout}
```
2.  Produire un graphique permettant de comparer les distributions des
    variables chez les deux espèces.

```{=tex}
\begin{knitrout}\small
\definecolor{shadecolor}{rgb}{0.969, 0.969, 0.969}\color{fgcolor}
\includegraphics[width=\maxwidth]{../figures/TP-boxplot-1} 
\end{knitrout}
```
3.  Représenter la relation bivariée entre la hauteur et le nombre de
    cônes (utiliser l'objet `protea`). Pensez-vous qu'une relation
    d'allométrie existe ? La relation semble-t-elle linéaire ? Examiner
    la relation en utilisant une échelle logarithmique sur l'axe
    vertical.

```{=tex}
\begin{knitrout}\small
\definecolor{shadecolor}{rgb}{0.969, 0.969, 0.969}\color{fgcolor}
\includegraphics[width=\maxwidth]{../figures/TP-bivar-1} 
\end{knitrout}
```
```{=tex}
\begin{knitrout}\small
\definecolor{shadecolor}{rgb}{0.969, 0.969, 0.969}\color{fgcolor}
\includegraphics[width=\maxwidth]{../figures/TP-bivarlog-1} 
\end{knitrout}
```
## Transformation des données

Les données issus de comptage d'objets en relative grand nombre (ici les
cônes) sont souvent distribués de façon *log-normale* : une
transformation logarithmique rend les distributions plus proches d'une
distribution normale[^1].

Ajouter une colonne correspondant au logarithme du nombre de cônes au
tableau.

# Analyse de corrélation

## Estimation de la covariance

Estimer la valeur de la covariance entre la hauteur des individus et le
nombre de cônes transformées par la fonction `log`.

```{=tex}
\begin{knitrout}\small
\definecolor{shadecolor}{rgb}{0.969, 0.969, 0.969}\color{fgcolor}\begin{kframe}
\begin{verbatim}
## [1] 58.16299
\end{verbatim}
\end{kframe}
\end{knitrout}
```
## Calcul des coefficients de corrélation

Réaliser des tests de corrélation en utilisant deux coefficients de
corrélation différents. Comparer les résultats.

```{=tex}
\begin{knitrout}\small
\definecolor{shadecolor}{rgb}{0.969, 0.969, 0.969}\color{fgcolor}\begin{kframe}
\begin{alltt}
\hlstd{> }\hlcom{# Coefficient de corrélation de Pearson}
\hlstd{> }\hlstd{test1} \hlkwb{<-} \hlkwd{cor.test}\hlstd{(protea}\hlopt{$}\hlstd{height,  protea}\hlopt{$}\hlstd{lcones)} \hlcom{# par défaut}
\hlstd{> }\hlstd{test1}
\end{alltt}
\begin{verbatim}
## 
##  Pearson's product-moment correlation
## 
## data:  protea$height and protea$lcones
## t = 16.217, df = 95, p-value < 2.2e-16
## alternative hypothesis: true correlation is not equal to 0
## 95 percent confidence interval:
##  0.7932566 0.9023011
## sample estimates:
##       cor 
## 0.8570992
\end{verbatim}
\begin{alltt}
\hlstd{> }\hlcom{# Coefficient de corrélation de Spearman}
\hlstd{> }\hlstd{test2} \hlkwb{<-} \hlkwd{cor.test}\hlstd{(protea}\hlopt{$}\hlstd{height,  protea}\hlopt{$}\hlstd{lcones,}  \hlkwc{method} \hlstd{=} \hlstr{"spearman"}\hlstd{)}
\end{alltt}


{\ttfamily\noindent\color{warningcolor}{\#\# Warning in cor.test.default(protea\$height, protea\$lcones, method = "{}spearman"{}): Cannot compute exact p-value with ties}}\begin{alltt}
\hlstd{> }\hlstd{test2}
\end{alltt}
\begin{verbatim}
## 
##  Spearman's rank correlation rho
## 
## data:  protea$height and protea$lcones
## S = 20436, p-value < 2.2e-16
## alternative hypothesis: true rho is not equal to 0
## sample estimates:
##       rho 
## 0.8656393
\end{verbatim}
\end{kframe}
\end{knitrout}
```
Retrouver la valeur du coefficient de corrélation de Pearson à partir de
la covariance.

```{=tex}
\begin{knitrout}\small
\definecolor{shadecolor}{rgb}{0.969, 0.969, 0.969}\color{fgcolor}\begin{kframe}
\begin{verbatim}
## [1] 0.8570992
\end{verbatim}
\end{kframe}
\end{knitrout}
```
Vérifier que le coefficient de Spearman est le même sur les données
nonè-transformées (rangs inchangés par la transformation `log`)

```{=tex}
\begin{knitrout}\small
\definecolor{shadecolor}{rgb}{0.969, 0.969, 0.969}\color{fgcolor}\begin{kframe}
\begin{verbatim}
## [1] 0.8656393
\end{verbatim}
\end{kframe}
\end{knitrout}
```
# Modèle linéaire

Un modèle statistique s'écrit toujours sous forme d'une formule dans
`\Rlogo`{=tex} : $Y\sim X$. On considère ici la relation entre le nombre
de cônes et la hauteur des individus.

## Modèle sur données initiales

1.  Estimer les paramètres de la relation précédente en calibrant un
    modèle linéaire simple (fonction `lm`).

```{=tex}
\begin{knitrout}\small
\definecolor{shadecolor}{rgb}{0.969, 0.969, 0.969}\color{fgcolor}\begin{kframe}
\begin{verbatim}
## 
## Call:
## lm(formula = cones ~ height, data = protea)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -134.73  -42.68   -3.00   34.68  552.07 
## 
## Coefficients:
##              Estimate Std. Error t value Pr(>|t|)    
## (Intercept) -147.8342    21.4760  -6.884 6.23e-10 ***
## height         1.4894     0.1408  10.579  < 2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 79.9 on 95 degrees of freedom
## Multiple R-squared:  0.5409, Adjusted R-squared:  0.536 
## F-statistic: 111.9 on 1 and 95 DF,  p-value: < 2.2e-16
\end{verbatim}
\end{kframe}
\end{knitrout}
```
2.  Etudier la structure des deux objets `mod` et `res` : classe, noms,
    éléments.

## Diagnostic du modèle

Le diagnostic d'un modèle linéaire passe par l'examen :

1.  de la normalité des résidus :
    1.  Représenter un graphique quantiles-quantiles des résidus
        (*residuals*, `$residuals`, ou fonction `resid()`)
    2.  Tester la normalité à l'aide d'un test

```{=tex}
\begin{knitrout}\small
\definecolor{shadecolor}{rgb}{0.969, 0.969, 0.969}\color{fgcolor}\begin{kframe}
\begin{verbatim}
## 
##  Shapiro-Wilk normality test
## 
## data:  resid(mod)
## W = 0.75073, p-value = 1.55e-11
\end{verbatim}
\end{kframe}
\includegraphics[width=\maxwidth]{../figures/TP-qq-1} 
\end{knitrout}
```
2.  de l'homosédasticité des résidus : on se contente ici d'un
    diagnostic graphique.
    -   Représenter les résidus en fonction de valeurs estimées
        (*fitted*, `$fitted.values`, ou fonction `fitted()`)

```{=tex}
\begin{knitrout}\small
\definecolor{shadecolor}{rgb}{0.969, 0.969, 0.969}\color{fgcolor}
\includegraphics[width=\maxwidth]{../figures/TP-unnamed-chunk-2-1} 
\end{knitrout}
```
Dans `\Rlogo`{=tex}, on peut obtenir plusieurs graphiques diagnostics
facilement en utilisant la fonction générique `plot` (s'adapte à la
classe de l'objet).

```{=tex}
\begin{knitrout}\small
\definecolor{shadecolor}{rgb}{0.969, 0.969, 0.969}\color{fgcolor}
\includegraphics[width=\maxwidth]{../figures/TP-diag-1} 
\end{knitrout}
```
On vérifier également si les résidus sont bien centrés autour de 0
(erreur nulle en espérance).

## Modèle sur données transformées

Ce modèle est-il plus correct que le précédent ? (Visualiser les
graphiques de diagnostique)

Comparer avec un modèle sans intercept (ordonnée à l'origine) : on
utilise -1 dans la formule du modèle.

```{=tex}
\begin{knitrout}\small
\definecolor{shadecolor}{rgb}{0.969, 0.969, 0.969}\color{fgcolor}\begin{kframe}
\begin{verbatim}
## 
## Call:
## lm(formula = lcones ~ height - 1, data = protea)
## 
## Residuals:
##     Min      1Q  Median      3Q     Max 
## -2.3157 -0.1837  0.2301  0.5577  1.4120 
## 
## Coefficients:
##         Estimate Std. Error t value Pr(>|t|)    
## height 0.0230587  0.0004671   49.37   <2e-16 ***
## ---
## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
## 
## Residual standard error: 0.7017 on 96 degrees of freedom
## Multiple R-squared:  0.9621, Adjusted R-squared:  0.9617 
## F-statistic:  2437 on 1 and 96 DF,  p-value: < 2.2e-16
\end{verbatim}
\end{kframe}
\includegraphics[width=\maxwidth]{../figures/TP-mod3-1} 
\end{knitrout}
```
#### Valeurs atypiques et influentes

Les graphics diagnostics peuvent révéler la présence d'observations mal
prises en compte dans le modèle (atypiques[^2]) et / ou influentes[^3]

```{=tex}
\begin{knitrout}\small
\definecolor{shadecolor}{rgb}{0.969, 0.969, 0.969}\color{fgcolor}
\includegraphics[width=\maxwidth]{../figures/TP-out-1} 
\end{knitrout}
```
# Interpréation du modèle

## Qualité d'ajustement du modèle

La qualité d'ajustement mesure à quel point les valeurs estimées
sont-elles proches des valeurs observées, ou encore le pourcentage de
variance de la variable réponse expliquée par la (les) variable(s)
explicative(s).

```{=tex}
\begin{knitrout}\small
\definecolor{shadecolor}{rgb}{0.969, 0.969, 0.969}\color{fgcolor}\begin{kframe}
\begin{alltt}
\hlstd{> }\hlcom{# Représentation graphique}
\hlstd{> }\hlkwd{plot}\hlstd{(}\hlkwd{fitted}\hlstd{(mod4)} \hlopt{~} \hlstd{protea}\hlopt{$}\hlstd{lcones)}
\hlstd{> }\hlkwd{abline}\hlstd{(}\hlnum{0}\hlstd{,} \hlnum{1}\hlstd{)}
\hlstd{> }
\hlstd{> }\hlcom{# Coefficient de corrélation et de détermination entre valeurs observées / estimées}
\hlstd{> }\hlstd{r} \hlkwb{<-} \hlkwd{cor}\hlstd{(}\hlkwd{fitted}\hlstd{(mod4), protea}\hlopt{$}\hlstd{lcones)}
\hlstd{> }\hlstd{r}\hlopt{^}\hlnum{2}
\end{alltt}
\begin{verbatim}
## [1] 0.7250137
\end{verbatim}
\begin{alltt}
\hlstd{> }\hlcom{# Pourcentage de variance expliquée :}
\hlstd{> }\hlnum{1} \hlopt{-} \hlkwd{var}\hlstd{(}\hlkwd{resid}\hlstd{(mod4))} \hlopt{/} \hlkwd{var}\hlstd{(protea}\hlopt{$}\hlstd{lcones)}
\end{alltt}
\begin{verbatim}
## [1] 0.7250137
\end{verbatim}
\end{kframe}
\includegraphics[width=\maxwidth]{../figures/TP-r2-1} 
\end{knitrout}
```
## Interprétation des coefficients

Quelles sont les valeurs des pentes et ordonnée à l'origine du modèle
calibré ? Sont-elles significativement différentes de 0 ?

Retrouver les valeurs des coefficiens d'après les formules du cours
(solution des moindres carrés);

Représenter la relation estimée sur le graphique de la relation.

```{=tex}
\begin{knitrout}\small
\definecolor{shadecolor}{rgb}{0.969, 0.969, 0.969}\color{fgcolor}
\includegraphics[width=\maxwidth]{../figures/TP-estim-1} 
\end{knitrout}
```
```{=tex}
\begin{knitrout}\small
\definecolor{shadecolor}{rgb}{0.969, 0.969, 0.969}\color{fgcolor}\begin{kframe}


{\ttfamily\noindent\itshape\color{messagecolor}{\#\# `geom\_smooth()` using formula 'y \textasciitilde{} x'}}\end{kframe}
\includegraphics[width=\maxwidth]{../figures/TP-estim2-1} 
\end{knitrout}
```
# Comparaison entre espèces

On examine la possibilié de relations alliométriques différentes entre
les deux espèces.

```{=tex}
\begin{knitrout}\small
\definecolor{shadecolor}{rgb}{0.969, 0.969, 0.969}\color{fgcolor}\begin{kframe}


{\ttfamily\noindent\itshape\color{messagecolor}{\#\# `geom\_smooth()` using formula 'y \textasciitilde{} x'}}\end{kframe}
\includegraphics[width=\maxwidth]{../figures/TP-bivarsp-1} 
\end{knitrout}
```
Calibrer un modèle par espèce et comparer les pentes des relations.

```{=tex}
\begin{knitrout}\small
\definecolor{shadecolor}{rgb}{0.969, 0.969, 0.969}\color{fgcolor}\begin{kframe}
\begin{verbatim}
##               Estimate  Std. Error  t value     Pr(>|t|)
## (Intercept) 0.76113158 0.300345383 2.534188 1.466220e-02
## height      0.01866261 0.001898776 9.828757 5.575774e-13
##               Estimate  Std. Error   t value     Pr(>|t|)
## (Intercept) 0.57294488 0.239791983  2.389341 2.123052e-02
## height      0.02061951 0.001855083 11.115141 2.320522e-14
## [1] 0.01484486 0.02248035
## [1] 0.01688318 0.02435584
\end{verbatim}
\end{kframe}
\end{knitrout}
```
**Conclusion** : l'allométrie entre le nombre de cônes (transformé) et
la hauteur des individus est-elle différente entre les deux espèces ?

[^1]: On déconseille en général les transformations de données, car il
    s'agit bien de changer de variable : la variable concernée est
    modifiée, par une fonction mathématique (`log`, racine carrée, etc).
    La transformation reste une alternative possible pour améliorer la
    `\og `{=tex}normalité `\fg`{=tex} dans les cas de données
    non-normales.

[^2]: résidu associé extrême en valeur absolue

[^3]: qui influence fortement les valeurs estimées des paramètres de la
    relation (coefficients de régression).
