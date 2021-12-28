# Demonstratives: Discrepancy Coding Procedure
Repository for storing supplementary materials and data to accompany "TODO: TITLE" (2022). Part of [**Demonstratives: Main Study**](https://github.com/DominikDziedzic/DemonstrativesMain) repository.

The results of the Discrepancy Coding Procedure are presented and discussed. The Procedure is a part of Demonstratives: Main Study. The task discussed in the Main Study was the following: a sample of scenarios, in which utterances of ordinary demonstrative sentences are made, was presented to participants; each sentence was presented as either true or false in the described context. The study aimed to measure participants’ subjective beliefs regarding the nature of the stimuli and the effectiveness of experimental manipulation. The sample in the Main Study consists of TODO: NUMBER scenarios. However, the initial sample consisted of 26 scenarios. 

This repository is concerned primarily with the results of the following coding task: a team of 8 professional philosophers (4 Ph.D.'s and 4 Ph.D. candidates with interest in semantics and indexicality in particular; excluding the author of the current study) categorized the stimuli into two categories:
- **Possible discrepancy [+_d_] between actual demonstration and intended referent**,
- **No discrepancy [-_d_] between actual demonstration and intended referent**.

To view the form that the coders were requested to fill in, visit:
- [**Discrepancy Coding Results**](https://github.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/blob/main/Discrepancy%20Coding%20Results.pdf) (contains the results and coders' comments on each of the stimulus) or
- [**Discrepancy Coding Form**](https://github.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/blob/main/Discrepancy%20Coding%20FormCLEAN.pdf).

 ---

Content of the repository (after opening each file, right-click and select Save as):
- **Raw data** 
  - data - Probability per Scenario [in .txt](https://raw.githubusercontent.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/main/data%20-%20Probability%20per%20Scenario.txt) or [.csv format](https://raw.githubusercontent.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/main/data%20-%20Probability%20per%20Scenario.csv) 
  - data - Probability per Coder [in .txt](https://raw.githubusercontent.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/main/data%20-%20Probability%20per%20Coder.txt) or [.csv format](https://raw.githubusercontent.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/main/data%20-%20Probability%20per%20Coder.csv)
- [**Source code in .R**]() TODO
- **Entire Sample of 26 Scenarios:**
  - [**Categorization (26)**](https://github.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/blob/main/README.md#categorization-26)
  - [**Analysis of the Contingency Table (26)**](https://github.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/blob/main/README.md#analysis-of-the-contingency-table-26)
  - [**Similarity Between Coders - or Lack Thereof (26)**](https://github.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/blob/main/README.md#similarity-between-coders---or-lack-thereof-26)
- **Revised Sample of 20 Scenarios:**
  - [**Categorization (20)**](https://github.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/blob/main/README.md#categorization-20)
  - [**Analysis of the Contingency Table (20)**](https://github.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/blob/main/README.md#analysis-of-the-contingency-table-20)
  - [**Similarity Between Coders - or Lack Thereof (20)**](https://github.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/blob/main/README.md#similarity-between-coders---or-lack-thereof-20)
- [**References**](https://github.com/DominikDziedzic/DemonstrativesDiscrepancyCoding#references)

## Required Packages

Run the following code in R to install the required packages:
``` r
install.packages("ggplot2")
install.packages("dplyr")
install.packages("tidyverse")
install.packages("lsa")
```

Load the required packages:
``` r
library(ggplot2)
library(dplyr)
library(tidyverse)
library(lsa)
```

## Entire Sample of 26 Scenarios

## Categorization (26)

### Import data
Download raw data, "data - Probability per Scenario", in [.txt](https://raw.githubusercontent.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/main/data%20-%20Probability%20per%20Scenario.txt) or [.csv format](https://raw.githubusercontent.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/main/data%20-%20Probability%20per%20Scenario.csv) and run the following in R to import the data:

``` r
# a) if in .txt:
data <- read.delim(file.choose())
# b) if in .csv:
data <- read.csv(file.choose(), sep = ";")
```

Let's review the dataset:

``` r
> str(data)
'data.frame':	26 obs. of  3 variables:
 $ scenario: chr  "(Reimer, 1991a, pp. 190–191)" "(Perry, 2017, p. 979)" "(Siegel, 2002, pp. 10–11)" "(McGinn, 1981, p. 162)" ...
 $ ND      : int  3 6 1 2 1 6 3 6 5 8 ...
 $ PD      : int  5 2 7 6 7 2 5 2 3 0 ...
```
The dataset consists of the variable `scenario` that holds the citations in an (author, year, page numbers) format. To view the list of references from which the citations come, see the last page of [**Discrepancy Coding Results**](https://github.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/blob/main/Discrepancy%20Coding%20Results.pdf). Column `ND` corresponds to "**No discrepancy [-_d_] between actual demonstration and intended referent**" response on the Discrepancy Coding Form, while column `PD` tracks "**Possible discrepancy [+_d_] between actual demonstration and intended referent**". For example, the scenario adapted from (Reimer, 1991a, pp. 190–191) was categorized as **[-_d_]** scenario by 3 coders and as **[+_d_]** by 5 coders.

Given the nature of the Coding Procedure and the character of the variables `ND` and `PD`, it can be stated that the dataset is of caterogical type.

### Compute the probabilities of categorizing the given scenarios

Find the number of observations.
``` r
> data$N <- data$ND + data$PD
> str(data)
'data.frame':	26 obs. of  4 variables:
 $ scenario: chr  "(Reimer, 1991a, pp. 190–191)" "(Perry, 2017, p. 979)" "(Siegel, 2002, pp. 10–11)" "(McGinn, 1981, p. 162)" ...
 $ ND      : int  3 6 1 2 1 6 3 6 5 8 ...
 $ PD      : int  5 2 7 6 7 2 5 2 3 0 ...
 $ N       : int  8 8 8 8 8 8 8 8 8 8 ...
```

Compute the arithmetic means.

``` r
> data$prob.ND <- data$ND / data$N
> data$prob.PD <- data$PD / data$N
> str(data)
'data.frame':	26 obs. of  6 variables:
 $ scenario: chr  "(Reimer, 1991a, pp. 190–191)" "(Perry, 2017, p. 979)" "(Siegel, 2002, pp. 10–11)" "(McGinn, 1981, p. 162)" ...
 $ ND      : int  3 6 1 2 1 6 3 6 5 8 ...
 $ PD      : int  5 2 7 6 7 2 5 2 3 0 ...
 $ N       : int  8 8 8 8 8 8 8 8 8 8 ...
 $ prob.ND : num  0.375 0.75 0.125 0.25 0.125 0.75 0.375 0.75 0.625 1 ...
 $ prob.PD : num  0.625 0.25 0.875 0.75 0.875 0.25 0.625 0.25 0.375 0 ...
```

Define some "discrimination thresholds", or simply "thresholds", for later use as the legend keys on the plot.

``` r
> data$prob.thresholds <- as.factor(ifelse(data$prob.PD < 0.5, "0",
+                                          ifelse(data$prob.PD == 0.5, "0.5", "1")
+                                          ))
> data$prob.thresholds
 [1] 1   0   1   1   1   0   1   0   0   0   0   0   0   0.5 0   0   1   1   1   0   1   0   0   0   0   0.5
Levels: 0 0.5 1
```

Specify the order in the dataset: increasing PD.
``` r
data.sort <- data[with(data, order(data$prob.PD, scenario)),]
```

### Plot (26)

![Categorization output (26)](https://github.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/blob/main/Output/Plot%20(26).png)
TODO: Description.

## Analysis of the Contingency Table (26)

In what follows, the chi-square and Fisher's exact tests are conducted to determine whether there is an association between two categorical variables: i) the categories assigned by the respondents during the coding process (i.e., `PD`), and ii) the categories shown on the plot (i.e., `prob.thresholds`). The two variables are likely to be statistically related, obviously.

Let us address the following question first: How to proceed with scenarios that were categorized as ND as often as they were as PD during the coding process? There are two such scenarios:

```r
> data$scenario[which(data$prob.thresholds == 0.5)]
[1] "(Reimer, 1991b, p. 180)" "(Perry, 2009, p. 193)"
```

One of them is (Reimer, 1991b, p. 180), the other: (Perry, 2009, p. 193).
``` r
Reimer <- which(data$scenario == "(Reimer, 1991b, p. 180)")
Perry <- which(data$scenario == "(Perry, 2009, p. 193)")
```
Does it make a difference if these two scenarios fall into ND or PD category? Let us consider 4 possibilities: i) both into ND, ii) Reimer's into ND, Perry's into PD, iii) vice versa, iv) both into PD.

``` r
> for (i in 1:4){
+   col.tmp <- paste0("prob.thresholds", i)
+   data[, col.tmp] <- data$prob.thresholds
+ }
> 
> {
+   data$prob.thresholds1[Reimer] <- 0 # i)
+   data$prob.thresholds1[Perry] <- 0
+   data$prob.thresholds2[Reimer] <- 0 # ii)
+   data$prob.thresholds2[Perry] <- 1
+   data$prob.thresholds3[Reimer] <- 1 # iii)
+   data$prob.thresholds3[Perry] <- 0
+   data$prob.thresholds4[Reimer] <- 1 # iV)
+   data$prob.thresholds4[Perry] <- 1
+ }
```
Create datasets to store the p-values of the tests performed on the i)-iv).

``` r
> chi.values <- data.frame(matrix("", ncol = 0, nrow = 1))
> fisher.values <- data.frame(matrix("", ncol = 0, nrow = 1))
> 
> for (i in 1:4){
+   col.tmp <- paste0("prob.thresholds", i)
+   data[, col.tmp] <- droplevels(data[, col.tmp]) # Clean the variables
+   
+   dat.tmp <- data[,c(3,7+i)]
+   tab.tmp <- table(dat.tmp) # Construct contingency tables
+   chi.tmp <- chisq.test(tab.tmp) # Perform chi-square tests
+   Fisher.tmp <- fisher.test(tab.tmp) # Perform Fisher's Exact Test
+   
+   col.tmp <- paste0("Chisq", i, "[p-value", i, "]")
+   chi.values[, col.tmp] <- chi.tmp$p.value # Store the p-values of the tests
+   
+   col.tmp <- paste0("FET", i, "[p-value", i, "]")
+   fisher.values[, col.tmp] <- Fisher.tmp$p.value
+ }
Warning messages:
1: In chisq.test(tab.tmp) : Chi-squared approximation may be incorrect
2: In chisq.test(tab.tmp) : Chi-squared approximation may be incorrect
3: In chisq.test(tab.tmp) : Chi-squared approximation may be incorrect
4: In chisq.test(tab.tmp) : Chi-squared approximation may be incorrect
```

Display the p-values.

``` r
> chi.values %>% round(5)
  Chisq1[p-value1] Chisq2[p-value2] Chisq3[p-value3] Chisq4[p-value4]
1            5e-04          0.00119          0.00119            5e-04
```

As expected, the two variables are statistically related; that is, there is enough evidence to suggest an association between `prob.tresholds` (as specified on the plot) and the categories assigned by the coders regardless of whether the borderline cases are categorized as ND or PD (all p-values < 0.005).

However, R printed out the following warning messages:
``` r
1: In chisq.test(tab.tmp) : Chi-squared approximation may be incorrect
```

Most likely, this warning is related to the assumption that the expected cell count in the contingency table should be 5 or more in at least 80% of the cells (McHugh, 2013). As can be seen in the last computed table (containing `prob.thresholds4`), this is not the case.

``` r
> chi.tmp$expected
   prob.thresholds4
PD          0         1
  0 3.4615385 2.5384615
  1 2.3076923 1.6923077
  2 2.3076923 1.6923077
  3 0.5769231 0.4230769
  4 1.1538462 0.8461538
  5 1.7307692 1.2692308
  6 1.7307692 1.2692308
  7 1.7307692 1.2692308
```

Collapsing the rows/columns in the contingency table to increase the cell counts is usually suggested as a solution in such cases.

``` r
> tab.tmp1 <- rbind(tab.tmp["0",]+tab.tmp["1",]+tab.tmp["2",]+tab.tmp["3",],
+                   tab.tmp["4",]+tab.tmp["5",]+tab.tmp["6",]+tab.tmp["7",])
> chisq.test(tab.tmp1)

	Pearson's Chi-squared test with Yates' continuity correction

data:  tab.tmp1
X-squared = 22.064, df = 1, p-value = 2.637e-06

Warning message:
In chisq.test(tab.tmp1) : Chi-squared approximation may be incorrect
```

However, the assumption at hand seems to violated in this case as well.

``` r
> tmp$expected
            0        1
[1,] 8.653846 6.346154
[2,] 6.346154 4.653846
```

The contingency table above confirms that we should use the Fisher’s exact test instead of the chi-square test because 1/4 of the expected cells is below 5.

``` r
> fisher.values
  FET1[p-value1] FET2[p-value2] FET3[p-value3] FET4[p-value4]
1   4.800691e-06    5.45961e-06    5.45961e-06   2.070886e-06
```

From the output we see that the p-values are less than the significance level.

## Similarity Between Coders - or Lack Thereof (26)

TODO

## Revised Sample of 20 Scenarios

## Categorization (20)

TODO

## Analysis of the Contingency Table (20)

TODO

## Similarity Between Coders - or Lack Thereof (20)

TODO

## References
- McHugh, Mary L. (2013). The Chi-Square Test of Independence. _Biochemia Medica_, _23_(2), 143–149. doi:10.11613/bm.2013.018
- Müller, K., Wickham, H. (2021). tibble: Simple Data Frames. R package version 3.1.2. https://CRAN.R-project.org/package=tibble
- R Core Team (2021). R: A Language and Environment for Statistical Computing. R Foundation for Statistical Computing, Vienna, Austria. URL https://www.R-project.org/.
- Wickham, H. (2016). ggplot2: Elegant Graphics for Data Analysis. New York: Springer-Verlag.
- Wickham, H. (2019). stringr: Simple, Consistent Wrappers for Common String Operations. R package version 1.4.0. https://CRAN.R-project.org/package=stringr
- Wickham, H. (2021). forcats: Tools for Working with Categorical Variables (Factors). R package version 0.5.1. https://CRAN.R-project.org/package=forcats
- Wickham, H. (2021). tidyr: Tidy Messy Data. R package version 1.1.3. https://CRAN.R-project.org/package=tidyr
- Wickham H., François, R., Henry, L., Müller, K. (2021). dplyr: A Grammar of Data Manipulation. R package version 1.0.6. https://CRAN.R-project.org/package=dplyr
- Wickham et al., (2019). Welcome to the tidyverse. _Journal of Open Source Software_, _4_(43), 1686, https://doi.org/10.21105/joss.01686
- Wild, F. (2020). lsa: Latent Semantic Analysis. R package version 0.73.2. https://CRAN.R-project.org/package=lsa
