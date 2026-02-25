# Data Wrangling and Tidy Data Project
Reece Sinn Frantz
2026-02-23

## 1. Introduction

To show the importance of the principles of tidy data, I began by
combing through a deliberately frustrating “messy” data set. We often
encounter data where variables are trapped in column headers or
observations are scattered. This project demonstrates the transformation
of a raw biological dataset into a tidy format, making it ready for
visualisation.

## 2. Data

The dataset used in this study consists of morphological measurements
of *Macrocystis maxima* (Giant Kelp) collected from two sites in the
Western Cape: Boulders Beach and Batsata Rock. The variables include
stipe length, diameter, frond mass, and epiphyte length. This data is
considered “tidy” because each row represents a single kelp individual,
and each column represents a specific measurement.

## 3. Analysis

The R script below explores the morphological differences between the
two sites using the tidyverse suite. All analyses were done in R.

``` r
# Loading the R packages
library(dplyr)    # To help us manipulate data
library(tidyr)    # To keep everything tidy
library(readr)    # To read our kelp file
library(ggplot2)  # To make the professional pink/purple plots

# STEP 1: Load the kelp data
# R will look inside your 'data' folder for this file
kelp_data <- read_csv("data/kelp_data.csv")

# STEP 2: Quick Clean
# We make sure the 'site' column is treated as a category (factor)
kelp_tidy <- kelp_data %>%
  mutate(site = as.factor(site))

# STEP 3: Plot 1 - The Boxplot 
# Comparing Stipe Length between sites
p1 <- ggplot(kelp_tidy, aes(x = site, y = stipe_length, fill = site)) +
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

# Show the plot in the report
p1

# Saving Plot 1 as a physical file
ggsave("plot1.png", p1, width = 8, height = 6)


# STEP 4: Plot 2 - The Relationship 
# Checking how Stipe Mass affects Frond Mass
p2 <- ggplot(kelp_tidy, aes(x = stipe_mass, y = frond_mass, color = site)) +
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

# Show the plot in the report
p2

# Saving Plot 2 as a physical file
ggsave("plot2.png", p2, width = 8, height = 6)
```

<div id="fig-kelp-plots-1">

<img src="README_files/figure-commonmark/fig-kelp-plots-1.png"
id="fig-kelp-plots-1" />

Figure 1

</div>

<div id="fig-kelp-plots-2">

<img src="README_files/figure-commonmark/fig-kelp-plots-2.png"
id="fig-kelp-plots-2" />

Figure 2

</div>

## 4. Results

The results show that morphological traits differ between the two
populations, with Batsata Rock specimens generally showing higher
measurements.

## 5. Discussion

Applying tidy data principles to this kelp data set allowed for a
reproducible and professional analysis.
