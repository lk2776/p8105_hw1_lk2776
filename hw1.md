---
title: "HW1"
output: github_document
date: "2024-09-21"
---

### Problem 1
Write a short description of the penguins dataset (not the penguins_raw dataset) using inline R code. In your discussion, please
include:
the data in this dataset, including names / values of important variables
the size of the dataset (using nrow and ncol )
the mean flipper length
Make a scatterplot of flipper_length_mm (y) vs bill_length_mm (x); color points using the species variable (adding color = ... inside of
aes in your ggplot code should help).
Export your first scatterplot to your project directory using ggsave.


```{r}
library("tidyverse")
library("palmerpenguins")

data("penguins", package = "palmerpenguins")
p1_df_a <- tibble(penguins)
p1_df_b <- drop_na(p1_df_a)
```

### Description 
Rows with NA in Penguins dataset are removed. 

The number of rows and columns in the Penguins dataset are : **`r dim(p1_df_b)`**

Column names of the dataset are: **`r colnames(p1_df_b)`**

Types of penguin species of the dataset are: **`r (levels(p1_df_b$species))`**

Types of islands of the dataset are: **`r (levels(p1_df_b$island))`**

Levels of Sex of the dataset are: **`r (levels(p1_df_b$sex))`**

Years listed in the dataset are: **`r (levels(as.factor(p1_df_b$year)))`**

Mean of flipper length in mm is: **`r round(mean(pull(p1_df_b, flipper_length_mm)), digits=2)`**

Mean of bill length in mm is: **`r round(mean(pull(p1_df_b, bill_length_mm)), digits=2)`**

Mean of bill depth in mm is: **`r round(mean(pull(p1_df_b, bill_depth_mm)), digits=2)`**

Mean of body mass in g is: **`r round(mean(pull(p1_df_b, body_mass_g)), digits=2)`**

### Plot

```{r plot}
ggplot(p1_df_b, aes(x = bill_length_mm, y = flipper_length_mm, color=species)) + geom_point()
ggsave("plot_hw1_p1.pdf", height = 4, width = 6)

```

### Problem 2:
Create a data frame comprised of:
a random sample of size 10 from a standard Normal distribution.
a logical vector indicating whether elements of the sample are greater than 0
a character vector of length 10
a factor vector of length 10, with 3 different factor “levels”

Try to take the mean of each variable in your dataframe. What works and what doesn’t?

Hint: to take the mean of a variable in a dataframe, you need to pull the variable out of the dataframe. Try loading the tidyverse and using the
pull function.

In some cases, you can explicitly convert variables from one type to another. Write a code chunk that applies the as.numeric function to the logical, character, and factor variables (please show this chunk but not the output). What happens, and why? Does this help explain what happens when you try to take the mean?

```{r setup, eval='FALSE'}
library(tidyverse)
```

```{r problem2 create df}
set.seed(1)

p2_df <- tibble(
  norm_samp = rnorm(10, mean = 0, sd = 1),
  log_vec = norm_samp > 0,
  char_vec = c("Alpha", "Bravo", "Charlie", "Delta", "Echo",
                "Foxtrot","Golf","Hotel","India","Juliet"),
  factor_vec = factor(c("He","She","They","She","They",
                         "They","He","She","She","He"))
)

p2_df

```
### Checking the class of the variables:

The variable `norm_samp` has class `r class(pull(p2_df, norm_samp))`

The variable `log_vec` has class `r class(pull(p2_df, log_vec))`

The variable `char_vec` has class `r class(pull(p2_df, char_vec))`

The variable `factor_vec` has class `r class(pull(p2_df, factor_vec))`

### Find the mean of the variables:

The variable `norm_samp` has mean `r mean(pull(p2_df, norm_samp))`

The variable `log_vec` has mean `r mean(pull(p2_df, log_vec))`

The variable `char_vec` has mean `r mean(pull(p2_df, char_vec))`

The variable `factor_vec` has mean `r mean(pull(p2_df, factor_vec))`


The mean function works for numeric and logical classes, but not for character and factor classes. 

### Convert logical, character and factor classes to numeric class. 

```{r as_numeric_test, eval='FALSE'}
new_log <- as.numeric(p2_df$log_vec)
new_char <- as.numeric(p2_df$char_vec)
new_factor <- as.numeric(p2_df$factor_vec)

```
Logical vector converted from "TRUE" and "FALSE"  to numeric vector "1" and "0". Therefore, mean function works on a new logical vector. 

Character vector failed to convert to numeric, instead NAs are generated. Therefore, mean function does not work on a new character vector. 

Factor vector converted from "He", "She", "They" to "1", "2", "3". Therefore, mean function works on a new factor vector. 

