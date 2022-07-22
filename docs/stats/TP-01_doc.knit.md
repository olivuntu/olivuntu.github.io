---
title: "Multiple Correspondence Analysis (MCA) of ecological data - ED-P01"
subtitle: Ecological Data Analysis (ECODATA) - Master 1 BEST-T
date:
fontsize: 11pt
logo: true
lang: english
numbered: true
toc: true
toc-depth: 2
multitoc: 2 
geometry: margin = 2.5cm
newcom: newcom
abstract: Introductory practical work on multivariate analyses with focus on Multiple Correspondence Analysis (MCA). MCA concerns information tables containing _qualitative variables_ only. We then seek to describe the structure of the table concerned in terms of relationships between variables and distances of individuals along the principal components.
graphicspath:
    - /home/olive/Boulot/images/
    - /home/olive/Boulot/uni/figures/
biblatex: true
bibliography: /home/olive/Boulot/uni/master.bib
---

-------------------------------------------




There are several packages for performing multivariate analyses in \Rlogo. Here we use the packages `ade4` and `FactoMineR`. 


```r
> # Useful packages (use 'install.packages' if not installed)
> library(ade4)       # multivariate analyses
> library(factoextra) # graphics for multivariate analyses (loads FactoMineR)
```

```
## Welcome! Want to learn more? See two factoextra-related books at https://goo.gl/ve3WBa
```

```r
> library(purrr)      # useful programming functions 'map_...'
> library(FactoMineR)
```

```
## 
## Attaching package: 'FactoMineR'
```

```
## The following object is masked from 'package:ade4':
## 
##     reconst
```

# MCA: Multiple Correspondence Analysis

## Principle

The MCA makes it possible to describe the structure of the correlations between qualitative variables^[The case of a mixture of variables of different types (qualitative and quantitative variables) can be treated using an analysis of Hill and Smith (`dudi.hillsmith`).] and reduce the dimension of an initial data array. To do this, we identify the principal components maximizing the correlation ratios with the initial variables.


## Steps

Conducting a MCA on a dataset involves the following steps: 

1. prepare the data (select and clean),
2. run the analysis,
3. evaluate the number of principal components required to correctly describe the data,  
4. interprete the PC in relation with initial variables and the relationships between variables as well,
5. describe the distribution of individuals on the factorial plans.



## Biological example

### Malaveae in the Mascarenes

#### Case description

The _Dombeya_ genus is one of the largest genera in the _Malvaceae_ family (formerly in the _Sterculiaceae_ family) $\approx$ 255 species). These species are naturally present throughout Africa, Madagascar and the Mascarenes archipelago. In the Mascarenes, and in La Réunion in particular, the genus is a classical example of an evolutionary radiation. Species in this genus are mostly trees and occurs mostly in forest at mid-elevation. Biological and ecological characters are however highlt variable in this genus both across and within species. Apart from _Dombeya_, _Trochettia_ is another related genus and one species of _Artisia_ is also present in Mauritius ([@le_pechon_systematics_2009], [@le_pechon_multiple_2010]).


#### Botanical and ecological characters of species

