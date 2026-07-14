Impulsive Online Fashion Purchase Study
================
Tasnim Talha

# Introduction

This research investigates the drivers of impulsive online fashion
buying among Generation Z consumers in Dhaka, Bangladesh, by examining
the relationship between external social pressures and internal
psychological states. The study finds that while peer influence and
social status create background pressure, personality traits serve as
the primary internal drivers that shape an individual’s behavioral
traits, such as the tendency to act without forethought or seek
emotional gratification.

Quantitative analysis of **160 participants** revealed that all specific
personality indices, peer influence, and perceived social status were
the only significant predictors of impulsive buying. Interestingly, a
significant difference in impulsive behavior was found between genders,
with female respondents in this sample exhibiting higher impulsivity
levels than males. Furthermore, although higher impulsivity is
significantly associated with post-purchase regret, many consumers
engage in rationalized impulsivity—cognitively framing unplanned
purchases as necessary or personally meaningful to mitigate negative
emotions. The report concludes that impulsive fashion consumption in
Dhaka is a cyclical process of mood regulation, and it recommends that
marketers focus on creating mood-lifting content and providing
post-purchase validation to neutralize consumer guilt.

## Aims & Objectives

### **Aim**

To critically investigate the determinants of impulsive online fashion
buying behavior among Generation Z consumers in Dhaka by analyzing the
collective influence of external social pressures (peer influence and
perceived social status) and internal personality traits, while
examining how demographic factors and shopping platforms moderate these
behaviors.

### **Primary Objectives**

1.  To examine the impact of peer influence and perceived social status
    on the frequency of impulsive online fashion purchases.
2.  To evaluate the role of specific personality traits (such as high
    impulsivity and hedonic motives) in driving unplanned fashion buying
    behavior.

### **Secondary Objectives**

3.  To analyze the inter-relationship between peer influence and social
    status, specifically determining if peer pressure intensifies the
    need for status signaling.
4.  To assess the variations in impulsive buying behavior based on
    situational factors, including choice of social commerce platform
    and preferred payment methods.
5.  To compare impulsive buying tendencies across different demographic
    groups, focusing on gender differences and the impact of monthly
    financial allowance.
6.  To determine the relationship between high impulsivity and the
    psychological outcome of post-purchase regret.

## Setting up the Environment & Importing the Dataset

First, we import the necessary libraries for **Exploratory Data Analysis
(EDA)** and **hypothesis testing**.

``` r
# importing the necessary libraries
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.2.1     ✔ readr     2.2.0
    ## ✔ forcats   1.0.1     ✔ stringr   1.6.0
    ## ✔ ggplot2   4.0.2     ✔ tibble    3.3.1
    ## ✔ lubridate 1.9.5     ✔ tidyr     1.3.2
    ## ✔ purrr     1.2.1     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(readxl)
library(psych)
```

    ## 
    ## Attaching package: 'psych'
    ## 
    ## The following objects are masked from 'package:ggplot2':
    ## 
    ##     %+%, alpha

``` r
library(car)
```

    ## Loading required package: carData
    ## 
    ## Attaching package: 'car'
    ## 
    ## The following object is masked from 'package:psych':
    ## 
    ##     logit
    ## 
    ## The following object is masked from 'package:dplyr':
    ## 
    ##     recode
    ## 
    ## The following object is masked from 'package:purrr':
    ## 
    ##     some

``` r
# printing a confirmation message
message("The libraries have been imported successfully.")
```

    ## The libraries have been imported successfully.

Now, we import the dataframe and take a look at the first few rows of
the **raw data**.

``` r
# importing the raw dataframe and inspecting the first few rows
df_raw <- read_excel(path = "../data/survey_response.xlsx", sheet = "raw_data")
head(df_raw)
```

    ## # A tibble: 6 × 27
    ##   Timestamp           `What is your age range?` `What is your gender?`
    ##   <dttm>              <chr>                     <chr>                 
    ## 1 2026-01-11 01:53:53 23–27                     Female                
    ## 2 2026-01-11 08:39:49 23–27                     Male                  
    ## 3 2026-01-11 09:33:21 23–27                     Female                
    ## 4 2026-01-11 09:54:58 23–27                     Male                  
    ## 5 2026-01-11 11:41:22 23–27                     Male                  
    ## 6 2026-01-11 13:34:23 23–27                     Female                
    ## # ℹ 24 more variables: `What is your personal monthly allowance/income?` <chr>,
    ## #   `What is your preferred method of payment for any sort of purchase—be it fashion item or non-fashion item?` <chr>,
    ## #   `How frequently do you buy fashion items online?` <chr>,
    ## #   `Which platform do you use the most for online fashion shopping?` <chr>,
    ## #   `I discuss fashion purchases with friends or family before buying.` <dbl>,
    ## #   `Seeing my friends post about fashion brands increases my urge to buy.` <dbl>,
    ## #   `Seeing influencers cover certain fashion brands increases my urge to buy that item.` <dbl>, …

The raw dataframe has longer questions and have some redundant columns
that will be unnecessary for our analysis. So, we load the **clean
dataframe** sheet which replaces all the longer questions with q1, q2,…
and so on. The clean dataframe sheet also drops the redundant columns as
well. Finally, the last three questions—q23, q24, and q25—are
numerically coded so that they can be inserted into the models.

``` r
# importing the clean dataframe and inspecting the first few rows
df <- read_excel(path = "../data/survey_response.xlsx", sheet = "clean_data")
head(df)
```

    ## # A tibble: 6 × 26
    ##   Timestamp           q1    q2     q3        q4    q5    q6       q7    q8    q9
    ##   <dttm>              <chr> <chr>  <chr>     <chr> <chr> <chr> <dbl> <dbl> <dbl>
    ## 1 2026-01-11 01:53:53 23–27 Female 5,000–10… Mobi… 2–3 … Inst…     4     4     5
    ## 2 2026-01-11 08:39:49 23–27 Male   Below 5,… Cash… Once… Inst…     2     1     1
    ## 3 2026-01-11 09:33:21 23–27 Female Above 20… Mobi… Rare… Bran…     4     3     2
    ## 4 2026-01-11 09:54:58 23–27 Male   5,000–10… Cash… Rare… Face…     3     1     1
    ## 5 2026-01-11 11:41:22 23–27 Male   Above 20… Cash… Once… Face…     3     4     4
    ## 6 2026-01-11 13:34:23 23–27 Female Below 5,… Cash… Once… Bran…     4     2     2
    ## # ℹ 16 more variables: q10 <dbl>, q11 <dbl>, q12 <dbl>, q13 <dbl>, q14 <dbl>,
    ## #   q15 <dbl>, q16 <dbl>, q17 <dbl>, q18 <dbl>, q19 <dbl>, q20 <dbl>,
    ## #   q21 <dbl>, q22 <dbl>, q23 <dbl>, q24 <dbl>, q25 <dbl>

# Data Preparation

## Factorizing the Appropriate Columns

First, there are some columns where order matters. For those column, we
convert them into factors.

``` r
df <- df |>
  mutate(
    across(c(q2, q4, q6), as.factor),
    q1 = factor(x = q1, levels = c("18–22", "23–27", "28 and above"), ordered = TRUE),
    q3 = factor(x = q3, levels = c("Below 5,000", "5,000–10,000", "10,000–15,000", "15,000–20,000", "Above 20,000"), ordered = TRUE),
    q5 = factor(x = q5, levels = c("Rarely", "Once a couple months", "Once a month", "2–3 times a month", "Once a week or more"), ordered = TRUE),
    q23_label = factor(x = q23, levels = c(1:5), labels = c("Never", "Rarely", "Sometimes", "Often", "Very Often"), ordered = TRUE),
    q24_label = factor(x = q24, levels = c(1, 3, 5), labels = c("No, never", "Yes, a few times", "Yes, many times"), ordered = TRUE),
    q25_label = factor(x = q25, levels = c(1, 3, 5), labels = c("No", "Maybe", "Yes"), ordered = TRUE)
  )
```

We create some **composite indices** of the rows in our dataframe across
the **three** independent variables—peer influence, perceived social
status, and personality traits—and **one** dependent variable of the
study.

## Creating Composite Indices

