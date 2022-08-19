# Demonstratives: Discrepancy Coding Procedure
Repository for storing supplementary materials and data to accompany "TODO: TITLE" (2022). Part of [**Demonstratives: Main Study**](https://github.com/DominikDziedzic/DemonstrativesMain) repository.

The results of the Discrepancy Coding Procedure are presented and discussed. The Procedure is a part of Demonstratives: Main Study. The task discussed in the Main Study was the following: a sample of scenarios, in which utterances of ordinary demonstrative sentences are made, was presented to participants; each sentence was presented as either true or false in the described context. The study aimed to measure participants’ subjective beliefs regarding the nature of the stimuli and the effectiveness of experimental manipulation. The sample in the Main Study consists of 20 scenarios. However, the initial sample consisted of 26 scenarios. 

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

Table of contents:
- **Entire Sample of 26 Scenarios:**
  - [**Categorization (26)**](https://github.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/blob/main/README.md#categorization-26)
  - [**Analysis of the Contingency Table (26)**](https://github.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/blob/main/README.md#analysis-of-the-contingency-table-26)
  - [**Similarity Between Coders - or Lack Thereof (26)**](https://github.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/blob/main/README.md#similarity-between-coders---or-lack-thereof-26)
- **Revised Sample of 20 Scenarios:**
  - [**Categorization (20)**](https://github.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/blob/main/README.md#categorization-20)
  - [**Analysis of the Contingency Table (20)**](https://github.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/blob/main/README.md#analysis-of-the-contingency-table-20)
  - [**Similarity Between Coders - or Lack Thereof (20)**](https://github.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/blob/main/README.md#similarity-between-coders---or-lack-thereof-20)
- [**Final Sample: (Textor, 2007, p. 955) in place of "Dubliners"**](https://github.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/blob/main/README.md#appendix-textor-2007-p-955-in-place-of-dubliners)
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
Download raw data: "data - Probability per Scenario" in [.txt](https://raw.githubusercontent.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/main/data%20-%20Probability%20per%20Scenario.txt) or [.csv format](https://raw.githubusercontent.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/main/data%20-%20Probability%20per%20Scenario.csv) and run the following in R to import the data:

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

In what follows, the chi-square and Fisher's exact tests are conducted to determine whether there is an association between two categorical variables: i) the categories assigned by the respondents during the coding process (i.e., `PD`), and ii) the categories shown on the plot (i.e., `prob.thresholds`). The two variables are likely to be statistically related since the latter was determined by arranging the data stored in the former.

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

### Import data
Download raw data: "data - Probability per Coder" in [.txt](https://raw.githubusercontent.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/main/data%20-%20Probability%20per%20Coder.txt) or [.csv format](https://raw.githubusercontent.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/main/data%20-%20Probability%20per%20Coder.csv) and run the following in R to import the data:

``` r
# a) if in .txt:
data <- read.delim(file.choose())
# b) if in .csv:
data <- read.csv(file.choose(), sep = ";")
```

Review the dataset.

``` r
> head(data.c)
                      scenario  name response
1 (Reimer, 1991a, pp. 190–191) Jakub        0
2        (Perry, 2017, p. 979) Jakub        0
3    (Siegel, 2002, pp. 10–11) Jakub        1
4       (McGinn, 1981, p. 162) Jakub        1
5       (Gauker, 2008, p. 363) Jakub        0
6                    Dubliners Jakub        0
```

The dataset consists of 3 variables: scenario ID (mostly in reference format), name of the coder, and his or her response to the categorization question: "Is this a no discrepancy [-d] or possible discrepancy [+d] type of scenario in your opinion?". [-d] is coded here as 0, [+d] as 1.

Transform the dataset into its "wide" form to take a closer look at differences between coders' responses.

``` r
> data.w <- data.c %>%
+   select(scenario, name, response) %>%
+   spread(name, response)
> head(data.w)
                       scenario Jakub Maciej G. Maciej T. Maria Paweł Piotr Tadeusz Wojciech
1  (Ciecierski, Makowski, 2020)     0         0         0     0     1     0       0        1
2 (de Gaynesford, 2006, p. 169)     0         0         0     1     1     1       0        0
3        (Gauker, 2008, p. 363)     0         1         1     1     1     1       1        1
4        (Kaplan, 1978, p. 239)     0         1         1     1     1     1       0        0
5          (King, 1999, p. 156)     0         0         0     0     0     1       0        0
6          (King, 2014, p. 224)     1         1         0     1     1     1       1        0
```

For ease of calculating, remove the first column from the dataset.

``` r
> data.w <- data.w[,2:ncol(data.w)]
```

### Compare the responses to see which pairs of coders give similar answers

Method A) cosine similarity.
The first method quantifies the similarity between vectors - it measures the cosine of the angle between two vectors in a pair: the smaller the angle, the higher the similarity. It needs vectors to work - create the matrix from the dataset.

``` r
> matrix.tmp <- data.matrix(data.w)
```

Compute the cosine similarities.

``` r
> cosine.sim <- cosine(matrix.tmp)
```

Remove the superfluous data, and compute the mean and standard deviation.

``` r
> diag(cosine.sim) = NA
> mean(cosine.sim, na.rm = TRUE)
[1] 0.5868093
> sd(cosine.sim, na.rm = TRUE)
[1] 0.1388474
> cosine.sim
              Jakub Maciej G. Maciej T.     Maria     Paweł     Piotr   Tadeusz  Wojciech
Jakub            NA 0.4780914 0.1889822 0.5455447 0.6546537 0.4724556 0.6681531 0.6681531
Maciej G. 0.4780914        NA 0.4743416 0.7302967 0.7302967 0.6324555 0.6708204 0.5590170
Maciej T. 0.1889822 0.4743416        NA 0.5773503 0.5773503 0.3750000 0.3535534 0.5303301
Maria     0.5455447 0.7302967 0.5773503        NA 0.8333333 0.7216878 0.6123724 0.6123724
Paweł     0.6546537 0.7302967 0.5773503 0.8333333        NA 0.6495191 0.6123724 0.8164966
Piotr     0.4724556 0.6324555 0.3750000 0.7216878 0.6495191        NA 0.6187184 0.4419417
Tadeusz   0.6681531 0.6708204 0.3535534 0.6123724 0.6123724 0.6187184        NA 0.6250000
Wojciech  0.6681531 0.5590170 0.5303301 0.6123724 0.8164966 0.4419417 0.6250000        NA
```

Method B) probability of giving the same answer.
Try a more direct approach next: for a given pair of coders, find the number of the same answers for each scenario and divide by the number of scenarios to get the probability of giving the same answer.

``` r
> names <- levels(as.factor(data$name)) # For later use, find all the names of
> # the coders.
```

Create a matrix to store the means of each possible pair of coders.

``` r
> same.responses.means <- matrix(NA, length(names),length(names))
> dimnames(same.responses.means) <- list(names,names) # Set the row and column 
> # names of the matrix.
```

Compute the means and store them in the matrix.

``` r
> for (i in 1:length(names)) {
+   for (j in 1:length(names)) {
+     temp.vector <- ifelse(data.w[,i]==data.w[,j],1,0)
+     same.responses.means[i,j] <- mean(temp.vector)}
+ }
```

Remove the superfluous data, and compute the mean and standard deviation.

``` r
> diag(same.responses.means) = NA
> mean(same.responses.means, na.rm = TRUE)
[1] 0.6909341
> sd(same.responses.means, na.rm = TRUE)
[1] 0.09920726
> same.responses.means
              Jakub Maciej G. Maciej T.     Maria     Paweł     Piotr   Tadeusz  Wojciech
Jakub            NA 0.6538462 0.6538462 0.6538462 0.7307692 0.5000000 0.8076923 0.8076923
Maciej G. 0.6538462        NA 0.6923077 0.7692308 0.7692308 0.6153846 0.7692308 0.6923077
Maciej T. 0.6538462 0.6923077        NA 0.6923077 0.6923077 0.4615385 0.6923077 0.7692308
Maria     0.6538462 0.7692308 0.6923077        NA 0.8461538 0.6923077 0.6923077 0.6923077
Paweł     0.7307692 0.7692308 0.6923077 0.8461538        NA 0.6153846 0.6923077 0.8461538
Piotr     0.5000000 0.6153846 0.4615385 0.6923077 0.6153846        NA 0.6153846 0.4615385
Tadeusz   0.8076923 0.7692308 0.6923077 0.6923077 0.6923077 0.6153846        NA 0.7692308
Wojciech  0.8076923 0.6923077 0.7692308 0.6923077 0.8461538 0.4615385 0.7692308        NA
```

Create the contingency table and calculate the chi-squared statistics.

``` r
> tab.tmp <- table(data.c$name, data.c$response)
> chi.tmp <- chisq.test(tab.tmp)
> chi.tmp

	Pearson's Chi-squared test

data:  tab.tmp
X-squared = 15.816, df = 7, p-value = 0.02685
```

The null hypothesis of homogeneity is rejected, with a p-value < 0.05.

The expected counts in the cells of the table are:

``` r
> chi.tmp$expected
           
                 0     1
  Jakub     16.375 9.625
  Maciej G. 16.375 9.625
  Maciej T. 16.375 9.625
  Maria     16.375 9.625
  Paweł     16.375 9.625
  Piotr     16.375 9.625
  Tadeusz   16.375 9.625
  Wojciech  16.375 9.625
```

Check the residuals: Looking at residuals with largest absolute values gives an idea which cells of the table contribute most to the result.

``` r
> chi.tmp$residuals
           
                      0           1
  Jakub      0.64869217 -0.84611411
  Maciej G. -0.09267031  0.12087344
  Maciej T.  1.39005464 -1.81310167
  Maria     -0.58691196  0.76553182
  Paweł     -0.58691196  0.76553182
  Piotr     -1.57539526  2.05484856
  Tadeusz    0.40157134 -0.52378493
  Wojciech   0.40157134 -0.52378493
```

The largest discrepancies between observed and expected counts are related to Piotr's and Maciej T.'s answers.

## Revised Sample of 20 Scenarios

## Categorization (20)

TODO: Justification for removing some data.

``` r
> data$scenario[which(data$PD == 0)]
[1] "Exile and the Kingdom"     "(Textor, 2007, p. 955)"    "The Adventures of Sindbad" "The Loser"                
[5] "Ulysses"                   "A Spy"       
```

Remove the specific data.

``` r
> data.20 <- data[which(data$PD != 0),]
> levels(as.factor(data.20$scenario))
 [1] "(Ciecierski, Makowski, 2020)"  "(de Gaynesford, 2006, p. 169)" "(Gauker, 2008, p. 363)"       
 [4] "(Kaplan, 1978, p. 239)"        "(King, 1999, p. 156)"          "(King, 2014, p. 224)"         
 [7] "(McGinn, 1981, p. 162)"        "(McGinn, 1981, pp. 161–162)"   "(Perry, 2009, p. 193)"        
[10] "(Perry, 2017, p. 979)"         "(Radulescu, 2019, p. 15)"      "(Radulescu, 2019, p. 7)"      
[13] "(Reimer, 1991a, p. 194)"       "(Reimer, 1991a, pp. 190–191)"  "(Reimer, 1991b, p. 180)"      
[16] "(Reimer, 1991b, p. 182)"       "(Siegel, 2002, pp. 10–11)"     "(Ullman et al., 2012, p. 457)"
[19] "A Pianist at the Party"        "Dubliners"
```

Define some "discrimination thresholds", or simply "thresholds", for later use as the legend keys on the plot.

```r
> data.20$prob.thresholds <- as.factor(ifelse(data.20$prob.PD < 0.5, "0",
+                                       ifelse(data.20$prob.PD == 0.5, "0.5", "1")
+ ))
```

Specify the order in the dataset: increasing PD.

``` r
> data.20.sort <- data.20[with(data.20, order(data.20$prob.PD, scenario)),] 
```

### Plot (20)

![Categorization output (20)](https://github.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/blob/main/Output/Plot%20(20).png)
TODO: Description.

## Analysis of the Contingency Table (20)

Identify the scenarios that were categorized as ND as often as they were as PD.

``` r
> data.20$scenario[which(data.20$prob.thresholds == 0.5)]
[1] "(Reimer, 1991b, p. 180)" "(Perry, 2009, p. 193)"  
> # One of them is (Reimer, 1991b, p. 180), the other: (Perry, 2009, p. 193).
> Reimer <- which(data.20$scenario == "(Reimer, 1991b, p. 180)")
> Perry <- which(data.20$scenario == "(Perry, 2009, p. 193)")
```

Dataset `data.20` already contains the four possible categorizations: i) both scenarios fall into ND, ii) Reimer's into ND, Perry's into PD, iii) vice versa, iv) both into PD.

``` r
> data.20[Reimer,c(1,8:11)]
                  scenario prob.thresholds1 prob.thresholds2 prob.thresholds3 prob.thresholds4
14 (Reimer, 1991b, p. 180)                0                0                1                1
> data.20[Perry, c(1,8:11)]
                scenario prob.thresholds1 prob.thresholds2 prob.thresholds3 prob.thresholds4
26 (Perry, 2009, p. 193)                0                1                0                1
```

Perform chi-square and Fisher's tests.

``` r
> for (i in 1:4){
+   col.tmp <- paste0("prob.thresholds", i)
+   data.20[, col.tmp] <- droplevels(data.20[, col.tmp]) # Clean the variables
+   
+   dat.tmp <- data.20[,c(3,7+i)]
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

Chi-square test output.

``` r
> chi.values %>% round(5)
  Chisq1[p-value1] Chisq2[p-value2] Chisq3[p-value3] Chisq4[p-value4]
1          0.00277          0.00623          0.00623          0.00277
> chi.tmp$expected
   prob.thresholds4
PD     0    1
  1 1.80 2.20
  2 1.80 2.20
  3 0.45 0.55
  4 0.90 1.10
  5 1.35 1.65
  6 1.35 1.65
  7 1.35 1.65
```

R printed out the warning messages - many of the expected cells are below 5. Use Fisher's exact test instead.

``` r
> fisher.values
  FET1[p-value1] FET2[p-value2] FET3[p-value3] FET4[p-value4]
1   6.549178e-05   9.742579e-05   9.742579e-05   6.549178e-05
```

The null hypothesis of independence is strongly rejected, with a p-value near 0.

## Similarity Between Coders - or Lack Thereof (20)

Prepare "data - Probability per Coder" dataset in its "wide" form anew.

``` r
> data.w <- data.c %>%
+   select(scenario, name, response) %>%
+   spread(name, response)
```

Find data for which `PD` = 0 and remove them.

``` r
> data.w$PD <- rowSums(data.w[,2:9])
> data.w$scenario[which(data.w$PD == 0)]
[1] "(Textor, 2007, p. 955)"    "A Spy"                     "Exile and the Kingdom"     "The Adventures of Sindbad"
[5] "The Loser"                 "Ulysses"                  
> data.w.20 <- data.w[which(data.w$PD != 0),]
> str(data.w.20)
'data.frame':	20 obs. of  10 variables:
 $ scenario : chr  "(Ciecierski, Makowski, 2020)" "(de Gaynesford, 2006, p. 169)" "(Gauker, 2008, p. 363)" "(Kaplan, 1978, p. 239)" ...
 $ Jakub    : int  0 0 0 0 0 1 1 0 0 0 ...
 $ Maciej G.: int  0 0 1 1 0 1 1 0 1 0 ...
 $ Maciej T.: int  0 0 1 1 0 0 0 0 0 0 ...
 $ Maria    : int  0 1 1 1 0 1 0 0 1 1 ...
 $ Paweł    : int  1 1 1 1 0 1 1 0 0 0 ...
 $ Piotr    : int  0 1 1 1 1 1 1 1 1 1 ...
 $ Tadeusz  : int  0 0 1 0 0 1 1 0 1 0 ...
 $ Wojciech : int  1 0 1 0 0 0 1 0 0 0 ...
 $ PD       : num  2 3 7 5 1 6 6 1 4 2 ...
```

For ease of calculating, remove the first column from the dataset.

``` r
> data.w.20 <- data.w.20[,2:9]
```

Method A) cosine similarity.

``` r
> matrix.tmp <- data.matrix(data.w.20)
> 
> cosine.sim <- cosine(matrix.tmp) # Compute the cosine similarities.
> 
> diag(cosine.sim) = NA # Remove the superfluous data, and compute the mean and 
> # standard deviation.
> mean(cosine.sim, na.rm = TRUE)
[1] 0.5868093
> sd(cosine.sim, na.rm = TRUE)
[1] 0.1388474
> cosine.sim
              Jakub Maciej G. Maciej T.     Maria     Paweł     Piotr   Tadeusz  Wojciech
Jakub            NA 0.4780914 0.1889822 0.5455447 0.6546537 0.4724556 0.6681531 0.6681531
Maciej G. 0.4780914        NA 0.4743416 0.7302967 0.7302967 0.6324555 0.6708204 0.5590170
Maciej T. 0.1889822 0.4743416        NA 0.5773503 0.5773503 0.3750000 0.3535534 0.5303301
Maria     0.5455447 0.7302967 0.5773503        NA 0.8333333 0.7216878 0.6123724 0.6123724
Paweł     0.6546537 0.7302967 0.5773503 0.8333333        NA 0.6495191 0.6123724 0.8164966
Piotr     0.4724556 0.6324555 0.3750000 0.7216878 0.6495191        NA 0.6187184 0.4419417
Tadeusz   0.6681531 0.6708204 0.3535534 0.6123724 0.6123724 0.6187184        NA 0.6250000
Wojciech  0.6681531 0.5590170 0.5303301 0.6123724 0.8164966 0.4419417 0.6250000        NA
```

Method B) probability of giving the same answer.

``` r
> names <- levels(as.factor(data.c$name)) # For later use, find all the names of
> # the coders.
> 
> same.responses.means <- matrix(NA, length(names),length(names)) # Matrix
> dimnames(same.responses.means) <- list(names,names) # Set the row and column 
> # names of the matrix.
> 
> for (i in 1:length(names)) { # Compute the means and store them in the matrix.
+   for (j in 1:length(names)) {
+     temp.vector <- ifelse(data.w.20[,i]==data.w.20[,j],1,0)
+     same.responses.means[i,j] <- mean(temp.vector)}
+ }
> 
> diag(same.responses.means) = NA # Remove the superfluous data, and compute the 
> # mean and standard deviation.
> mean(same.responses.means, na.rm = TRUE)
[1] 0.5982143
> sd(same.responses.means, na.rm = TRUE)
[1] 0.1289694
> same.responses.means
          Jakub Maciej G. Maciej T. Maria Paweł Piotr Tadeusz Wojciech
Jakub        NA      0.55      0.55  0.55  0.65  0.35    0.75     0.75
Maciej G.  0.55        NA      0.60  0.70  0.70  0.50    0.70     0.60
Maciej T.  0.55      0.60        NA  0.60  0.60  0.30    0.60     0.70
Maria      0.55      0.70      0.60    NA  0.80  0.60    0.60     0.60
Paweł      0.65      0.70      0.60  0.80    NA  0.50    0.60     0.80
Piotr      0.35      0.50      0.30  0.60  0.50    NA    0.50     0.30
Tadeusz    0.75      0.70      0.60  0.60  0.60  0.50      NA     0.70
Wojciech   0.75      0.60      0.70  0.60  0.80  0.30    0.70       NA
```

Prepare the data for the chi-squared test.

``` r
> data.tmp <- data.c[which(data.c$scenario != "(Textor, 2007, p. 955)" & 
+              data.c$scenario != "A Spy" &
+              data.c$scenario != "Exile and the Kingdom" &
+              data.c$scenario != "The Adventures of Sindbad" &
+              data.c$scenario != "The Loser" &
+              data.c$scenario != "Ulysses"),]
> 
> tab.tmp <- table(data.tmp$name, data.tmp$response) # Construct the contingency 
> # table
```

Chi-square test.

``` r
> chi.tmp <- chisq.test(tab.tmp)
> chi.tmp

	Pearson's Chi-squared test

data:  tab.tmp
X-squared = 19.202, df = 7, p-value = 0.007578
```

``` r
> chi.tmp$expected
           
                 0     1
  Jakub     10.375 9.625
  Maciej G. 10.375 9.625
  Maciej T. 10.375 9.625
  Maria     10.375 9.625
  Paweł     10.375 9.625
  Piotr     10.375 9.625
  Tadeusz   10.375 9.625
  Wojciech  10.375 9.625

> chi.tmp$residuals
           
                     0          1
  Jakub      0.8149581 -0.8461141
  Maciej G. -0.1164226  0.1208734
  Maciej T.  1.7463387 -1.8131017
  Maria     -0.7373430  0.7655318
  Paweł     -0.7373430  0.7655318
  Piotr     -1.9791838  2.0548486
  Tadeusz    0.5044978 -0.5237849
  Wojciech   0.5044978 -0.5237849
```

## Final Sample: (Textor, 2007, p. 955) in place of "Dubliners"

One last touch: reduce the sample of scenarios to 20 and replace "Dubliners" with (Textor, 2007, p. 955). "Dubliners" received some alarming comments from the coders.

### Categorization and tests

``` r
> data.textor <- data[which(data$scenario != "Dubliners" & 
+                            data$scenario != "A Spy" &
+                            data$scenario != "Exile and the Kingdom" &
+                            data$scenario != "The Adventures of Sindbad" &
+                            data$scenario != "The Loser" &
+                            data$scenario != "Ulysses"),]
> levels(as.factor(data.textor$scenario))
 [1] "(Ciecierski, Makowski, 2020)"  "(de Gaynesford, 2006, p. 169)" "(Gauker, 2008, p. 363)"       
 [4] "(Kaplan, 1978, p. 239)"        "(King, 1999, p. 156)"          "(King, 2014, p. 224)"         
 [7] "(McGinn, 1981, p. 162)"        "(McGinn, 1981, pp. 161–162)"   "(Perry, 2009, p. 193)"        
[10] "(Perry, 2017, p. 979)"         "(Radulescu, 2019, p. 15)"      "(Radulescu, 2019, p. 7)"      
[13] "(Reimer, 1991a, p. 194)"       "(Reimer, 1991a, pp. 190–191)"  "(Reimer, 1991b, p. 180)"      
[16] "(Reimer, 1991b, p. 182)"       "(Siegel, 2002, pp. 10–11)"     "(Textor, 2007, p. 955)"       
[19] "(Ullman et al., 2012, p. 457)" "A Pianist at the Party"
```

Chi-square and Fisher's tests.

``` r
> for (i in 1:4){
+   #col.tmp <- paste0("prob.thresholds", i)
+   #data.20[, col.tmp] <- droplevels(data.20[, col.tmp]) # Clean the variables
+   
+   dat.tmp <- data.textor[,c(3,7+i)]
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

``` r
> chi.values
  Chisq1[p-value1] Chisq2[p-value2] Chisq3[p-value3] Chisq4[p-value4]
1      0.005569683          0.01197          0.01197      0.005569683
> chi.tmp$expected
   prob.thresholds4
PD     0    1
  0 0.45 0.55
  1 1.80 2.20
  2 1.35 1.65
  3 0.45 0.55
  4 0.90 1.10
  5 1.35 1.65
  6 1.35 1.65
  7 1.35 1.65
```

``` r
> fisher.values
  FET1[p-value1] FET2[p-value2] FET3[p-value3] FET4[p-value4]
1   0.0001428912   0.0002381519   0.0002381519   0.0001428912
```

### Similarity between coders - or lack thereof

Prepare the data.

``` r
> data.w <- data.c %>%
+   select(scenario, name, response) %>%
+   spread(name, response)
> 
> data.w.textor <- data.w[which(data.w$scenario != "Dubliners" & 
+                             data.w$scenario != "A Spy" &
+                             data.w$scenario != "Exile and the Kingdom" &
+                             data.w$scenario != "The Adventures of Sindbad" &
+                             data.w$scenario != "The Loser" &
+                             data.w$scenario != "Ulysses"),]
> data.w.textor <- data.w.textor[,2:9] # Remove the first column from the dataset.
```

Method A) cosine similarity.

``` r
> matrix.tmp <- data.matrix(data.w.textor)
> cosine.sim <- cosine(matrix.tmp) # Compute the cosine similarities.
> diag(cosine.sim) = NA # Remove the superfluous data, and compute the mean and 
> # standard deviation.
> mean(cosine.sim, na.rm = TRUE)
[1] 0.5966167
> sd(cosine.sim, na.rm = TRUE)
[1] 0.1410904
> cosine.sim
              Jakub Maciej G. Maciej T.     Maria     Paweł     Piotr   Tadeusz  Wojciech
Jakub            NA 0.5039526 0.1889822 0.5455447 0.6546537 0.4879500 0.6681531 0.6681531
Maciej G. 0.5039526        NA 0.5000000 0.7698004 0.7698004 0.6024641 0.7071068 0.5892557
Maciej T. 0.1889822 0.5000000        NA 0.5773503 0.5773503 0.3872983 0.3535534 0.5303301
Maria     0.5455447 0.7698004 0.5773503        NA 0.8333333 0.7453560 0.6123724 0.6123724
Paweł     0.6546537 0.7698004 0.5773503 0.8333333        NA 0.6708204 0.6123724 0.8164966
Piotr     0.4879500 0.6024641 0.3872983 0.7453560 0.6708204        NA 0.6390097 0.4564355
Tadeusz   0.6681531 0.7071068 0.3535534 0.6123724 0.6123724 0.6390097        NA 0.6250000
Wojciech  0.6681531 0.5892557 0.5303301 0.6123724 0.8164966 0.4564355 0.6250000        NA
```

Method B) probability of giving the same answer.

``` r
> names <- levels(as.factor(data.c$name)) # For later use, find all the names of
> same.responses.means <- matrix(NA, length(names),length(names)) # Matrix
> dimnames(same.responses.means) <- list(names,names) # Set the row and column 
> for (i in 1:length(names)) {
+   for (j in 1:length(names)) {
+     temp.vector <- ifelse(data.w.textor[,i]==data.w.textor[,j],1,0)
+     same.responses.means[i,j] <- mean(temp.vector)}
+ }
> diag(same.responses.means) = NA # Remove the superfluous data, and compute the 
> # mean and standard deviation.
> mean(same.responses.means, na.rm = TRUE)
[1] 0.6196429
> sd(same.responses.means, na.rm = TRUE)
[1] 0.1201055
> same.responses.means
          Jakub Maciej G. Maciej T. Maria Paweł Piotr Tadeusz Wojciech
Jakub        NA      0.60      0.55  0.55  0.65  0.40    0.75     0.75
Maciej G.  0.60        NA      0.65  0.75  0.75  0.50    0.75     0.65
Maciej T.  0.55      0.65        NA  0.60  0.60  0.35    0.60     0.70
Maria      0.55      0.75      0.60    NA  0.80  0.65    0.60     0.60
Paweł      0.65      0.75      0.60  0.80    NA  0.55    0.60     0.80
Piotr      0.40      0.50      0.35  0.65  0.55    NA    0.55     0.35
Tadeusz    0.75      0.75      0.60  0.60  0.60  0.55      NA     0.70
Wojciech   0.75      0.65      0.70  0.60  0.80  0.35    0.70       NA
```

Prepare the data for the chi-squared test and perform the test.

``` r
> data.tmp <- data.c[which(data.c$scenario != "Dubliners" & 
+                            data.c$scenario != "A Spy" &
+                            data.c$scenario != "Exile and the Kingdom" &
+                            data.c$scenario != "The Adventures of Sindbad" &
+                            data.c$scenario != "The Loser" &
+                            data.c$scenario != "Ulysses"),]
> tab.tmp <- table(data.tmp$name, data.tmp$response) # Construct the contingency 
> # Chi-squared test.
> chi.tmp <- chisq.test(tab.tmp)
> chi.tmp

	Pearson's Chi-squared test

data:  tab.tmp
X-squared = 16.841, df = 7, p-value = 0.01845
```

``` r
> chi.tmp$expected
           
                 0     1
  Jakub     10.625 9.375
  Maciej G. 10.625 9.375
  Maciej T. 10.625 9.375
  Maria     10.625 9.375
  Paweł     10.625 9.375
  Piotr     10.625 9.375
  Tadeusz   10.625 9.375
  Wojciech  10.625 9.375
> chi.tmp$residuals
           
                     0          1
  Jakub      0.7286167 -0.7756718
  Maciej G.  0.1150447 -0.1224745
  Maciej T.  1.6489747 -1.7554676
  Maria     -0.8053132  0.8573214
  Paweł     -0.8053132  0.8573214
  Piotr     -1.7256712  1.8371173
  Tadeusz    0.4218307 -0.4490731
  Wojciech   0.4218307 -0.4490731
```

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
