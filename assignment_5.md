Assignment 5: Data import (readr) and tidy data (tidyr)
================

\#**Instructions: Please read through this before you begin**

\*This homework is due by **10pm on Monday 10/26/20**

\*Please **reproduce this markdown template**. Pay attention to all the
formating in this file, including bullet points, bolded characterrs,
inserted code chunks, headings, text colors, blank lines, etc.

\*For the first two excercises in this assignment, **import** data files
into R and **parse** them properly as instructed. **Tidy and/or
**transform\*\* the data frames when appropriate.

  - We have not extensively covered parsing data in lecture, but you can
    find more information about here.

  - Briefly, you can choose to use the `col_types` argument in
    a`read_*()` function to parse the data during import.

  - In many cases, you can also choose to parse the data using one of
    the `parse_*()` functions nested within a `mutate()` after importing
    the data.

  - In many cases, you can choose to parse the data using one of the
    `parse_*()` functions nested within a `mutate()` function after
    importing the data.

  - For the next exercise, you will explore a dataset with details about
    passengers on the Titanic. First, **answer the questions below and
    use figures or tables to support your answer** Then, **explore the
    dataset on your own** using the data transformation and
    visualization skills that you have learned in this so far.
    
      - For this excercise, pleas make sure to put some thought into
        **editing the aesthetics of your figures and tables** to make
        them easier to understand and nicer to look at (e.g. choose the
        most appropriate geometric object, aesthetic mapping, facetting,
        postion adjustment; add meaningful axis lables, figure titles
        legend titile; change the background; be creative; and etc.)

  - Please note that the **excercise 4 is optional**. It is a good
    exercise for practicing your data tidying and exploration skills
    through, so we encourage you to give it a try if time allows.

  - Please **reproduce the tables and plots exactly as shown in this
    markdown file**.

  - When a verbal response is needed, answer by editing the part in the
    R markdown template where it says “Write your response here” .

  - Have all your code embedded within the R markdown file, and show
    both of your **code** and **plots** in the kitted markdown file.

  - Use R Markdown functionalities to **hide message and warnings when
    needed**. (Suggestion: messages and warnings can often be
    informative and important, so please examine them carefylly and only
    turn them off when you finish the exercise).

  - Please name your R markdown file `assignment_5.Rmd` and the knitted
    markdwon file `assignment_5.md`. Please upload both files using your
    personal GitHub repository for this class.

  - To start, first load all the required packages with the following
    code. Install them if they are not installed yet.

<!-- end list -->

``` r
library(tidyverse)
library(knitr)
```

\#\#\*\*Exercise 1. Tibble and Data Import

Import data frames listed below into R and parse the columns
appropraitly when need ed. Watch out for the formating oddities of each
dataset. Print the results with `kable()`.

1.1 **Create the following tibble manually, first using `tribble()` and
then using `tibble()`. Print both results.**

``` r
tribble(~a,~b, ~c,
        1, 2.1, "apple",
        2, 3.2, "orrange"
        ) %>% 
  kable()
```

| a |   b | c       |
| -: | --: | :------ |
| 1 | 2.1 | apple   |
| 2 | 3.2 | orrange |

``` r
tibble(a = c(1,2),
       b = c(2.1, 3.2), 
       c = c("apple", "orrange"))%>% 
  kable()
```

| a |   b | c       |
| -: | --: | :------ |
| 1 | 2.1 | apple   |
| 2 | 3.2 | orrange |

**1.2 import
`https://raw.githubusercontent.com/nt246/NTRES6940-data-science/master/datasets/dataset2.txt`
into R. Change the column names into “Name”, “Weight”, “Price”.**

``` r
data_Fruit<-read.table("https://raw.githubusercontent.com/nt246/NTRES6940-data-science/master/datasets/dataset2.txt", sep = ",") 

colnames(data_Fruit) <- c("Name", "Weight", "Price")
kable(data_Fruit)
```

| Name   | Weight | Price |
| :----- | -----: | ----: |
| apple  |      1 |   2.9 |
| orange |      2 |   4.9 |
| durian |     10 |  19.9 |

**1.3 Import
`https://raw.githubusercontent.com/nt246/NTRES6940-data-science/master/datasets/dataset3.txt`
into R . Watch out for the first few lines, missing values, separators,
quotation marks, and deliminaters.**

``` r
read_table("https://raw.githubusercontent.com/nt246/NTRES6940-data-science/master/datasets/dataset3.txt")#, row.names = 1)# sep = ",")
```

    ## Parsed with column specification:
    ## cols(
    ##   `Table of fruits` = col_character()
    ## )

    ## # A tibble: 5 x 1
    ##   `Table of fruits`       
    ##   <chr>                   
    ## 1 09/25/18                
    ## 2 /Name/;/Weight/;/Price/ 
    ## 3 /apple/;1;2.9           
    ## 4 /orange/;2;Not Available
    ## 5 /durian/;?;19.9

``` r
Fruit<-read_csv("Name, Weight,Price
         apple,1,2.9
         orange, 2, NA
         durian, NA, 19.9")
kable(Fruit)
```

| Name   | Weight | Price |
| :----- | -----: | ----: |
| apple  |      1 |   2.9 |
| orange |      2 |    NA |
| durian |     NA |  19.9 |

**1.4 Import
`https://raw.githubsercontent.com/nt246/NTRES6940-science/master/datasets/dataset4.txt`
into R. Watch out for comments, units, and decimal markes(which are `,`
in this case).**

``` r
#read_csv("https://raw.githubsercontent.com/nt246/NTRES6940-science/master/datasets/dataset4.txt")#, sep =",")
#hacky hour not working
```

**1.5 import
`https://raw.githubusercontent.com/nt246/NTRES6940-data-science/master/datasets/dataset5.txt`
into R. Parse the columns properly. Write this imported and parsed data
frame into a new csv file named `dataset5_new.csv` in your
`problem_sets` folder.**

``` r
Expdate<-read.table("https://raw.githubusercontent.com/nt246/NTRES6940-data-science/master/datasets/dataset5.txt")
colnames(Expdate) <- Expdate[1,]
Expdate<-Expdate[-1,]
kable(Expdate)
```

|   | Name   | Expiration Date    | Time    |
| :- | :----- | :----------------- | :------ |
| 2 | apple  | September 26, 2018 | 1:00am  |
| 3 | orange | October 2, 2018    | 1:00pm  |
| 4 | durian | October 21, 2018   | 11:00am |

``` r
write.csv(Expdate, file = "dataset5_new.csv") #imported dataframe
```