Data were extracted and adapted from \href{http://mahots.univ-reunion.fr/web/}{http://mahots.univ-reunion.fr/web/}.

All variables are factors describing biological or ecological characteristics of the species.

* `grofor`: typical species growth-form as tree ($> 7$ m), small tree ($> 7$ m), or shrub,
* `altir`: altitudinal range (`low`: not present over 800 m asl, `mid`: present up to 1600 m asl, can be present at lower elevation, `high`: present over 1600 m, not present below 600 m,
* `hab`: most often found in humid forests in its altitudinal range (`hum`) or dry forests (`dry),
* `flownb`: number of flower per inflorescence,  1 [1-3] | 2 [4 - 30] | 3 [> 30], 
* `petalcol`: color of petals (mixed : pink / white, pink / carmin),
* `blade` : oval-shaped leaf blade (`oval`), ellipsoid (`ell`), or variable (`mix`),
* `cuspid` : presence of 3 cuspids on the leaves,
* `decid`: deciduous (1) or permanent (0) foliage, 
* `inflo` : inflorescence type,
* `sex`: reproduction system with separate sex (`dioc` for dioecious, `her`: hermaphrodite, or `mix`: unclear !)



**The principal objective of the study is to describe the relationships between species characteristics and to highlight the (dis)similarities between species.**

More precisely, the questions that we want to adress are:

* Is there any similarity between species and in relation with which characteristics ?
* Are there any species clusters in species according to the studied characteristics ?
* Are there any consistent groups of species in relation with their origin ?


### Data import


```r
> maho <- read.table("data/mahots.csv", header = T, sep = ",", na.strings = "")
> # Column names
> names(maho)
```

```
##  [1] "tax_id"    "name"      "distr"     "grofor"    "altir"     "hab"       "blade"     "margin"    "cusp"      "acum"      "heterop"   "bladehair" "inflo"     "flownb"    "petalcol" 
## [16] "stami"     "decid"     "sex"
```

```r
> # Data summary
> summary(maho)
```

```
##     tax_id              name              distr              grofor             altir               hab               blade               margin            cusp             acum     
##  Length:26          Length:26          Length:26          Length:26          Length:26          Length:26          Length:26          Min.   :0.0000   Min.   :0.0000   Min.   :0.00  
##  Class :character   Class :character   Class :character   Class :character   Class :character   Class :character   Class :character   1st Qu.:0.0000   1st Qu.:0.0000   1st Qu.:0.00  
##  Mode  :character   Mode  :character   Mode  :character   Mode  :character   Mode  :character   Mode  :character   Mode  :character   Median :0.0000   Median :0.0000   Median :0.00  
##                                                                                                                                       Mean   :0.1154   Mean   :0.2692   Mean   :0.44  
##                                                                                                                                       3rd Qu.:0.0000   3rd Qu.:0.7500   3rd Qu.:1.00  
##                                                                                                                                       Max.   :1.0000   Max.   :1.0000   Max.   :1.00  
##                                                                                                                                                                         NA's   :1     
##     heterop      bladehair            inflo               flownb        petalcol             stami            decid          sex           
##  Min.   :0.00   Length:26          Length:26          Min.   :1.000   Length:26          Min.   :0.0000   Min.   :0.00   Length:26         
##  1st Qu.:0.00   Class :character   Class :character   1st Qu.:2.000   Class :character   1st Qu.:1.0000   1st Qu.:0.00   Class :character  
##  Median :0.00   Mode  :character   Mode  :character   Median :2.000   Mode  :character   Median :1.0000   Median :0.00   Mode  :character  
##  Mean   :0.24                                         Mean   :2.115                      Mean   :0.8846   Mean   :0.28                     
##  3rd Qu.:0.00                                         3rd Qu.:3.000                      3rd Qu.:1.0000   3rd Qu.:1.00                     
##  Max.   :1.00                                         Max.   :3.000                      Max.   :1.0000   Max.   :1.00                     
##  NA's   :1                                                                                                NA's   :1
```

```r
> # Transform columns into factors and the object back to a data frame
> maho <- map_df(maho, as.factor) %>% as.data.frame()
> # equivalent to: 
> maho <- as.data.frame(map_df(maho, as.factor))
> 
> # Select columns for the analysis
> n <- c("tax_id", "distr", "altir", "grofor", "hab", "blade", 
+ 			 "cusp", "acum", "heterop", "bladehair", "inflo", "flownb",
+ 			 "petalcol", "decid", "sex")
> maho <- maho[, n]
```

Because some classes show low numbers, we choose to gather them in larger classes.


```r
> # Recode levels for some factors / classes with low numbers
> levels(maho$distr)
```

```
## [1] "M"    "R"    "R-Ro" "Ro"
```

```r
> levels(maho$distr) <- c("Mas", "R", "R", "Mas")
> levels(maho$inflo)
```

```
## [1] "bip" "omb" "sol" "tri"
```

```r
> levels(maho$inflo) <- c("bip", "omb", "so-tr", "so-tr")
```

Missing data are not allowed in ordination analyses, they need to be either suppressed or informed (estimated). Here we suppress taxa with missing data.


```r
> # Suppress lines with missing data (NA)
> maho <- na.omit(maho)
> # Change line names for graphics
> rownames(maho) <- maho$tax_id
```


_How many species are left ?_


## Performing the analysis


```r
> # MCA on selected data
> # (suppressing columns 'tax_id', 'distr')
> tmp <- maho[, -c(1, 2)] # suppressed columns
> acm <- dudi.acm(tmp, scann = T)
```

How many axes can we retain ? 

Eigen values are stored in the component of the result object (`acm`) called `eig`. They indicate the power of synthesis relative to each the corresponding principal component of the analysis. They can be represented in a barplot, or in a special _scree plot_.





```r
> # Vecteur des valeurs propres
> lambda <- acm$eig
> barplot(lambda)
```

\begin{figure}

{\centering \includegraphics[width=0.6\linewidth]{../figures/TP-bar-1} 

}

\caption{Barplot of eigen values ($\lambda$).}\label{fig:bar}
\end{figure}


## Quality of the analysis

The quality of the analysis refers to the power of synthesis of the principal components of the analysis.


Same graph, but using the function `fviz_mca`.


```r
> fviz_eig(acm, addlabels = T)
```

\begin{figure}

{\centering \includegraphics[width=0.6\linewidth]{../figures/TP-bar2-1} 

}

\caption{Scree plot showing the power of synthesis of MCA axes.}\label{fig:bar2}
\end{figure}



```r
> # Commande pour retenir directement un nombre d'axes (argument 'nf')
> acm <- dudi.acm(maho[, -(1 : 2)], scan = FALSE, nf = 3)
```


```r
> # Eigen values of the analysis
> lambda <- acm$eig
> # Total inertia
> # = sum of eigen values
> iner.tot <- sum(lambda)
> # Proportion of inertia for three principal components
> p <- sum(lambda[1 : 3]) / iner.tot * 100
> round(p, digits = 1)
```

```
## [1] 52.5
```

## Decomposition of total inertia: contributions

Decomposing the total inertia allows to evaluate the contribution of lines (species here) or columns (variables) to the building of the principal components in the analysis.



```r
> # Décomposition of total inertia
> iner <- inertia.dudi(acm)
> iner
```

```
## Inertia information:
## Call: inertia.dudi(x = acm)
## 
## Decomposition of total inertia:
##        inertia     cum  cum(%)
## Ax1  0.3470662  0.3471   21.49
## Ax2  0.2878299  0.6349   39.30
## Ax3  0.2136113  0.8485   52.53
## Ax4  0.1558715  1.0044   62.18
## Ax5  0.1246125  1.1290   69.89
## Ax6  0.1132112  1.2422   76.90
## Ax7  0.0934491  1.3357   82.68
## Ax8  0.0555572  1.3912   86.12
## Ax9  0.0489396  1.4401   89.15
## Ax10 0.0459567  1.4861   92.00
## Ax11 0.0394626  1.5256   94.44
## Ax12 0.0287239  1.5543   96.22
## Ax13 0.0210467  1.5753   97.52
## Ax14 0.0198594  1.5952   98.75
## Ax15 0.0073088  1.6025   99.20
## Ax16 0.0046016  1.6071   99.49
## Ax17 0.0042862  1.6114   99.75
## Ax18 0.0024198  1.6138   99.90
## Ax19 0.0011974  1.6150   99.98
## Ax20 0.0003734  1.6154  100.00
```

```r
> # Représentations graphiques (pour l'axe 1)
> # Contributions of indivduals (lines = species here)
> fviz_contrib(acm, choice = "ind")
```

\begin{figure}

{\centering \includegraphics[width=0.6\linewidth]{../figures/TP-inert1-1} 

}

\caption{Contribution of individuals to the first component of the MCA. Note that labels on x-axis refer to row names in the analyzed table.}\label{fig:inert1}
\end{figure}



```r
> # Contributions des variables (colonnes)
> fviz_contrib(acm, choice = "var")
```

\begin{figure}

{\centering \includegraphics[width=0.6\linewidth]{../figures/TP-contrv-1} 

}

\caption{Contribution of variables (binary, hence classes) to the first component of the MCA.}\label{fig:contrv}
\end{figure}

_Inspect the contributions of species and their characteristics to the other component retained to summarize the structure of the studied dataset._



## Graphical representation of the results

### General view



```r
> #scatter(acm)
> fviz_mca(acm)
```

\begin{figure}

{\centering \includegraphics[width=0.6\linewidth]{../figures/TP-gacm-1} 

}

\caption{Projection of individual on the first factorial (first two principal components) of the MCA. Points represent species, ellipses represent classes for the different factors (labels indicate the centroid of species).}\label{fig:gacm}
\end{figure}

### Inspecting lines and columns



```r
> score(acm)	# projète les variables
```

\begin{figure}

{\centering \includegraphics[width=0.6\linewidth]{../figures/TP-sacm-1} 

}

\caption{Species coordinates on the firt principal component (first axis, $C_1$) of MCA.}\label{fig:sacm}
\end{figure}


```r
> #library(FactoMineR)
> acm2 <- MCA(maho[, -(1:2)])
> fviz_mca_var(acm2)
```




### Relationships with geographical origin

An interesting way to use the output of an ordination analysis in general is to inspect the discrimintation of the statistical units according to some factor of interest. Here we wish to address the likelihood of species possessing different characters according to their geographical origin.

A usual way of representing supplementary informatioln coded in a factor is to represent ellipses around the points in each classes. These ellispes may be defined in different ways, including using confidence intervals on the average scores within classes. 


```r
> fviz_mca_ind(acm, habillage = maho$distr, addEllipses = TRUE)
```



\begin{center}\includegraphics[width=0.6\linewidth]{../figures/TP-clad2-1} \end{center}

Another option is to represent _minimim convex polygons_ (hulls), i.e. polygons of minimal surface being convex and containing all points in the concerned class.



```r
> s.chull(dfxy = acm$li, fac = maho$distr, optchull = 1, col = c("green", "red"))
```

\begin{figure}

{\centering \includegraphics[width=0.6\linewidth]{../figures/TP-poly2-1} 

}

\caption{Minimum convex polygons drawn around species in the first factorial plan ($C_1, C_2)$.}\label{fig:poly2}
\end{figure}


To refine the analysis, one can select less variables at start by removing those that are not discriminant.



## Finalize the analysis

_Answer the research questions having inspecting the results for all the retained axes._ 


