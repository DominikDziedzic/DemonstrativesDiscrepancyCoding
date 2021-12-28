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
  - [**Categorization (26)**](https://github.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/blob/main/README.md#categorization)
  - [**Analysis of the Contingency Table (26)**](https://github.com/DominikDziedzic/DemonstrativesDiscrepancyCoding#analysis-of-the-contingency-table)
  - [**Similarity Between Coders (or Lack Thereof) (26)**](https://github.com/DominikDziedzic/DemonstrativesDiscrepancyCoding#similarity-between-coders-or-lack-thereof)
- **Revised Sample of 20 Scenarios:**
  - [**Categorization (26)**](https://github.com/DominikDziedzic/DemonstrativesDiscrepancyCoding/blob/main/README.md#categorization)
  - [**Analysis of the Contingency Table (26)**](https://github.com/DominikDziedzic/DemonstrativesDiscrepancyCoding#analysis-of-the-contingency-table)
  - [**Similarity Between Coders (or Lack Thereof) (26)**](https://github.com/DominikDziedzic/DemonstrativesDiscrepancyCoding#similarity-between-coders-or-lack-thereof)
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

TODO

## Analysis of the Contingency Table (26)

TODO (mention: two categorical variables, justification of choosing the tests)

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