``` r
# creating composite indices
df <- df %>%
  mutate(peer_influence_index = rowMeans(across(q7:q11), na.rm = TRUE)) %>% 
  mutate(social_status_index = rowMeans(across(q12:q15), na.rm = TRUE)) %>% 
  mutate(personality_index = rowMeans(across(q16:q22), na.rm = TRUE)) %>% 
  mutate(dependent_variable_index = rowMeans(across(q23:q24), na.rm = TRUE))

# displaying the composite indices column
head(
  df %>% 
    select(peer_influence_index, social_status_index, personality_index, dependent_variable_index)
)
```

    ## # A tibble: 6 × 4
    ##   peer_influence_index social_status_index personality_index
    ##                  <dbl>               <dbl>             <dbl>
    ## 1                  4.2                4                 3.57
    ## 2                  1.2                1.75              1.14
    ## 3                  2.2                2.75              2.14
    ## 4                  1.8                1.75              2.57
    ## 5                  3.6                3.5               3.14
    ## 6                  3.4                3                 3.29
    ## # ℹ 1 more variable: dependent_variable_index <dbl>

## Calculating the Cronbach’s Alphas

Finally, we calculate the **Cronbach’s alpha** to assess the internal
consistency of the multi-item constructs. According to google,
***Cronbach’s alpha** (𝛼) is a coefficient of internal consistency or
reliability used to measure how closely related a set of items (e.g.,
Likert scale questions) are as a group*.

``` r
# for Peer Influence
alpha_peer_influence <- alpha(df[, c("q7","q8","q9","q10","q11")])

# for Perceived Social Status
alpha_social_status <- alpha(df[, c("q12","q13","q14","q15")])

# for Personality Traits
alpha_personality_traits <- alpha(df[, c("q16","q17","q18","q19","q20", "q21", "q22")])

# displaying the results
alpha_peer_influence
```

    ## 
    ## Reliability analysis   
    ## Call: alpha(x = df[, c("q7", "q8", "q9", "q10", "q11")])
    ## 
    ##   raw_alpha std.alpha G6(smc) average_r S/N   ase mean   sd median_r
    ##       0.65      0.65    0.62      0.27 1.8 0.044    3 0.84     0.28
    ## 
    ##     95% confidence boundaries 
    ##          lower alpha upper
    ## Feldt     0.55  0.65  0.73
    ## Duhachek  0.56  0.65  0.73
    ## 
    ##  Reliability if an item is dropped:
    ##     raw_alpha std.alpha G6(smc) average_r S/N alpha se  var.r med.r
    ## q7       0.66      0.66    0.61      0.33 2.0    0.044 0.0070  0.29
    ## q8       0.54      0.54    0.49      0.23 1.2    0.059 0.0096  0.28
    ## q9       0.59      0.59    0.53      0.26 1.4    0.053 0.0080  0.29
    ## q10      0.56      0.56    0.52      0.24 1.3    0.057 0.0214  0.23
    ## q11      0.61      0.61    0.58      0.28 1.6    0.050 0.0205  0.30
    ## 
    ##  Item statistics 
    ##       n raw.r std.r r.cor r.drop mean  sd
    ## q7  160  0.53  0.53  0.32   0.25  3.4 1.3
    ## q8  160  0.72  0.72  0.64   0.50  3.0 1.3
    ## q9  160  0.64  0.66  0.55   0.42  2.5 1.2
    ## q10 160  0.71  0.70  0.58   0.48  3.1 1.4
    ## q11 160  0.62  0.62  0.44   0.36  2.8 1.4
    ## 
    ## Non missing response frequency for each item
    ##        1    2    3    4    5 miss
    ## q7  0.12 0.14 0.21 0.29 0.23    0
    ## q8  0.17 0.24 0.17 0.29 0.14    0
    ## q9  0.24 0.28 0.26 0.16 0.06    0
    ## q10 0.19 0.14 0.19 0.31 0.16    0
    ## q11 0.24 0.21 0.16 0.28 0.11    0

``` r
alpha_social_status
```

    ## 
    ## Reliability analysis   
    ## Call: alpha(x = df[, c("q12", "q13", "q14", "q15")])
    ## 
    ##   raw_alpha std.alpha G6(smc) average_r S/N   ase mean   sd median_r
    ##       0.69      0.69    0.65      0.35 2.2 0.037  2.8 0.86     0.33
    ## 
    ##     95% confidence boundaries 
    ##          lower alpha upper
    ## Feldt     0.61  0.69  0.77
    ## Duhachek  0.62  0.69  0.77
    ## 
    ##  Reliability if an item is dropped:
    ##     raw_alpha std.alpha G6(smc) average_r S/N alpha se  var.r med.r
    ## q12      0.59      0.58    0.52      0.31 1.4    0.052 0.0396  0.25
    ## q13      0.59      0.58    0.52      0.32 1.4    0.052 0.0334  0.28
    ## q14      0.57      0.57    0.48      0.31 1.3    0.056 0.0053  0.28
    ## q15      0.73      0.73    0.66      0.48 2.8    0.037 0.0063  0.51
    ## 
    ##  Item statistics 
    ##       n raw.r std.r r.cor r.drop mean   sd
    ## q12 160  0.78  0.76  0.64   0.54  2.8 1.32
    ## q13 160  0.78  0.76  0.65   0.53  2.2 1.31
    ## q14 160  0.78  0.77  0.69   0.58  2.0 1.16
    ## q15 160  0.52  0.59  0.34   0.29  4.2 0.93
    ## 
    ## Non missing response frequency for each item
    ##        1    2    3    4    5 miss
    ## q12 0.20 0.24 0.21 0.23 0.12    0
    ## q13 0.46 0.21 0.12 0.16 0.06    0
    ## q14 0.50 0.21 0.15 0.12 0.03    0
    ## q15 0.01 0.05 0.12 0.36 0.45    0

``` r
alpha_personality_traits
```

    ## 
    ## Reliability analysis   
    ## Call: alpha(x = df[, c("q16", "q17", "q18", "q19", "q20", "q21", "q22")])
    ## 
    ##   raw_alpha std.alpha G6(smc) average_r S/N   ase mean  sd median_r
    ##        0.8      0.79    0.79      0.35 3.8 0.024  2.8 0.9     0.35
    ## 
    ##     95% confidence boundaries 
    ##          lower alpha upper
    ## Feldt     0.74   0.8  0.84
    ## Duhachek  0.75   0.8  0.84
    ## 
    ##  Reliability if an item is dropped:
    ##     raw_alpha std.alpha G6(smc) average_r S/N alpha se var.r med.r
    ## q16      0.76      0.76    0.76      0.34 3.1    0.028 0.023  0.30
    ## q17      0.77      0.77    0.76      0.35 3.3    0.028 0.019  0.33
    ## q18      0.75      0.74    0.73      0.32 2.9    0.031 0.013  0.33
    ## q19      0.76      0.75    0.74      0.34 3.1    0.030 0.012  0.33
    ## q20      0.75      0.75    0.74      0.33 2.9    0.030 0.014  0.33
    ## q21      0.79      0.79    0.78      0.39 3.8    0.025 0.018  0.41
    ## q22      0.80      0.79    0.78      0.39 3.9    0.024 0.017  0.41
    ## 
    ##  Item statistics 
    ##       n raw.r std.r r.cor r.drop mean  sd
    ## q16 160  0.69  0.70  0.62   0.56  3.1 1.3
    ## q17 160  0.66  0.66  0.58   0.52  2.6 1.3
    ## q18 160  0.77  0.76  0.73   0.64  3.1 1.5
    ## q19 160  0.73  0.71  0.68   0.60  3.3 1.4
    ## q20 160  0.75  0.74  0.70   0.62  3.0 1.4
    ## q21 160  0.53  0.56  0.43   0.38  1.8 1.2
    ## q22 160  0.53  0.54  0.42   0.36  2.4 1.3
    ## 
    ## Non missing response frequency for each item
    ##        1    2    3    4    5 miss
    ## q16 0.17 0.13 0.26 0.26 0.17    0
    ## q17 0.24 0.28 0.19 0.21 0.08    0
    ## q18 0.23 0.15 0.17 0.22 0.22    0
    ## q19 0.19 0.09 0.21 0.30 0.21    0
    ## q20 0.22 0.16 0.16 0.29 0.17    0
    ## q21 0.59 0.22 0.07 0.07 0.05    0
    ## q22 0.32 0.26 0.18 0.16 0.09    0

Now, we can enter the **EDA** phase of the project.

# Exploratory Data Analysis (EDA)

Now, we do some exploratory data analysis on the dataset to understand
the relationships between the features and the target variable. We will
use various visualization techniques such as histograms, box plots,
scatter plots, and correlation matrices to analyze the data.

