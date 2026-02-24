# Data Wrangling and Tidy Data Project
Reece Sinn Frantz
2026-02-23

## Introduction

To show the importance of the principles of tidy data, I began by
combing through a deliberately frustrating “messy” data set. We often
encounter data where variables are trapped in column headers or
observations are scattered. This project demonstrates the transformation
of a raw biological dataset into a tidy format, making it ready for
visualisation.

### Data

The dataset used in this study consists of morphological measurements of
*Macrocystis maxima* (Giant Kelp) collected from two sites in the
Western Cape: **Boulders Beach** and **Batsata Rock**. The variables
include stipe length, diameter, frond mass, and epiphyte length. This
data is considered “tidy” because each row represents a single kelp
individual, and each column represents a specific measurement.

### Analysis

The R script below explores the morphological differences between the
two sites using the tidyverse suite. All analyses were done in R
\[@R2025\].

<details class="code-fold">
<summary>Code</summary>

``` r
#```{r}
#| label: fig-kelp-plots
#| message: false
#| warning: false

# Loading the R packages
library(dplyr)    # To help us manipulate data
```

</details>


    Attaching package: 'dplyr'

    The following objects are masked from 'package:stats':

        filter, lag

    The following objects are masked from 'package:base':

        intersect, setdiff, setequal, union

<details class="code-fold">
<summary>Code</summary>

``` r
library(tidyr)    # To keep everything tidy
library(readr)    # To read our kelp file
library(ggplot2)  # To make the professional pink/purple plots

# STEP 1: Load the kelp data
# R will look inside your 'data' folder for this file
kelp_data <- read_csv("data/kelp_data.csv")
```

</details>

    Rows: 26 Columns: 12

    ── Column specification ────────────────────────────────────────────────────────
    Delimiter: ","
    chr  (2): species, site
    dbl (10): ID, stipe_length, stipe_diameter, frond_length, digits, primary_bl...

    ℹ Use `spec()` to retrieve the full column specification for this data.
    ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

<details class="code-fold">
<summary>Code</summary>

``` r
# STEP 2: Quick Clean
# We make sure the 'site' column is treated as a category (factor)
kelp_tidy <- kelp_data %>%
  mutate(site = as.factor(site))

# STEP 3: Plot 1 - The Boxplot (Aesthetic Professional)
# Comparing Stipe Length between sites
ggplot(kelp_tidy, aes(x = site, y = stipe_length, fill = site)) +
  # Adding a soft boxplot
  geom_boxplot(alpha = 0.7, outlier.shape = NA) +
  # Adding 'jitter' so we can see the individual data points
  geom_jitter(width = 0.1, color = "black", alpha = 0.3) +
  # Setting our Pink and Purple colors
  scale_fill_manual(values = c("Boulders Beach" = "#FF69B4", "Batsata Rock" = "#9370DB")) +
  theme_minimal() +
  labs(
    title = "Stipe Length: Boulders Beach vs. Batsata Rock",
    subtitle = "A comparison of kelp morphology",
    x = "Site Location",
    y = "Stipe Length (mm)"
  ) +
  theme(legend.position = "none")
```

</details>

![](README_files/figure-commonmark/unnamed-chunk-1-1.png)

<details class="code-fold">
<summary>Code</summary>

``` r
# STEP 4: Plot 2 - The Relationship (Aesthetic Professional)
# Checking how Stipe Mass affects Frond Mass
ggplot(kelp_tidy, aes(x = stipe_mass, y = frond_mass, color = site)) +
  geom_point(size = 3, alpha = 0.8) +
  # Adding a clean trend line
  geom_smooth(method = "lm", se = FALSE) +
  # Setting our Dark Pink and Deep Purple colors
  scale_color_manual(values = c("Boulders Beach" = "#FF1493", "Batsata Rock" = "#8A2BE2")) +
  theme_minimal() +
  labs(
    title = "Biomass Relationship",
    x = "Stipe Mass (g)",
    y = "Frond Mass (g)"
  )
```

</details>

    `geom_smooth()` using formula = 'y ~ x'

![](README_files/figure-commonmark/unnamed-chunk-1-2.png)

## Results

The results show that morphological traits differ between the two
populations, with Batsata Rock specimens generally showing higher
measurements.

## Discussion

Applying tidy data principles to this kelp data set allowed for a
reproducible and professional analysis.
