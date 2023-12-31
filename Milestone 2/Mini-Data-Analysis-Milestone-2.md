Mini Data Analysis Milestone 2
================

*To complete this milestone, you can either edit [this `.rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are commented out with
`<!--- start your work here--->`. When you are done, make sure to knit
to an `.md` file by changing the output in the YAML header to
`github_document`, before submitting a tagged release on canvas.*

# Welcome to the rest of your mini data analysis project!

In Milestone 1, you explored your data. and came up with research
questions. This time, we will finish up our mini data analysis and
obtain results for your data by:

- Making summary tables and graphs
- Manipulating special data types in R: factors and/or dates and times.
- Fitting a model object to your data, and extract a result.
- Reading and writing data as separate files.

We will also explore more in depth the concept of *tidy data.*

**NOTE**: The main purpose of the mini data analysis is to integrate
what you learn in class in an analysis. Although each milestone provides
a framework for you to conduct your analysis, it’s possible that you
might find the instructions too rigid for your data set. If this is the
case, you may deviate from the instructions – just make sure you’re
demonstrating a wide range of tools and techniques taught in this class.

# Instructions

**To complete this milestone**, edit [this very `.Rmd`
file](https://raw.githubusercontent.com/UBC-STAT/stat545.stat.ubc.ca/master/content/mini-project/mini-project-2.Rmd)
directly. Fill in the sections that are tagged with
`<!--- start your work here--->`.

**To submit this milestone**, make sure to knit this `.Rmd` file to an
`.md` file by changing the YAML output settings from
`output: html_document` to `output: github_document`. Commit and push
all of your work to your mini-analysis GitHub repository, and tag a
release on GitHub. Then, submit a link to your tagged release on canvas.

**Points**: This milestone is worth 50 points: 45 for your analysis, and
5 for overall reproducibility, cleanliness, and coherence of the Github
submission.

**Research Questions**: In Milestone 1, you chose two research questions
to focus on. Wherever realistic, your work in this milestone should
relate to these research questions whenever we ask for justification
behind your work. In the case that some tasks in this milestone don’t
align well with one of your research questions, feel free to discuss
your results in the context of a different research question.

# Learning Objectives

By the end of this milestone, you should:

- Understand what *tidy* data is, and how to create it using `tidyr`.
- Generate a reproducible and clear report using R Markdown.
- Manipulating special data types in R: factors and/or dates and times.
- Fitting a model object to your data, and extract a result.
- Reading and writing data as separate files.

# Setup

Begin by loading your data and the tidyverse package below:

``` r
library(datateachr) # <- might contain the data you picked!
library(tidyverse)
```

# Task 1: Process and summarize your data

From milestone 1, you should have an idea of the basic structure of your
dataset (e.g. number of rows and columns, class types, etc.). Here, we
will start investigating your data more in-depth using various data
manipulation functions.

### 1.1 (1 point)

First, write out the 4 research questions you defined in milestone 1
were. This will guide your work through milestone 2:

<!-------------------------- Start your work below ---------------------------->

1.  Does the mean number of concave points (concave_points_mean) differ
    between patients diagnosed with a malignant (M) or benign (B) tumor?

2.  Do patients with malignant tumors tend to have larger/worse nuclei
    radius (radius_worse) than those with benign tumors?

3.  Do patients with more concave points (concave_points_mean) also tend
    to have more severe concavity (concavity_worse)?

4.  Can mean smoothness (smoothness_mean) be used to predict mean
    texture (texture_mean)?

Note: This question has been changed because the originally was a bit
too vague with how many variables there are, and did not really fit the
tasks.

(Original:What variables can be used to predict mean texture
(texture_mean)?)

<!----------------------------------------------------------------------------->

Here, we will investigate your data using various data manipulation and
graphing functions.

### 1.2 (8 points)

Now, for each of your four research questions, choose one task from
options 1-4 (summarizing), and one other task from 4-8 (graphing). You
should have 2 tasks done for each research question (8 total). Make sure
it makes sense to do them! (e.g. don’t use a numerical variables for a
task that needs a categorical variable.). Comment on why each task helps
(or doesn’t!) answer the corresponding research question.

Ensure that the output of each operation is printed!

Also make sure that you’re using dplyr and ggplot2 rather than base R.
Outside of this project, you may find that you prefer using base R
functions for certain tasks, and that’s just fine! But part of this
project is for you to practice the tools we learned in class, which is
dplyr and ggplot2.

**Summarizing:**

1.  Compute the *range*, *mean*, and *two other summary statistics* of
    **one numerical variable** across the groups of **one categorical
    variable** from your data.
2.  Compute the number of observations for at least one of your
    categorical variables. Do not use the function `table()`!
3.  Create a categorical variable with 3 or more groups from an existing
    numerical variable. You can use this new variable in the other
    tasks! *An example: age in years into “child, teen, adult, senior”.*
4.  Compute the proportion and counts in each category of one
    categorical variable across the groups of another categorical
    variable from your data. Do not use the function `table()`!

**Graphing:**

6.  Create a graph of your choosing, make one of the axes logarithmic,
    and format the axes labels so that they are “pretty” or easier to
    read.
7.  Make a graph where it makes sense to customize the alpha
    transparency.

Using variables and/or tables you made in one of the “Summarizing”
tasks:

8.  Create a graph that has at least two geom layers.
9.  Create 3 histograms, with each histogram having different sized
    bins. Pick the “best” one and explain why it is the best.

Make sure it’s clear what research question you are doing each operation
for!

<!------------------------- Start your work below ----------------------------->

#### Research Question 1: Does the mean number of concave points (concave_points_mean) differ between patients diagnosed with a malignant (M) or benign (B) tumor?

**Summarizing**: Compute the *range*, *mean*, and *two other summary
statistics* of **one numerical variable** across the groups of **one
categorical variable** from your data.

``` r
q1_table <- cancer_sample %>% 
  group_by(diagnosis) %>% 
  summarise(mean = mean(concave_points_mean),
            min = min(concave_points_mean),
            max = max(concave_points_mean),
            sd = sd(concave_points_mean),
            median = median(concave_points_mean))

q1_table
```

    ## # A tibble: 2 × 6
    ##   diagnosis   mean    min    max     sd median
    ##   <chr>      <dbl>  <dbl>  <dbl>  <dbl>  <dbl>
    ## 1 B         0.0257 0      0.0853 0.0159 0.0234
    ## 2 M         0.0880 0.0203 0.201  0.0344 0.0863

Because the range is the minimum and maximum values, in order to have
one row per level of the categorical variable, I made 2 separate
columns, min and max.

This helps answer the research question because I can compare the mean
values of concave_points_mean for both diagnoses.

**Graphing**: Make a graph where it makes sense to customize the alpha
transparency

``` r
ggplot(cancer_sample, aes(concave_points_mean,fill=diagnosis,color=diagnosis))+
  geom_density(alpha=0.5)+
  labs(x="Mean Number of Concave Points",
       y="Density",
       title = "Density of Number of Concave Points by Diagnosis")
```

![](Mini-Data-Analysis-Milestone-2_files/figure-gfm/Task%201.2_RQ1-1.png)<!-- -->

When plotting densities, it makes sense to change the transparency so
you can see both densities even when they are overlapping.

This helps answer the research question because I can see that the
density of mean concave points for the benign group is more skewed to
the left while the density of mean points for the malignant group is
more to the right, indicating they may have more concave points on
average.

#### Research Question 2: Do patients with malignant tumors tend to have larger/worse nuclei radius (radius_worse) than those with benign tumors?

**Summarizing**: Create a categorical variable with 3 or more groups
from an existing numerical variable. You can use this new variable in
the other tasks! An example: age in years into “child, teen, adult,
senior”.

I split the range into 3 to make the cutoffs for my categories.

``` r
range(cancer_sample$radius_worst)
```

    ## [1]  7.93 36.04

``` r
seq(7.93,36.04,length.out=4)
```

    ## [1]  7.93 17.30 26.67 36.04

``` r
q2_table <- cancer_sample %>%
  mutate(radius_worst_category = case_when(radius_worst < 17~ "small",
                                           radius_worst >= 17 & radius_worst < 27 ~ "medium",
                                           .default = "large")) 

head(select(q2_table,radius_worst,radius_worst_category))
```

    ## # A tibble: 6 × 2
    ##   radius_worst radius_worst_category
    ##          <dbl> <chr>                
    ## 1         25.4 medium               
    ## 2         25.0 medium               
    ## 3         23.6 medium               
    ## 4         14.9 small                
    ## 5         22.5 medium               
    ## 6         15.5 small

Because there are a lot many columns, I just selected radius_worst and
radius_worst_category to display the new column.

This helps answer the research question since I can compare the number
of observations in each bin (which represents worsening/larger radius’s)
between each diagnosis.

**Graphing**: Create a graph that has at least two geom layers.

``` r
ggplot(q2_table,aes(radius_worst_category),fill=diagnosis)+
     geom_bar(position = position_dodge(preserve = "single"),aes(fill=diagnosis)) +
     geom_text(stat='count', aes(label=after_stat(count),group=diagnosis), 
               position=position_dodge(.9), hjust=.5,vjust=-1)+
  ylim(0,360)+ 
  labs(x="Size of Largest Radius",
       y= "Frequency",
       title="Frequency of Different Largest Radius Size by Diagnosis")
```

![](Mini-Data-Analysis-Milestone-2_files/figure-gfm/Task%201.2_RQ2-1.png)<!-- -->

I used bar plots because I have counts, and I added the counts above
each bar to make them easier to look at.

This helps answer the research question because I can see how many
observations from each group have each radius size. And I can see people
with benign tumors have mostly small radius’ while people with malignant
tumors have larger, so patients with malignant tumors do seem to have
larger radius’s.

#### Research Question 3: Do patients with more concave points (concave_points_mean) also tend to have more severe concavity (concavity_worse)?

**Summarizing**: Create a categorical variable with 3 or more groups
from an existing numerical variable. You can use this new variable in
the other tasks! *An example: age in years into “child, teen, adult,
senior”.*

I split the range into 3 to make the cutoffs for my categories.

``` r
range(cancer_sample$concavity_worst)
```

    ## [1] 0.000 1.252

``` r
seq(0,1.252,length.out=4)
```

    ## [1] 0.0000000 0.4173333 0.8346667 1.2520000

``` r
q3_table <- cancer_sample %>%
  mutate(concave_worst_category = case_when(concavity_worst < 0.42~ "small",
                                           concavity_worst >= 0.42 & concavity_worst < 0.84 ~ "medium",
                                           .default = "large")) 

head(select(q3_table,concavity_worst,concave_worst_category))
```

    ## # A tibble: 6 × 2
    ##   concavity_worst concave_worst_category
    ##             <dbl> <chr>                 
    ## 1           0.712 medium                
    ## 2           0.242 small                 
    ## 3           0.450 medium                
    ## 4           0.687 medium                
    ## 5           0.4   small                 
    ## 6           0.536 medium

This helps answer the research question in a similar way as the
summarizing for research question 2. With categories instead of just
numeric variables, it is easier to quantify what is worse, and compare
across groups.

**Graphing**: Create a graph that has at least two geom layers.

``` r
ggplot(q3_table,aes(concave_worst_category,concave_points_mean))+
  geom_boxplot(fill="lightblue")+
  geom_jitter(color="green", size=0.4, alpha=0.9)+
  labs(y="Mean concave points",
       x="Worst concavity category",
       title = "Mean concave points by Worst concavity")
```

![](Mini-Data-Analysis-Milestone-2_files/figure-gfm/Task%201.2_RQ3-1.png)<!-- -->

This helps answer the research question because I look at the
distribution of mean concave points by worst concavity category. We do
see with worse concavity, the mean concave points are higher, but the
points do highlight that there aren’t as many points in the worse
categories, and the difference bwteen medium and large is not that big.

#### Research Question 4: Can mean smoothness (smoothness_mean) be used to predict mean texture (texture_mean)?

**Summarizing**: Create a categorical variable with 3 or more groups
from an existing numerical variable. You can use this new variable in
the other tasks! *An example: age in years into “child, teen, adult,
senior”.*

I am creating a categorical variable classifying the mean texture by the
quartiles

``` r
quantile(cancer_sample$texture_mean)
```

    ##    0%   25%   50%   75%  100% 
    ##  9.71 16.17 18.84 21.80 39.28

``` r
q4_table <- cancer_sample %>%
  mutate(texture_mean_quartile = case_when(texture_mean < 16.17~ "Quartile 1",
                                           texture_mean >= 16.17 & texture_mean < 18.84 ~ "Quartile 2",
                                           texture_mean >= 18.84 & texture_mean < 21.8 ~ "Quartile 3",
                                           .default = "Quartile 4")) 
head(select(q4_table,texture_mean,texture_mean_quartile))
```

    ## # A tibble: 6 × 2
    ##   texture_mean texture_mean_quartile
    ##          <dbl> <chr>                
    ## 1         10.4 Quartile 1           
    ## 2         17.8 Quartile 2           
    ## 3         21.2 Quartile 3           
    ## 4         20.4 Quartile 3           
    ## 5         14.3 Quartile 1           
    ## 6         15.7 Quartile 1

Splitting into categories/quartiles helps answer the research question
because having categories instead of just numeric variables makes it
easier to quantify what is larger.

**Graphing**: Create a graph that has at least two geom layers.

``` r
ggplot(cancer_sample,aes(smoothness_mean,texture_mean))+
  geom_point()+
  geom_smooth(method="lm")+
  labs(x="Mean Smoothness",
       y="Mean Texture",
       title = "Scatterplot of Mean Smoothness vs Mean Texture")
```

    ## `geom_smooth()` using formula = 'y ~ x'

![](Mini-Data-Analysis-Milestone-2_files/figure-gfm/Task%201.2_RQ4-1.png)<!-- -->

This plot helps answer the research question because geom_smooth adds
the regression line, and a steeper slope indicates a stronger
relationship, and can indicate if the variables can be used for
prediction. Here we can see the line is mostly flat, indicating mean
smoothness is not a good predictor.

<!----------------------------------------------------------------------------->

### 1.3 (2 points)

Based on the operations that you’ve completed, how much closer are you
to answering your research questions? Think about what aspects of your
research questions remain unclear. Can your research questions be
refined, now that you’ve investigated your data a bit more? Which
research questions are yielding interesting results?

<!------------------------- Write your answer here ---------------------------->

**Research Question 1:** The plot seems to indicate patients with
malignant tumors have more concave points. The summary statistics also
show this, but because the units of measurement are so small, it is a
bit unclear if this is a significant difference. The question could
potentially be better answered if I did a hypothesis test to see if the
difference is significant

**Research Question 2:** From the bar chart, it is pretty clear that
patients with malignant tumors tend to have worse concavity. The
majority of benign patients have small concavity, while malignant
patients have most medium, and only malignant patients had any large
concavity. I think this question is mostly answered.

**Research Question 3:** There does seem to be a positive association
between worst concavity and mean concave points. The boxplots showed
that as the category went from small to large, the mean number of points
also went up, though this difference is small between large and medium,
potentially due to lack of data. I think this question is mostly
answered.

**Research Question 4:** The plot seems to indicate smoothness mean is
not a good predictor because the regression/smooth line is fairly flat,
but this has not been formally tested, so I can’t be sure. Actually
fitting a model and doing a test on the regression coefficients may help
fully answer this question.
<!----------------------------------------------------------------------------->

# Task 2: Tidy your data

In this task, we will do several exercises to reshape our data. The goal
here is to understand how to do this reshaping with the `tidyr` package.

A reminder of the definition of *tidy* data:

- Each row is an **observation**
- Each column is a **variable**
- Each cell is a **value**

### 2.1 (2 points)

Based on the definition above, can you identify if your data is tidy or
untidy? Go through all your columns, or if you have \>8 variables, just
pick 8, and explain whether the data is untidy or tidy.

<!--------------------------- Start your work below --------------------------->

Since my dataset has many columns, I will use the first 8 columns

``` r
cancer_subset<- cancer_sample[,1:8]
cancer_subset
```

    ## # A tibble: 569 × 8
    ##          ID diagnosis radius_mean texture_mean perimeter_mean area_mean
    ##       <dbl> <chr>           <dbl>        <dbl>          <dbl>     <dbl>
    ##  1   842302 M                18.0         10.4          123.      1001 
    ##  2   842517 M                20.6         17.8          133.      1326 
    ##  3 84300903 M                19.7         21.2          130       1203 
    ##  4 84348301 M                11.4         20.4           77.6      386.
    ##  5 84358402 M                20.3         14.3          135.      1297 
    ##  6   843786 M                12.4         15.7           82.6      477.
    ##  7   844359 M                18.2         20.0          120.      1040 
    ##  8 84458202 M                13.7         20.8           90.2      578.
    ##  9   844981 M                13           21.8           87.5      520.
    ## 10 84501001 M                12.5         24.0           84.0      476.
    ## # ℹ 559 more rows
    ## # ℹ 2 more variables: smoothness_mean <dbl>, compactness_mean <dbl>

If each row is an observation, there should be no repeat ID’s

``` r
length(unique(cancer_subset$ID)) == nrow(cancer_subset)
```

    ## [1] TRUE

The number of unique patient ID’s is equal to the number of rows, so
each row is a unique observation/patient.

Next, I need to check if every column is a variable

``` r
names(cancer_subset)
```

    ## [1] "ID"               "diagnosis"        "radius_mean"      "texture_mean"    
    ## [5] "perimeter_mean"   "area_mean"        "smoothness_mean"  "compactness_mean"

``` r
length(names(cancer_subset)) == ncol(cancer_subset)
```

    ## [1] TRUE

There are 8 unique variables, and 8 columns, so there are no repeats and
each column is a unique variable.

Each cell is a single measurement because there is a single value in
each cell, which represents the measurement for a given variable for one
patient

Given all these things, the data (or at least the first 8 columns) is
tidy

<!----------------------------------------------------------------------------->

### 2.2 (4 points)

Now, if your data is tidy, untidy it! Then, tidy it back to it’s
original state.

If your data is untidy, then tidy it! Then, untidy it back to it’s
original state.

Be sure to explain your reasoning for this task. Show us the “before”
and “after”.

<!--------------------------- Start your work below --------------------------->

Since my data is tidy, I will make it untidy with pivot_longer. The
variables can be split into 3 categories, mean, se or worst, so I will
pivot all variables concerning mean values into 2 columns, 1 listing the
variable name, and 1 listing the value of the variable. I will repeat
this with all variables concerning se values and all variables
concerning worst values together.

``` r
cancer_untidy <- cancer_sample %>% 
  # Pivot the mean variables
  pivot_longer(cols = radius_mean:fractal_dimension_mean,
               names_to = "mean variable",
               values_to = "mean value") %>% 
  # Pivot the se variables
  pivot_longer(cols = radius_se:fractal_dimension_se,
               names_to = "sd variable",
               values_to = "sd value") %>% 
  # Pivot the worst variables
  pivot_longer(cols = radius_worst:fractal_dimension_worst,
               names_to = "worst variable",
               values_to = "worst value")

cancer_untidy
```

    ## # A tibble: 569,000 × 8
    ##        ID diagnosis `mean variable` `mean value` `sd variable` `sd value`
    ##     <dbl> <chr>     <chr>                  <dbl> <chr>              <dbl>
    ##  1 842302 M         radius_mean             18.0 radius_se           1.10
    ##  2 842302 M         radius_mean             18.0 radius_se           1.10
    ##  3 842302 M         radius_mean             18.0 radius_se           1.10
    ##  4 842302 M         radius_mean             18.0 radius_se           1.10
    ##  5 842302 M         radius_mean             18.0 radius_se           1.10
    ##  6 842302 M         radius_mean             18.0 radius_se           1.10
    ##  7 842302 M         radius_mean             18.0 radius_se           1.10
    ##  8 842302 M         radius_mean             18.0 radius_se           1.10
    ##  9 842302 M         radius_mean             18.0 radius_se           1.10
    ## 10 842302 M         radius_mean             18.0 radius_se           1.10
    ## # ℹ 568,990 more rows
    ## # ℹ 2 more variables: `worst variable` <chr>, `worst value` <dbl>

Re-tidy the data back into it’s original form. This is achieved by using
pivot_wider where I previously did pivot_longer. All the variables
stored in the “mean variable” column become their own column, and this
is repeated for the se and worst variables.

``` r
cancer_tidy <- cancer_untidy %>% 
  # Pivot the mean variables
  pivot_wider(names_from = "mean variable",
              values_from = "mean value") %>% 
  # Pivot the se variables
  pivot_wider(names_from = "sd variable",
              values_from = "sd value") %>% 
  # Pivot the worst variables
  pivot_wider(names_from = "worst variable",
              values_from = "worst value") 

cancer_tidy
```

    ## # A tibble: 569 × 32
    ##          ID diagnosis radius_mean texture_mean perimeter_mean area_mean
    ##       <dbl> <chr>           <dbl>        <dbl>          <dbl>     <dbl>
    ##  1   842302 M                18.0         10.4          123.      1001 
    ##  2   842517 M                20.6         17.8          133.      1326 
    ##  3 84300903 M                19.7         21.2          130       1203 
    ##  4 84348301 M                11.4         20.4           77.6      386.
    ##  5 84358402 M                20.3         14.3          135.      1297 
    ##  6   843786 M                12.4         15.7           82.6      477.
    ##  7   844359 M                18.2         20.0          120.      1040 
    ##  8 84458202 M                13.7         20.8           90.2      578.
    ##  9   844981 M                13           21.8           87.5      520.
    ## 10 84501001 M                12.5         24.0           84.0      476.
    ## # ℹ 559 more rows
    ## # ℹ 26 more variables: smoothness_mean <dbl>, compactness_mean <dbl>,
    ## #   concavity_mean <dbl>, concave_points_mean <dbl>, symmetry_mean <dbl>,
    ## #   fractal_dimension_mean <dbl>, radius_se <dbl>, texture_se <dbl>,
    ## #   perimeter_se <dbl>, area_se <dbl>, smoothness_se <dbl>,
    ## #   compactness_se <dbl>, concavity_se <dbl>, concave_points_se <dbl>,
    ## #   symmetry_se <dbl>, fractal_dimension_se <dbl>, radius_worst <dbl>, …

<!----------------------------------------------------------------------------->

### 2.3 (4 points)

Now, you should be more familiar with your data, and also have made
progress in answering your research questions. Based on your interest,
and your analyses, pick 2 of the 4 research questions to continue your
analysis in the remaining tasks:

<!-------------------------- Start your work below ---------------------------->

1.  Does the mean number of concave points (concave_points_mean) differ
    between patients diagnosed with a malignant (M) or benign (B) tumor?

2.  Do patients with more concave points (concave_points_mean) also tend
    to have more severe concavity (concavity_worse)?

<!----------------------------------------------------------------------------->

Explain your decision for choosing the above two research questions.

<!--------------------------- Start your work below --------------------------->

For the first research question, I think it is interesting because it
relates to the difference between patients with benign and malignant
tumors. And while I think there has been progress answer it, I think a
formal hypothesis test or more analyses would help.

For the second research question, now that I can see there is a positive
relationship, I think I could explore it more, possibly by fitting a
regression model and seeing how well it fits.
<!----------------------------------------------------------------------------->

Now, try to choose a version of your data that you think will be
appropriate to answer these 2 questions. Use between 4 and 8 functions
that we’ve covered so far (i.e. by filtering, cleaning, tidy’ing,
dropping irrelevant columns, etc.).

(If it makes more sense, then you can make/pick two versions of your
data, one for each research question.)

<!--------------------------- Start your work below --------------------------->

Since there are overlapping variables, I am going to create one dataset.

I am selecting the relevant columns, removing any NA’s if there are any,
arranging into increasing order and remaking the categorical variable I
made in Task 1.2 in case I want to use it.

``` r
rq_data <- cancer_sample %>% 
  select(ID, diagnosis,concave_points_mean,concavity_worst) %>% 
  filter(!is.na(concave_points_mean) & !is.na(concavity_worst)) %>% 
  arrange(concave_points_mean) %>% 
  mutate(concave_worst_category = case_when(concavity_worst < 0.45~ "small",
                                           concavity_worst >= 0.45 & concavity_worst < 0.9 ~ "medium",
                                           .default = "large")) 
rq_data
```

    ## # A tibble: 569 × 5
    ##         ID diagnosis concave_points_mean concavity_worst concave_worst_category
    ##      <dbl> <chr>                   <dbl>           <dbl> <chr>                 
    ##  1  862722 B                           0               0 small                 
    ##  2  868999 B                           0               0 small                 
    ##  3  871642 B                           0               0 small                 
    ##  4  872113 B                           0               0 small                 
    ##  5  875099 B                           0               0 small                 
    ##  6  894047 B                           0               0 small                 
    ##  7  903483 B                           0               0 small                 
    ##  8 9113846 B                           0               0 small                 
    ##  9  921092 B                           0               0 small                 
    ## 10  923748 B                           0               0 small                 
    ## # ℹ 559 more rows

``` r
glimpse(rq_data)
```

    ## Rows: 569
    ## Columns: 5
    ## $ ID                     <dbl> 862722, 868999, 871642, 872113, 875099, 894047,…
    ## $ diagnosis              <chr> "B", "B", "B", "B", "B", "B", "B", "B", "B", "B…
    ## $ concave_points_mean    <dbl> 0.000000, 0.000000, 0.000000, 0.000000, 0.00000…
    ## $ concavity_worst        <dbl> 0.000000, 0.000000, 0.000000, 0.000000, 0.00000…
    ## $ concave_worst_category <chr> "small", "small", "small", "small", "small", "s…

<!----------------------------------------------------------------------------->

# Task 3: Modelling

## 3.0 (no points)

Pick a research question from 1.2, and pick a variable of interest
(we’ll call it “Y”) that’s relevant to the research question. Indicate
these.

<!-------------------------- Start your work below ---------------------------->

**Research Question**: Do patients with more concave points
(concave_points_mean) also tend to have more severe concavity
(concavity_worse)?

**Variable of interest**: Mean concavity (concavity_worst)

<!----------------------------------------------------------------------------->

## 3.1 (3 points)

Fit a model or run a hypothesis test that provides insight on this
variable with respect to the research question. Store the model object
as a variable, and print its output to screen. We’ll omit having to
justify your choice, because we don’t expect you to know about model
specifics in STAT 545.

- **Note**: It’s OK if you don’t know how these models/tests work. Here
  are some examples of things you can do here, but the sky’s the limit.

  - You could fit a model that makes predictions on Y using another
    variable, by using the `lm()` function.
  - You could test whether the mean of Y equals 0 using `t.test()`, or
    maybe the mean across two groups are different using `t.test()`, or
    maybe the mean across multiple groups are different using `anova()`
    (you may have to pivot your data for the latter two).
  - You could use `lm()` to test for significance of regression
    coefficients.

<!-------------------------- Start your work below ---------------------------->

I am fitting a linear model with worst concavity as the response and
mean concave points as the predictor. I am going to test for the
significance of the regression coefficients, which will tell if
concave_points_mean is a good predictor for concavity_worse.

``` r
mod.fit <- lm(concavity_worst ~ concave_points_mean, data= rq_data)
mod.fit
```

    ## 
    ## Call:
    ## lm(formula = concavity_worst ~ concave_points_mean, data = rq_data)
    ## 
    ## Coefficients:
    ##         (Intercept)  concave_points_mean  
    ##              0.0743               4.0453

<!----------------------------------------------------------------------------->

## 3.2 (3 points)

Produce something relevant from your fitted model: either predictions on
Y, or a single value like a regression coefficient or a p-value.

- Be sure to indicate in writing what you chose to produce.
- Your code should either output a tibble (in which case you should
  indicate the column that contains the thing you’re looking for), or
  the thing you’re looking for itself.
- Obtain your results using the `broom` package if possible. If your
  model is not compatible with the broom function you’re needing, then
  you can obtain your results by some other means, but first indicate
  which broom function is not compatible.

<!-------------------------- Start your work below ---------------------------->

I am going to use the tidy() function from the broom package to display
the regression coefficients, which includes the estimate of the
coefficient, the standard error, the test statistic and p-value. The
main thing I am looking for is the estimate of the regression
coefficient, which is in the column “estimate” and next to
“concave_points_mean” since I am only interested in the regression
coefficient for that variable.

``` r
broom::tidy(mod.fit)
```

    ## # A tibble: 2 × 5
    ##   term                estimate std.error statistic   p.value
    ##   <chr>                  <dbl>     <dbl>     <dbl>     <dbl>
    ## 1 (Intercept)           0.0743   0.00928      8.00 6.89e- 15
    ## 2 concave_points_mean   4.05     0.149       27.2  7.02e-105

We can see the coefficient for concave_points_mean is about 4, with a
very small p-value, meaning it does significantly predict
concavity_worst.

<!----------------------------------------------------------------------------->

# Task 4: Reading and writing data

Get set up for this exercise by making a folder called `output` in the
top level of your project folder / repository. You’ll be saving things
there.

## 4.1 (3 points)

Take a summary table that you made from Task 1, and write it as a csv
file in your `output` folder. Use the `here::here()` function.

- **Robustness criteria**: You should be able to move your Mini Project
  repository / project folder to some other location on your computer,
  or move this very Rmd file to another location within your project
  repository / folder, and your code should still work.
- **Reproducibility criteria**: You should be able to delete the csv
  file, and remake it simply by knitting this Rmd file.

<!-------------------------- Start your work below ---------------------------->

I am going to turn the summary table from Task 1.2, Research Question 3
into a csv. The table created a categorical variable from
concavity_worst

``` r
library(here)
write_csv(q3_table,here::here("output","q3_table.csv"))
```

<!----------------------------------------------------------------------------->

## 4.2 (3 points)

Write your model object from Task 3 to an R binary file (an RDS), and
load it again. Be sure to save the binary file in your `output` folder.
Use the functions `saveRDS()` and `readRDS()`.

- The same robustness and reproducibility criteria as in 4.1 apply here.

<!-------------------------- Start your work below ---------------------------->

Save the mod.fit object form Task 3 into an RDS file called “model.rds”

``` r
saveRDS(mod.fit,file=here("output","model.rds"))
```

Read in the RDS file we just created

``` r
readRDS(file=here("output","model.rds"))
```

    ## 
    ## Call:
    ## lm(formula = concavity_worst ~ concave_points_mean, data = rq_data)
    ## 
    ## Coefficients:
    ##         (Intercept)  concave_points_mean  
    ##              0.0743               4.0453

<!----------------------------------------------------------------------------->

# Overall Reproducibility/Cleanliness/Coherence Checklist

Here are the criteria we’re looking for.

## Coherence (0.5 points)

The document should read sensibly from top to bottom, with no major
continuity errors.

The README file should still satisfy the criteria from the last
milestone, i.e. it has been updated to match the changes to the
repository made in this milestone.

## File and folder structure (1 points)

You should have at least three folders in the top level of your
repository: one for each milestone, and one output folder. If there are
any other folders, these are explained in the main README.

Each milestone document is contained in its respective folder, and
nowhere else.

Every level-1 folder (that is, the ones stored in the top level, like
“Milestone1” and “output”) has a `README` file, explaining in a sentence
or two what is in the folder, in plain language (it’s enough to say
something like “This folder contains the source for Milestone 1”).

## Output (1 point)

All output is recent and relevant:

- All Rmd files have been `knit`ted to their output md files.
- All knitted md files are viewable without errors on Github. Examples
  of errors: Missing plots, “Sorry about that, but we can’t show files
  that are this big right now” messages, error messages from broken R
  code
- All of these output files are up-to-date – that is, they haven’t
  fallen behind after the source (Rmd) files have been updated.
- There should be no relic output files. For example, if you were
  knitting an Rmd to html, but then changed the output to be only a
  markdown file, then the html file is a relic and should be deleted.

Our recommendation: delete all output files, and re-knit each
milestone’s Rmd file, so that everything is up to date and relevant.

## Tagged release (0.5 point)

You’ve tagged a release for Milestone 2.

### Attribution

Thanks to Victor Yuan for mostly putting this together.