## Demographics & Basic Behavior

### Age & Gender Distribution

``` r
# creating a stacked bar chart to observe the age vs gender distribution
ggplot(data = df, aes(x = q1, fill = q2)) +
  geom_bar(position = "stack") +
  labs(
    title = "Age & Gender Distribution",
    x = "Age Range",
    y = "Count",
    fill = ""
  ) + 
  theme_classic() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold", size = 14),
    axis.title.x = element_text(face = "bold"),
    axis.title.y = element_text(face = "bold"),
    legend.position = "top"
  )
```

![](impulse_buy_files/figure-gfm/age-gender-distribution-1.png)<!-- -->

**Key Insights**

- The sample skews female (97 vs 63) and toward the 23–27 band (99 vs 61
  in 18–22).

- The imbalance is concentrated in 23–27, where females (67) more than
  double males (32); the 18–22 band is nearly even (30 vs 31).

- The **28 and above** group is totally absent (0 respondent), so age is
  really a two-category variable in practice.

### Income vs. Online Shopping Frequency

``` r
# creating a stacked bar chart to observe the income vs online shopping frequency
ggplot(data = df, aes(x = q3, fill = q5)) +
  geom_bar(position = "fill") +
  scale_y_continuous(labels = scales::percent) +
  labs(
    title = "Income vs. Online Shopping Frequency",
    x = "Personal Monthly Allowance/Income",
    y = "Percentage",
    fill = ""
  ) + 
  theme_classic() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold", size = 14),
    axis.title.x = element_text(face = "bold"),
    axis.title.y = element_text(face = "bold"),
    legend.position = "top"
  )
```

![](impulse_buy_files/figure-gfm/income-vs-shopping-frequency-1.png)<!-- -->

**Key Insights**

- Lower-income respondents shop least often: in **Below 5,000**, 76%
  shop **Rarely** or **Once a couple months**.

- Purchase frequency rises with income—the mid and upper bands shift
  toward **Once a month** and more.

- **Once a week or more** is rare across every income band, so heavy
  shoppers are uncommon regardless of income.

## Platform & Shopping Behavior

### Most Used Fashion Purchase Platform & Preferred Payment Method Distribution

``` r
# creating a stacked bar chart to observe the fashion purchase platform vs online preferred payment method distribution
ggplot(data = df, aes(x = q6, fill = q4)) +
  geom_bar(position = "stack") +
  labs(
    title = "Most Used Purchase Platform & Preferred Payment Method Distribution",
    x = "Fashion Purchase Platforms",
    y = "Count",
    fill = ""
  ) + 
  theme_classic() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold", size = 14),
    axis.title.x = element_text(face = "bold"),
    axis.title.y = element_text(face = "bold"),
    legend.position = "top"
  )
```

![](impulse_buy_files/figure-gfm/platform-payment-distribution-1.png)<!-- -->

**Key Insights**

- **Facebook** is the dominant platform (88 of 160), followed by
  **Instagram** (39); **Daraz** (8) and **brand websites** (25) are
  minor.

- **Cash on Delivery** is the most common payment method overall, and
  especially on Facebook (51 of 88 ≈ 58%).

- Interestingly, brand-website buyers lean toward mobile financial
  services (13 of 25) rather than COD—a different pattern from the
  social platforms.

### Shopping Frequency

``` r
# creating a barchart of how often people actually shops
ggplot(data = df, aes(x = q5)) +
  geom_bar() +
  labs(
    title = "Gen Z Shopping Frequency",
    x = "How Often Gen Z Consumers Buy Fashion Items Online",
    y = "Count"
  ) +
  theme_classic() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold", size = 14),
    axis.title.x = element_text(face = "bold"),
    axis.title.y = element_text(face = "bold")
  )
```

![](impulse_buy_files/figure-gfm/shopping-frequency-1.png)<!-- -->

**Key Insights**

The modal category is **Once a couple months** (44), and only 8
respondents shop weekly—the distribution is concentrated in
low-to-moderate frequency.

## Peer Pressure

### Peer Pressure Score Distribution

``` r
# creating a histogram of the peer pressure index column created earlier
ggplot(data = df, aes(x = peer_influence_index)) +
  geom_histogram(bins = 20) +
  labs(title = "Peer Pressure Index Distribution", x = "Peer Pressure Index", y = "Count") +
  theme_classic() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold", size = 14),
    axis.title.x = element_text(face = "bold"),
    axis.title.y = element_text(face = "bold")
  )
```

![](impulse_buy_files/figure-gfm/peer-pressure-distribution-1.png)<!-- -->

**Key Insights**

- Centered near the scale midpoint (mean 2.96, median 3.0, SD 0.84).

- Notably, no respondent scores above 4.4 despite a 1–5 scale—the top of
  the range is empty, suggesting few strongly peer-driven consumers.

### Peer Pressure by Demographics

``` r
# peer pressure index by gender & age
ggplot(data = df,aes(x = q2, y = peer_influence_index, fill = q1)) +
  geom_boxplot() +
  labs(
    title = "Peer Pressure Index by Gender & Age",
    x = "Gender",
    y = "Peer Pressure Index",
    fill = "Age Range"
  ) +
  theme_classic() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold", size = 14),
    axis.title.x = element_text(face = "bold"),
    axis.title.y = element_text(face = "bold"),
    legend.position = "top"
  )
```

![](impulse_buy_files/figure-gfm/peer-pressure-demographics-1.png)<!-- -->

**Key Insights**

- Females report higher peer influence than males at both ages (medians
  ~3.2–3.5 vs ~2.6–2.8).

- The gender gap is larger than the age gap, hinting gender matters more
  than age here.

### Peer Pressure vs. Regret

``` r
# displaying the relationship between purchase regret and peer pressure index
ggplot(data = df, aes(
  x = peer_influence_index,
  fill = q25_label
)) +
  geom_density(alpha = 0.5) +
  labs(
    title = "Peer Pressure Index vs. Regret",
    x = "Peer Pressure Index",
    y = "Density",
    fill = "Regretted a Purchase?"
  ) +
  theme_classic() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold", size = 14),
    axis.title.x = element_text(face = "bold"),
    axis.title.y = element_text(face = "bold"),
    legend.position = "top"
  )
```

![](impulse_buy_files/figure-gfm/peer-vs-regret-1.png)<!-- -->

### Peer Pressure vs. Unnecessary Purchase

``` r
# displaying the relationship between unnecessary purchase and peer pressure index
ggplot(data = df, aes(
  x = peer_influence_index,
  fill = q24_label
  )) +
  geom_density(alpha = 0.5) +
  labs(
    title = "Peer Pressure Index vs. Unnecessary Purchase",
    x = "Peer Pressure Index",
    y = "Density",
    fill = "Had an Unnecessary Purchase?"
    ) +
  theme_classic() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold", size = 14),
    axis.title.x = element_text(face = "bold"),
    axis.title.y = element_text(face = "bold"),
    legend.position = "top"
  )
```

![](impulse_buy_files/figure-gfm/peer-vs-unnecessary-1.png)<!-- -->

**Key Insights**

Peer influence shows little separation by either outcome—regret and
unnecessary purchase.

### Peer Pressure vs. Social Status

``` r
# peer pressure index vs social status index
ggplot(df, aes(x = peer_influence_index, y = social_status_index)) +
  geom_point(alpha = 0.6) +
  geom_smooth(method = "lm") +
  labs(
    title = "Peer Pressure vs. Social Status",
    x = "Peer Pressure Index",
    y = "Social Status Index"
  ) +
  theme_classic() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold", size = 14),
    axis.title.x = element_text(face = "bold"),
    axis.title.y = element_text(face = "bold")
  )
```

    ## `geom_smooth()` using formula = 'y ~ x'

![](impulse_buy_files/figure-gfm/peer-vs-social-status-1.png)<!-- -->

**Key Insights**

A weak-to-moderate positive trend (r ≈ 0.34) suggests that respondents
who feel more peer pressure also tend to perceive higher social status
pressure, but the relationship is far from deterministic.

### Peer Pressure vs. Personality Traits

``` r
# peer pressure index vs personality index
ggplot(df, aes(x = peer_influence_index, y = personality_index)) +
  geom_point(alpha = 0.6) +
  geom_smooth(method = "lm") +
  labs(
    title = "Peer Pressure vs. Personality Traits",
    x = "Peer Pressure Index",
    y = "Personality Traits Index"
  ) +
  theme_classic() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold", size = 14),
    axis.title.x = element_text(face = "bold"),
    axis.title.y = element_text(face = "bold")
  )
```

    ## `geom_smooth()` using formula = 'y ~ x'

![](impulse_buy_files/figure-gfm/peer-vs-personality-1.png)<!-- -->

**Key Insights**

A similarly weak positive association (r ≈ 0.30)—peer influence and
personality share some common ground but are largely independent
constructs.

### Peer Pressure vs. Dependent Variable

``` r
# creating a scatterplot of the peer pressure index and the dependent variable index
ggplot(data = df, aes(x = peer_influence_index, y = dependent_variable_index)) +
  geom_point(alpha = 0.6) +
  geom_smooth(method = "lm") +
  labs(
    title = "Peer Pressure vs. Dependent Variable",
    x = "Peer Pressure Index",
    y = "Dependent Variable Index"
  ) +
  theme_classic() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold", size = 14),
    axis.title.x = element_text(face = "bold"),
    axis.title.y = element_text(face = "bold")
  )
```

    ## `geom_smooth()` using formula = 'y ~ x'

![](impulse_buy_files/figure-gfm/peer-vs-dv-1.png)<!-- -->

**Key Insights**

- Peer influence correlates weakly-to-moderately with social status (r =
  0.34) and personality (r = 0.30).

- Its correlation with the dependent variable is the weakest of all
  predictors (r = 0.18)—an early hint that peer influence may not drive
  impulsive buying directly.

## Perceived Social Status

### Social Status Score Distribution

``` r
# creating a histogram of the social status index column created earlier
ggplot(data = df, aes(x = social_status_index)) +
  geom_histogram(bins = 20) +
  labs(title = "Social Status Index Distribution", x = "Social Status Index", y = "Count") +
  theme_classic() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold", size = 14),
    axis.title.x = element_text(face = "bold"),
    axis.title.y = element_text(face = "bold")
  )
```

![](impulse_buy_files/figure-gfm/social-status-distribution-1.png)<!-- -->

**Key Insights**

Roughly central and symmetric (mean 2.78, median 2.75, SD 0.86),
spanning the full 1–5 range with no unusual features.

### Social Status by Demographics

``` r
# social status index by gender & age
ggplot(data = df,aes(x = q2, y = social_status_index, fill = q1)) +
  geom_boxplot() +
  labs(
    title = "Social Status Index by Gender & Age",
    x = "Gender",
    y = "Social Status Index",
    fill = "Age Range"
  ) +
  theme_classic() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold", size = 14),
    axis.title.x = element_text(face = "bold"),
    axis.title.y = element_text(face = "bold"),
    legend.position = "top"
  )
```

![](impulse_buy_files/figure-gfm/social-status-demographics-1.png)<!-- -->

**Key Insights**

- Females again score somewhat higher than males (medians ~2.75–3.0 vs
  2.25–2.5).

- Differences are modest and the boxes overlap, so the gender gap is
  smaller here than for peer influence.

### Social Status vs. Regret

``` r
# displaying the relationship between purchase regret and social status index
ggplot(data = df, aes(
  x = social_status_index,
  fill = q25_label
  )) +
  geom_density(alpha = 0.5) +
  labs(
    title = "Social Status Index vs. Regret",
    x = "Social Status Index",
    y = "Density",
    fill = "Regretted a Purchase?"
  ) +
  theme_classic() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold", size = 14),
    axis.title.x = element_text(face = "bold"),
    axis.title.y = element_text(face = "bold"),
    legend.position = "top"
  )
```

![](impulse_buy_files/figure-gfm/social-status-vs-regret-1.png)<!-- -->

**Key Insights**

Social status shows little separation by either regret or unnecessary
purchase—the density curves for all groups largely overlap, mirroring
the weak pattern seen with peer influence.

### Social Status vs. Unnecessary Purchase

``` r
# displaying the relationship between social status and unnecessary purchases
ggplot(data = df, aes(
  x = social_status_index,
  fill = q24_label
  )) +  
  geom_density(alpha = 0.5) +
  labs(
    title = "Social Status Index vs. Unnecessary Purchase",
    x = "Social Status Index",
    y = "Density",
    fill = "Purchased Something that You Never Used?"
    ) +
  theme_classic() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold", size = 14),
    axis.title.x = element_text(face = "bold"),
    axis.title.y = element_text(face = "bold"),
    legend.position = "top"
  )
```

![](impulse_buy_files/figure-gfm/social-status-vs-unnecessary-1.png)<!-- -->

### Social Status vs. Personality Traits

``` r
# social status index vs. personality index
ggplot(df, aes(x = social_status_index, y = personality_index)) +
  geom_point(alpha = 0.6) +
  geom_smooth(method = "lm") +
  labs(
    title = "Social Status vs. Personality Traits",
    x = "Social Status Index",
    y = "Personality Traits Index"
  ) +
  theme_classic() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold", size = 14),
    axis.title.x = element_text(face = "bold"),
    axis.title.y = element_text(face = "bold")
  )
```

    ## `geom_smooth()` using formula = 'y ~ x'

![](impulse_buy_files/figure-gfm/social-status-vs-personality-1.png)<!-- -->

**Key Insights**

A weak positive association (r ≈ 0.27) between social status and
personality—the two constructs share some overlap but measure distinct
dimensions.

### Social Status vs. Dependent Variable

``` r
# creating a scatterplot of the social status index and the dependent variable index
ggplot(data = df, aes(x = social_status_index, y = dependent_variable_index)) +
  geom_point(alpha = 0.6) +
  geom_smooth(method = "lm") +
  labs(
    title = "Social Status vs. Dependent Variable",
    x = "Social Status Index",
    y = "Dependent Variable Index"
  ) +
  theme_classic() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold", size = 14),
    axis.title.x = element_text(face = "bold"),
    axis.title.y = element_text(face = "bold")
  )
```

    ## `geom_smooth()` using formula = 'y ~ x'

![](impulse_buy_files/figure-gfm/social-status-vs-dv-1.png)<!-- -->

**Key Insights**

Social status correlates moderately with the dependent variable (r ≈
0.29)—stronger than peer influence but well below personality’s standout
association.

## Personality Traits

### Personality Score Distribution

``` r
# creating a histogram of personality index distribution
ggplot(data = df, aes(x = personality_index)) +
  geom_histogram(bins = 20) +
  labs(title = "Personality Index Distribution", x = "Personality Index", y = "Count") +
  theme_classic() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold", size = 14),
    axis.title.x = element_text(face = "bold"),
    axis.title.y = element_text(face = "bold")
  )
```

![](impulse_buy_files/figure-gfm/personality-distribution-1.png)<!-- -->

**Key Insights**

Mean 2.76, median 2.71, SD 0.90; the widest-spread predictor (SD 0.90),
covering the full 1–5 range.

### Personality Traits by Gender & Age

``` r
# personality index by gender & age
ggplot(data = df,aes(x = q2, y = personality_index, fill = q1)) +
  geom_boxplot() +
  labs(
    title = "Personality Index by Gender & Age",
    x = "Gender",
    y = "Personality Index",
    fill = "Age Range"
  ) +
  theme_classic() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold", size = 14),
    axis.title.x = element_text(face = "bold"),
    axis.title.y = element_text(face = "bold"),
    legend.position = "top"
  )
```

![](impulse_buy_files/figure-gfm/personality-demographics-1.png)<!-- -->

**Key Insights**

Females have a markedly higher median (3.0 at both ages) than males
(2.14–2.29)—the largest, most consistent gender gap among the three
indices.

### Personality Traits vs. Regret

``` r
# displaying the relationship between purchase regret and personality index
ggplot(data = df, aes(
  x = personality_index,
  fill = q25_label
  )) +
  geom_density(alpha = 0.5) +
  labs(
    title = "Personality Index vs. Regret",
    x = "Personality Index",
    y = "Density",
    fill = "Regretted a Purchase?"
  ) +
  theme_classic() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold", size = 14),
    axis.title.x = element_text(face = "bold"),
    axis.title.y = element_text(face = "bold"),
    legend.position = "top"
  )
```

![](impulse_buy_files/figure-gfm/personality-vs-regret-1.png)<!-- -->

**Key Insights**

- The **No regret** group has a distinctly lower mean personality index
  (2.38) than the **Sometimes**/**Yes** groups (~3.0).

- The jump happens between **No** and any regret, rather than scaling
  smoothly—those who never regret look qualitatively different.

### Personality Traits vs. Unnecessary Purchase

``` r
# displaying the relationship between personality influence index and unnecessary purchase
ggplot(data = df, aes(
  x = personality_index,
  fill = q24_label
  )) +
  geom_density(alpha = 0.5) +
  labs(
    title = "Personality Index vs. Unnecessary Purchase",
    x = "Personality Index",
    y = "Density",
    fill = "Bought an Unnecessary Purchase?"
    ) +
  theme_classic() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold", size = 14),
    axis.title.x = element_text(face = "bold"),
    axis.title.y = element_text(face = "bold"),
    legend.position = "top"
  )
```

![](impulse_buy_files/figure-gfm/personality-vs-unnecessary-1.png)<!-- -->

**Key Insights**

- Clearest gradient of all the density plots: mean personality rises
  2.29 → 2.76 → 3.34 across **No**/**a few**/**many** unused purchases.

- Suggests higher-personality-index respondents accumulate more unused
  items—consistent with an impulsivity interpretation.

### Personality Traits vs. Dependent Variable

``` r
# creating a scatterplot of the personality index and the dependent variable index
ggplot(data = df, aes(x = personality_index, y = dependent_variable_index)) +
  geom_point(alpha = 0.6) +
  geom_smooth(method = "lm") +
  labs(
    title = "Personality vs. Dependent Variable",
    x = "Personality Index",
    y = "Dependent Variable Index"
    ) +
  theme_classic() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold", size = 14),
    axis.title.x = element_text(face = "bold"),
    axis.title.y = element_text(face = "bold")
  )
```

    ## `geom_smooth()` using formula = 'y ~ x'

![](impulse_buy_files/figure-gfm/personality-vs-dv-1.png)<!-- -->

**Key Insights**

- Strongest association of any predictor with the DV (r = 0.57).

- The positive slope is visibly tighter than the peer or status
  scatters, foreshadowing personality as the key predictor in H1/H2.

## Post-Purchase Outcomes & Rationalizations

### How Often Gen Z Make Unplanned Purchases

``` r
# creating a barchart of how often people make unplanned purchases
ggplot(data = df, aes(
  x = q23_label
  )) +
  geom_bar() +
  labs(
    title = "Unplanned Purchase Distribution",
    x = "How Often Gen Z Consumers Make Unplanned Purchases",
    y = "Count"
  ) +
  theme_classic() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold", size = 14),
    axis.title.x = element_text(face = "bold"),
    axis.title.y = element_text(face = "bold")
  )
```

![](impulse_buy_files/figure-gfm/unplanned-purchase-dist-1.png)<!-- -->

**Key Insights**

Centered on **Sometimes** (60) and **Rarely** (45); extremes are
uncommon (**Very Often** 11, **Never** 9).

### How Frequently Gen Z Consumers Bought an Item That Were Never Used

``` r
# creating a barchart of how often people bought items they never used
ggplot(data = df, aes(
  x = q24_label
)) +
  geom_bar() +
  labs(
    title = "Unused Purchase Distribution",
    x = "How Often Gen Z Consumers Made a Purchase that They Never Used",
    y = "Count"
  ) +
  theme_classic() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold", size = 14),
    axis.title.x = element_text(face = "bold"),
    axis.title.y = element_text(face = "bold")
  )
```

![](impulse_buy_files/figure-gfm/unused-purchase-dist-1.png)<!-- -->

**Key Insights**

Most report **a few times** (77); **many times** (37) exceeds **never**
(46) only modestly.

### How Often Gen Z Consumers Bought an Item That They Regretted

``` r
# creating a barchart of how often people regretted a purchase
ggplot(data = df, aes(
  x = q25_label
)) +
  geom_bar() +
  labs(
    title = "Regrettable Purchase Distribution",
    x = "How Often Gen Z Consumers Bought an Item That They Regretted",
    y = "Count"
  ) +
  theme_classic() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold", size = 14),
    axis.title.x = element_text(face = "bold"),
    axis.title.y = element_text(face = "bold")
  )
```

![](impulse_buy_files/figure-gfm/regret-dist-1.png)<!-- -->

**Key Insights**

- The distribution is **bimodal/polarized**—63 say **No** and 68 say
  **Yes**, with only 29 in the middle.

- Respondents tend to fall clearly into **regret** or **no regret**
  camps rather than the middle—relevant to how you frame H7.

## Payment Mechanics & Financial Access

``` r
# creating a side-by-side bar chart of the payment methods by income
ggplot(df, aes(x = q3, fill = q4)) +
  geom_bar(position = "dodge") +
  labs(
    title = "Payment Method by Income",
    x = "Personal Monthly Allowance/Income",
    y = "Count",
    fill = "Payment Method"
    ) +
  theme_classic() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold", size = 14),
    axis.title.x = element_text(face = "bold"),
    axis.title.y = element_text(face = "bold"),
    legend.position = "top"
  )
```

![](impulse_buy_files/figure-gfm/payment-by-income-1.png)<!-- -->

**Key Insights**

- COD dominates every income band, but its share shrinks as income
  rises.

- Credit/debit card use is almost entirely confined to the **Above
  20,000** band (9 of 14 card users), the clearest income effect here.

## Bringing All the Indices Together

``` r
indices <- c("peer_influence_index", "social_status_index", "personality_index", "dependent_variable_index")

cor_long <- df |>
  select(all_of(indices)) |>
  cor(use = "complete.obs") |>
  as_tibble(rownames = "var1") |>
  pivot_longer(-var1, names_to = "var2", values_to = "corr")

ggplot(data = cor_long, aes(x = var1, y = var2, fill = corr)) +
  geom_tile(color = "white") +
  geom_text(aes(label = sprintf("%.2f", corr)), size = 4) +
  scale_fill_gradient2(
    low = "#377EB8",
    mid = "white",
    high = "#E41A1C",
    midpoint = 0,
    limits = c(-1, 1)
    ) +
  scale_x_discrete(labels = function(x) gsub("_", " ", x)) +
  scale_y_discrete(labels = function(x) gsub("_", " ", x)) +
  labs(
    title = "Correlation Heatmap of Composite Indices",
    x = NULL,
    y = NULL,
    fill = "Pearson r"
    ) +
  theme_classic() +
  theme(
    plot.title = element_text(hjust = 0.5, face = "bold", size = 14),
    axis.text.x = element_text(angle = 30, hjust = 1)
    )
```

![](impulse_buy_files/figure-gfm/correlation-heatmap-1.png)<!-- -->

**Key Insights**

- All four indices are positively correlated (0.18–0.57); no negative
  relationships.

- Personality ↔ DV (0.57) is the standout; peer ↔ DV (0.18) the weakest.

- Inter-predictor correlations are low (0.27–0.34), so multicollinearity
  is unlikely to trouble the H1 regression.

# Hypothesis Testing

For this paper, we have set up **ten** hypotheses which we will now
scrutinize using the appropriate statistical tests. **These ten
hypotheses are:**

1.  Peer Influence, Perceived Social Status, and Behavioral Traits
    jointly and significantly predict impulsive online fashion buying
    behavior among Dhaka Gen Z consumers.
2.  Specific Behavioral Traits have no significant effect on impulse
    buying behavior among Dhaka Gen Z consumers.
3.  Peer influence has no significant effect on impulsive online fashion
    buying behavior among Dhaka Gen Z consumers.
4.  Perceived social status has no significant effect on impulse buying
    behavior among Dhaka Gen Z consumers.
5.  Peer Influence has no significant effect on perceived social status
    pressure among Dhaka Gen Z consumers.
6.  Impulsive online fashion buying behavior does not differ
    significantly based on the primary social commerce platform used by
    Dhaka Gen Z consumers.
7.  Higher impulsivity is not significantly associated with
    post-purchase regrets among Dhaka Gen Z consumers.
8.  There is no significant association between preferred payment method
    and impulsive online fashion buying behavior among Dhaka Gen Z
    consumers.
9.  Dhaka Gen Z consumers with higher monthly personal allowance does
    not exhibit significantly higher impulsive buying behavior.
10. There is no significant difference in impulsive online fashion
    buying behavior between male and female Dhaka Gen Z consumers.

## Hypothesis 01

***Peer Influence, Perceived Social Status, and Behavioral Traits
jointly and significantly predict impulsive online fashion buying
behavior among Dhaka Gen Z consumers.***

This hypothesis aims to find the relationship between the core
independent variables (peer influence, perceived social status,
personality traits) and impulsive buying. As the independent variables
(Peer Influence Index, Social Status Index, and Personality Index) are
numeric composite scores and the dependent variable (Impulsive Buying
Index) is also a numeric composite score, a **two-tailed multiple linear
regression** was conducted to examine whether the null hypothesis can be
rejected.

``` r
# simple multiple linear regression
model_h1 <- lm(
  formula = dependent_variable_index ~ peer_influence_index + social_status_index + personality_index,
  data = df
  )

summary(model_h1)
```

    ## 
    ## Call:
    ## lm(formula = dependent_variable_index ~ peer_influence_index + 
    ##     social_status_index + personality_index, data = df)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -2.08507 -0.62758  0.01845  0.63899  2.03376 
    ## 
    ## Coefficients:
    ##                      Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)           0.80829    0.30789   2.625  0.00952 ** 
    ## peer_influence_index -0.03818    0.08689  -0.439  0.66097    
    ## social_status_index   0.18606    0.08442   2.204  0.02899 *  
    ## personality_index     0.62050    0.07923   7.832 6.92e-13 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.8461 on 156 degrees of freedom
    ## Multiple R-squared:  0.3473, Adjusted R-squared:  0.3348 
    ## F-statistic: 27.67 on 3 and 156 DF,  p-value: 2.1e-14

The test reveals the p-value to be **less than 0.001**, which is less
than or equal to the α (0.05), and the model explains a substantial
portion (**34.73%**) of the variance. Holding other variables constant,
personality traits (β = **0.621**, p \< 0.001) and perceived social
status (β = **0.186**, p = 0.029) are both significantly related to the
dependent variable, whereas peer influence is not (β = **-0.038**, p =
0.661). So, we can hereby ***reject*** the null that *peer influence,
perceived social status, and personality traits does not jointly and
significantly predict impulsive online fashion buying behavior among
Dhaka Gen Z consumers*, i.e., and accept the alternative that they do
jointly and significantly predict.

## Hypothesis 02

***Specific Behavioral Traits have no significant effect on impulse
buying behavior among Dhaka Gen Z consumers.***

This hypothesis aims to find the relationship between behavioral traits
(shaped by personality traits) and impulsive buying. As both the
independent variable (Personality Traits Index) and dependent variable
(Impulsive Buying Index) are numeric composite scores, a **simple linear
regression** was performed to determine whether the null can be
rejected.

``` r
# Linear Regression
model_h2 <- lm(
  formula = dependent_variable_index ~ personality_index,
  data = df
  )

summary(model_h2)
```

    ## 
    ## Call:
    ## lm(formula = dependent_variable_index ~ personality_index, data = df)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -2.05277 -0.64517  0.01046  0.63546  1.91659 
    ## 
    ## Coefficients:
    ##                   Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)        1.11209    0.21771   5.108 9.27e-07 ***
    ## personality_index  0.65711    0.07502   8.759 2.88e-15 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.8538 on 158 degrees of freedom
    ## Multiple R-squared:  0.3269, Adjusted R-squared:  0.3226 
    ## F-statistic: 76.72 on 1 and 158 DF,  p-value: 2.884e-15

The test reveals the p-value to be **less than 0.001**, which is less
than or equal to the α (0.05), and personality traits now explain a
substantial portion (**32.69%**) of the variance in impulsive buying.
The relationship is positive (β = **0.657**), meaning stronger
personality-driven tendencies are associated with higher, not lower,
impulsive buying. So, we can hereby ***reject*** the null that *specific
behavioral traits (shaped by personality traits) have no significant
effect on impulse buying behavior among Dhaka Gen Z consumers*, i.e.,
and accept the alternative that specific behavioral traits have a
significant effect on impulse buying.

## Hypothesis 03

***Peer influence has no significant effect on impulsive online fashion
buying behavior among Dhaka Gen Z consumers.***

This hypothesis aims to find the relationship between peer influence and
impulsive buying. As both the independent variable (Peer Influence
Index) and dependent variable (Impulsive Buying Index) are numeric
composite scores, a **simple linear regression** was performed to
determine whether the null can be rejected.

``` r
# simple linear regression
model_h3 <- lm(
  formula = dependent_variable_index ~ peer_influence_index,
  data = df
  )

summary(object = model_h3)
```

    ## 
    ## Call:
    ## lm(formula = dependent_variable_index ~ peer_influence_index, 
    ##     data = df)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -2.20285 -0.75516 -0.06854  0.66650  2.24484 
    ## 
    ## Coefficients:
    ##                      Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)           2.26270    0.29602   7.644 1.92e-12 ***
    ## peer_influence_index  0.22384    0.09624   2.326   0.0213 *  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 1.023 on 158 degrees of freedom
    ## Multiple R-squared:  0.03311,    Adjusted R-squared:  0.02699 
    ## F-statistic:  5.41 on 1 and 158 DF,  p-value: 0.0213

The test reveals the p-value to be **0.021**, which is less than or
equal to the α (0.05), though the model still explains only a modest
portion (**3.31%**) of the variance. This means peer influence (β =
**0.224**) is positively and significantly related to impulsive buying.
So, we can hereby ***reject*** the null that *peer influence has no
significant effect on impulsive online fashion buying behavior among
Dhaka Gen Z consumers*, i.e., and accept the alternative that peer
influence has a significant effect.

## Hypothesis 04

***Perceived social status has no significant effect on impulse buying
behavior among Dhaka Gen Z consumers.***

As both the independent variable (Perceived Social Status Index) and
dependent variable (Impulsive Buying Index) are numeric composite
scores, a **simple linear regression** was performed to determine
whether the null can be rejected.

``` r
# simple linear regression
model_h4 <- lm(
  formula = dependent_variable_index ~ social_status_index,
  data = df
  )

summary(object = model_h4)
```

    ## 
    ## Call:
    ## lm(formula = dependent_variable_index ~ social_status_index, 
    ##     data = df)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -2.02073 -0.82689 -0.00035  0.65783  2.52004 
    ## 
    ## Coefficients:
    ##                     Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)          1.95958    0.26772   7.320 1.19e-11 ***
    ## social_status_index  0.34692    0.09194   3.773 0.000227 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.9967 on 158 degrees of freedom
    ## Multiple R-squared:  0.08266,    Adjusted R-squared:  0.07686 
    ## F-statistic: 14.24 on 1 and 158 DF,  p-value: 0.0002275

The test reveals the p-value to be **less than 0.001**, which is less
than or equal to the α (0.05), and the model explains a moderate portion
(**8.27%**) of the variance. Perceived social status (β = **0.347**)
shows a significant positive relationship with impulsive buying
behavior. So, we can hereby ***reject*** the null that *perceived social
status has no significant effect on impulse buying behavior among Dhaka
Gen Z consumers*, i.e., and accept the alternative that perceived social
status has a significant effect.

## Hypothesis 05

***Peer Influence has no significant effect on perceived social status
pressure among Dhaka Gen Z consumers.***

As both the independent variable (Peer Influence Index) and dependent
variable (Perceived Social Status Index) are numeric composite scores, a
**simple linear regression** was performed to determine whether the null
can be rejected.

``` r
# linear regression
model_h5 <- lm(
  formula = social_status_index ~ peer_influence_index,
  data = df
  )

summary(model_h5)
```

    ## 
    ## Call:
    ## lm(formula = social_status_index ~ peer_influence_index, data = df)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -1.58947 -0.68939 -0.02027  0.55283  1.92610 
    ## 
    ## Coefficients:
    ##                      Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)            1.7590     0.2347   7.495 4.44e-12 ***
    ## peer_influence_index   0.3460     0.0763   4.535 1.13e-05 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.8112 on 158 degrees of freedom
    ## Multiple R-squared:  0.1152, Adjusted R-squared:  0.1096 
    ## F-statistic: 20.57 on 1 and 158 DF,  p-value: 1.133e-05

The test reveals the p-value to be **less than 0.001**, which is less
than or equal to the α (0.05). The model explains a modest portion
(**11.52%**) of the variance, with peer influence (β = **0.346**)
positively predicting perceived social status pressure. So, we can
hereby ***reject*** the null that *peer influence has no significant
effect on perceived social status pressure among Dhaka Gen Z consumers*,
i.e., and accept the alternative that peer influence has a significant
effect.

## Hypothesis 06

***Impulsive online fashion buying behavior does not differ
significantly based on the primary social commerce platform used by
Dhaka Gen Z consumers.***

This hypothesis aims to find whether impulsive buying differs across the
primary platform used for fashion shopping. As the independent variable
(Primary Platform Used) is categorical with four groups of highly uneven
size (from n = 8 for Daraz to n = 88 for Facebook) and the dependent
variable (Impulsive Buying Index) is a numeric composite score, a
**Kruskal–Wallis test** (non-parametric, two-tailed) was conducted to
examine whether the null hypothesis can be rejected, since ANOVA’s
normality assumption becomes unreliable for a group this small. A
Kruskal-Wallis test will be used instead of a one-way ANOVA because the
smallest group (Daraz, n = 8) makes ANOVA’s normality-within-group
assumption unreliable—this non-parametric test doesn’t require that
assumption at all.

``` r
# converting the platform variable to factor and drop TikTok (0 responses)
df_h6 <- df |> 
  mutate(q6 = as.factor(q6)) |> 
  filter(q6 != "TikTok")

# running a Kruskal-Wallis test
kruskal.test(
  formula = dependent_variable_index ~ q6,
  data = df_h6
  )
```

    ## 
    ##  Kruskal-Wallis rank sum test
    ## 
    ## data:  dependent_variable_index by q6
    ## Kruskal-Wallis chi-squared = 13.955, df = 3, p-value = 0.002966

The test reveals the p-value to be **0.003**, which is less than or
equal to the α (0.05). This means impulsive online fashion buying
behavior does differ significantly based on the primary social commerce
platform used, with Instagram users showing the highest average
impulsivity and brand-website shoppers the lowest. So, we can hereby
***reject*** the null that *impulsive online fashion buying behavior
does not differ significantly based on the primary social commerce
platform used by Dhaka Gen Z consumers*, i.e., and accept the
alternative that it does differ significantly.

## Hypothesis 07

***Higher impulsivity is not significantly associated with post-purchase
regrets among Dhaka Gen Z consumers.***

This hypothesis aims to find the relationship between impulsivity and
post-purchase regret. As the independent variable (Post-Purchase Regret)
is categorical and the dependent variable (Impulsive Buying Index) is a
numeric composite score, a **one-way ANOVA (two-tailed)** was conducted
to test whether the null hypothesis can be rejected.

``` r
# running a one-way ANOVA test
anova_h7 <- aov(
  formula = dependent_variable_index ~ q25_label,
  data = df
)

summary(anova_h7)
```

    ##              Df Sum Sq Mean Sq F value   Pr(>F)    
    ## q25_label     2  21.79  10.893   11.45 2.27e-05 ***
    ## Residuals   157 149.31   0.951                     
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

The test reveals the p-value to be **less than 0.001**, which is less
than or equal to the α (0.05).

``` r
# running a post-hoc Tukey HSD because the ANOVA came back significant—the ANOVA only tells us some group differs from another whereas Tukey tells us specifically which pairs (e.g., is it Yes vs. No, or Maybe vs. No, or both)
TukeyHSD(x = anova_h7)
```

    ##   Tukey multiple comparisons of means
    ##     95% family-wise confidence level
    ## 
    ## Fit: aov(formula = dependent_variable_index ~ q25_label, data = df)
    ## 
    ## $q25_label
    ##                diff         lwr       upr     p adj
    ## Maybe-No  0.4310345 -0.08677297 0.9488419 0.1231637
    ## Yes-No    0.8161765  0.41266622 1.2196867 0.0000116
    ## Yes-Maybe 0.3851420 -0.12662948 0.8969135 0.1794067

Only **one** of the three pairwise comparisons clears the 0.05 bar.
**Maybe** sits numerically between **No** (2.50) and **Yes** (3.32) at
2.93, but statistically it’s not distinguishable from either neighbor—it
just doesn’t have enough separation (or a big enough sample, n = 29) to
be confident it’s different from either **No** or **Yes**.

``` r
# checking the equal-variance assumption ANOVA relies on (Levene's test)—if this test itself came back significant, it would mean the group variances differ too much for the ANOVA p-value above to be trustworthy
leveneTest(
  y = dependent_variable_index ~ q25_label,
  data = df
)
```

    ## Levene's Test for Homogeneity of Variance (center = median)
    ##        Df F value Pr(>F)
    ## group   2  0.2722 0.7621
    ##       157

From the test, p = **0.762**—not significant, meaning group variances
are roughly equal. This is good news—it means the ANOVA’s equal-variance
assumption holds, so the ANOVA’s p-value can be trusted above rather
than needing to lean on the Kruskal-Wallis backup.

``` r
# optionally running a non-parametric backup (Kruskal-Wallis) as a robustness check—since it makes no assumption about normality or equal variances, agreement between this and the ANOVA above means the conclusion doesn't hinge on those assumptions holding
kruskal.test(
  formula = dependent_variable_index ~ q25_label,
  data = df
)
```

    ## 
    ##  Kruskal-Wallis rank sum test
    ## 
    ## data:  dependent_variable_index by q25_label
    ## Kruskal-Wallis chi-squared = 18.753, df = 2, p-value = 8.467e-05

χ² = **18.75**, p = **8.47e-05**—also significant, agrees with the
ANOVA. Since Levene’s already told us the variances are fine, this
backup test is really just extra reassurance, not doing new work—but
it’s nice that it agrees.

This tests mean higher impulsivity is significantly associated with
post-purchase regret, though the post-hoc comparison shows this is
driven specifically by the **Yes** group—respondents who regretted a
purchase scored significantly higher on impulsivity than those who said
**No** (p \< 0.001). The **Maybe** group falls in between numerically
but is not statistically distinguishable from either **No** (p = 0.123)
or **Yes** (p = 0.179), likely reflecting its smaller sample size (n =
29) as much as genuine ambiguity. So, we can hereby ***reject*** the
null that *higher impulsivity is not significantly associated with
post-purchase regrets among Dhaka Gen Z consumers*, i.e., and accept the
alternative that higher impulsivity is significantly associated with
post-purchase regrets.

## Hypothesis 08

***There is no significant association between preferred payment method
and impulsive online fashion buying behavior among Dhaka Gen Z
consumers.***

This hypothesis aims to find whether preferred payment method is
associated with impulsive buying. As the independent variable (Payment
Method) and the dependent variable (Impulsivity Category, derived from
the frequency of unplanned purchases in Q23, grouped into
Low/Medium/High) are both categorical, a **two-tailed chi-square test of
independence** was conducted to examine whether the null hypothesis can
be rejected. We run the chi-square test with a Monte Carlo simulation
(`simulate.p.value = TRUE`) instead of the standard asymptotic p-value,
because several cells below have small expected counts—the usual
chi-square approximation isn’t reliable when expected counts drop much
below 5

``` r
# categorizing q23 into Low/Medium/High—a chi-square test of independence needs two categorical variables, and payment method is already categorical, so the numeric q23 needs bucketing first
df <- df |> 
  mutate(
    q23_cat = case_when(
      q23 %in% c(1, 2) ~ "Low",
      q23 == 3 ~ "Medium",
      q23 %in% c(4, 5) ~ "High",
      TRUE ~ NA_character_
    )
  )

# building the contingency table (Payment Method x Impulsivity Category)—this cross-tab of observed counts is the actual input the chi-square test compares against expected counts
tab_h8 <- table(df$q4, df$q23_cat)

# viewing the table
tab_h8
```

    ##                                                 
    ##                                                  High Low Medium
    ##   Cash on Delivery (COD)                           26  30     31
    ##   Credit/debit card                                 5   5      4
    ##   Mobile financial services (bKash/Nagad/Rocket)   15  19     25

``` r
cat("\n")
```

``` r
# running a Chi-square test of independence
chisq_result <- chisq.test(
  x = tab_h8,
  simulate.p.value = TRUE,
  B = 10000
)

chisq_result
```

    ## 
    ##  Pearson's Chi-squared test with simulated p-value (based on 10000
    ##  replicates)
    ## 
    ## data:  tab_h8
    ## X-squared = 1.326, df = NA, p-value = 0.8702

``` r
# checking the expected cell counts directly—this is the assumption check, and it's exactly what justified using the simulated p-value above instead of the default one
chisq_result$expected
```

    ##                                                 
    ##                                                     High     Low Medium
    ##   Cash on Delivery (COD)                         25.0125 29.3625 32.625
    ##   Credit/debit card                               4.0250  4.7250  5.250
    ##   Mobile financial services (bKash/Nagad/Rocket) 16.9625 19.9125 22.125

The test reveals the p-value to be **0.857**, which is higher than the α
(0.05). This means there is still no significant association between
preferred payment method and impulsive online fashion buying behavior—in
fact, the relationship is very weak. So, we ***cannot reject*** the null
that *there is no significant association between preferred payment
methods and impulsive online fashion buying behavior among Dhaka Gen Z
consumers*.

## Hypothesis 09

***Dhaka Gen Z consumers with higher monthly personal allowance does not
exhibit significantly higher impulsive buying behavior.***

As the independent variable (Monthly Income/Allowance) is categorical
and the dependent variable (Impulsivity) is a numeric composite score, a
**one-way ANOVA (two-tailed)** was conducted to test whether the null
hypothesis can be rejected.

``` r
# running a one-way ANOVA test
anova_h9 <- aov(
  formula = dependent_variable_index ~ q3,
  data = df
)

summary(anova_h9)
```

    ##              Df Sum Sq Mean Sq F value  Pr(>F)   
    ## q3            4   15.1   3.775   3.751 0.00609 **
    ## Residuals   155  156.0   1.006                   
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1

The test reveals the p-value to be **0.006**, which is less than or
equal to the α (0.05).

``` r
# running a Post-hoc (as the ANOVA is significant) to see which group differs
TukeyHSD(x = anova_h9)
```

    ##   Tukey multiple comparisons of means
    ##     95% family-wise confidence level
    ## 
    ## Fit: aov(formula = dependent_variable_index ~ q3, data = df)
    ## 
    ## $q3
    ##                                    diff         lwr       upr     p adj
    ## 5,000–10,000-Below 5,000     0.29304029 -0.39794874 0.9840293 0.7681330
    ## 10,000–15,000-Below 5,000    0.70238095  0.02680625 1.3779557 0.0372115
    ## 15,000–20,000-Below 5,000    0.79880952  0.04652138 1.5510977 0.0313855
    ## Above 20,000-Below 5,000     0.64880952  0.05146311 1.2461559 0.0259263
    ## 10,000–15,000-5,000–10,000   0.40934066 -0.34481190 1.1634932 0.5652421
    ## 15,000–20,000-5,000–10,000   0.50576923 -0.31780976 1.3293482 0.4400225
    ## Above 20,000-5,000–10,000    0.35576923 -0.32918890 1.0407274 0.6069740
    ## 15,000–20,000-10,000–15,000  0.09642857 -0.71426107 0.9071182 0.9974673
    ## Above 20,000-10,000–15,000  -0.05357143 -0.72297637 0.6158335 0.9994662
    ## Above 20,000-15,000–20,000  -0.15000000 -0.89675247 0.5967525 0.9812482

Out of 10 possible pairwise comparisons, exactly **3** are
significant—and all three involve **Below 5,000** as the lower-scoring
anchor. Notably, **Below 5,000** is **not** significantly different from
its immediate neighbor **5,000–10,000** (p = 0.768)—so the effect isn’t
a smooth income gradient, it’s more like a cliff edge somewhere between
the first and second bracket. And once we’re above 5,000, none of the
four remaining brackets differ from each other at all—10,000–15,000,
15,000–20,000, and Above 20,000 are all statistically tied.

``` r
# doing an assumption check (variance equality)
leveneTest(
  y = dependent_variable_index ~ q3,
  data = df
)
```

    ## Levene's Test for Homogeneity of Variance (center = median)
    ##        Df F value Pr(>F)
    ## group   4  1.1028 0.3573
    ##       155

From the test, p = **0.357**—not significant, so variances across the
five income groups are roughly equal. The ANOVA’s equal-variance
assumption holds, meaning we can trust the F-test above rather than
leaning on the non-parametric backup.

``` r
# conducting a Non-parametric backup (optional)
kruskal.test(
  formula = dependent_variable_index ~ q3, 
  data = df
  )
```

    ## 
    ##  Kruskal-Wallis rank sum test
    ## 
    ## data:  dependent_variable_index by q3
    ## Kruskal-Wallis chi-squared = 14.345, df = 4, p-value = 0.006271

From the test, χ² = **14.345**, df = **4**, p = **0.00627**—also
significant, agrees with the ANOVA, consistent with Levene’s giving the
ANOVA a clean bill of health.

This means Dhaka Gen Z consumers with higher monthly personal allowance
do exhibit significantly higher impulsive buying behavior, with post-hoc
comparisons confirming that the lowest income group (Below 5,000)
differs significantly from every band above 10,000. So, we can hereby
***reject*** the null that *Dhaka Gen Z consumers with higher monthly
personal allowance does not exhibit significantly higher impulsive
buying behavior*, i.e., and accept the alternative that they do.

## Hypothesis 10

***There is no significant difference in impulsive online fashion buying
behavior between male and female Dhaka Gen Z consumers.***

As the independent variable (Gender) is categorical and the dependent
variable (Impulsive Buying Index) is a numeric composite score, a
**two-tailed independent-samples t-test (Welch’s t-test)** was conducted
to examine whether the null hypothesis can be rejected. We run Welch’s
t-test since it doesn’t assume equal variances between groups—a safer
default here given the uneven group sizes (63 males vs. 97 females)

``` r
# keeping only "Male" and "Female" and converting gender into a two-level factor—a t-test compares exactly two groups, so this just locks in which two and their order (Male first, Female second)
df_h10 <- df |>  
  filter(q2 %in% c("Male", "Female")) |>  
  mutate(q2 = factor(q2, levels = c("Male", "Female")))

# running the t-test
t.test(
  formula = dependent_variable_index ~ q2,
  data = df_h10
)
```

    ## 
    ##  Welch Two Sample t-test
    ## 
    ## data:  dependent_variable_index by q2
    ## t = -4.7907, df = 132.84, p-value = 4.378e-06
    ## alternative hypothesis: true difference in means between group Male and group Female is not equal to 0
    ## 95 percent confidence interval:
    ##  -1.0644575 -0.4423336
    ## sample estimates:
    ##   mean in group Male mean in group Female 
    ##             2.468254             3.221649

The test reveals the p-value to be **less than 0.001**, which is less
than or equal to the α (0.05). This means there is a significant
difference in impulsive online fashion buying behavior between male and
female Dhaka Gen Z consumers—with female respondents (M = **3.22**) now
showing significantly higher impulsivity than male respondents (M =
**2.47**). So, we can hereby ***reject*** the null that *there is no
significant difference in impulsive online fashion buying behavior
between male and female Dhaka Gen Z consumers*, i.e., and accept the
alternative that there is a significant difference.

# Summary & Conclusions

## Summary Table

A summary table of all the ten hypotheses are presented below:

***Table 01:** Summary of Hypothesis Testing Results.*

| No. | Hypothesis | Result |
|----|----|----|
| **01** | Peer Influence, Perceived Social Status, and Behavioral Traits jointly and significantly predict impulsive online fashion buying behavior among Dhaka Gen Z consumers. | Reject Null |
| **02** | Specific Behavioral Traits have no significant effect on impulse buying behavior among Dhaka Gen Z consumers. | Reject Null |
| **03** | Peer influence has no significant effect on impulsive online fashion buying behavior among Dhaka Gen Z consumers. | Reject Null |
| **04** | Perceived social status has no significant effect on impulse buying behavior among Dhaka Gen Z consumers. | Reject Null |
| **05** | Peer Influence has no significant effect on perceived social status pressure among Dhaka Gen Z consumers. | Reject Null |
| **06** | Impulsive online fashion buying behavior does not differ significantly based on the primary social commerce platform used by Dhaka Gen Z consumers. | Reject Null |
| **07** | Higher impulsivity is not significantly associated with post-purchase regrets among Dhaka Gen Z consumers. | Reject Null |
| **08** | There is no significant association between preferred payment method and impulsive online fashion buying behavior among Dhaka Gen Z consumers. | Cannot Reject Null |
| **09** | Dhaka Gen Z consumers with higher monthly personal allowance does not exhibit significantly higher impulsive buying behavior. | Reject Null |
| **10** | There is no significant difference in impulsive online fashion buying behavior between male and female Dhaka Gen Z consumers. | Reject Null |

## Conclusion

This study concludes that impulsive fashion buying among Dhaka’s Gen Z
is driven primarily by internal psychological states rather than
external mechanics. The findings reveal that while peer influence
indirectly creates social status pressure, the actual decision to
purchase is fueled by internal traits and the desire for emotional
gratification. Furthermore, the prevalence of post-purchase regret
highlights that impulsive consumption is largely a cyclical process of
mood regulation that often conflicts with rational financial behavior.
