Lab 09 - Grading the professor, Pt. 1
================
Benjamin Egan
03/17/25

Here is the link to the assignment page:
<https://datascience4psych.github.io/DataScience4Psych/lab09.html>. This
includes the relevant information for the assignment alongside required
questions I needed to answer.

# Part 1 - Exploratory Data Analysis

``` r
view(evals)

evals %>%
  ggplot(aes(
    x = score
  ))+
  geom_histogram(color = "white")+
  labs(
    x = "Score",
    y = NULL,
    title = "Distribution of Scores",
    subtitle = "Average professor evaluation score: (1) very unsatisfactory - (5) excellent"
  )
```

![](lab-09_files/figure-gfm/score%20distribution-1.png)<!-- -->

``` r
skewness(evals$score)
```

    ## [1] -0.6993665

``` r
median(evals$score)
```

    ## [1] 4.3

``` r
mean(evals$score)
```

    ## [1] 4.17473

This distribution appears to be negatively skewed (-.699), with the
majority of the scores falling above four (median = 4.3, mean = 4.17)

``` r
attractive_score <- evals %>%
  ggplot(aes(
    x = score,
    y = bty_avg
  ))+
  labs(
    x = "Score",
    y = "Beauty rating",
    title = "Relationship between professor rating and attractiveness",
    subtitle = "Average professor evaluation score: (1) very unsatisfactory - (5) excellent"
  )

attractive_score + geom_smooth(se = FALSE, color = "black", span = 1)
```

![](lab-09_files/figure-gfm/rating%20and%20beauty%20relationship-1.png)<!-- -->

Based on this graph, I would say there is a positive relationship
between professor ratings and perceived attractiveness.

### Replication with geom_jitter()

I changed the visualization to use geom_point() instead of
geom_smooth(), which makes using geom_jitter() understandable.

``` r
attractive_score + geom_point() + geom_smooth(se = FALSE, color = "black", span = .5)
```

![](lab-09_files/figure-gfm/updated%20rating%20and%20beauty%20relationship-1.png)<!-- -->

``` r
attractive_score + geom_jitter(width = .05) + geom_smooth(se = FALSE, color = "black", span = .5)
```

![](lab-09_files/figure-gfm/updated%20rating%20and%20beauty%20relationship-2.png)<!-- -->

geom_jitter() can be extremely useful in visualizing the relationship
between a continuous variable and a discrete variable (i.e. a likert
scale). The scatterplot utilizing geom_point() can be visually
misleading, as you cannot tell if data overlap.

## Linear Regression with a numerical predictor

``` r
m_bty <- lm(score ~ bty_avg, data = evals)

summary(m_bty)
```

    ## 
    ## Call:
    ## lm(formula = score ~ bty_avg, data = evals)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -1.9246 -0.3690  0.1420  0.3977  0.9309 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  3.88034    0.07614   50.96  < 2e-16 ***
    ## bty_avg      0.06664    0.01629    4.09 5.08e-05 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.5348 on 461 degrees of freedom
    ## Multiple R-squared:  0.03502,    Adjusted R-squared:  0.03293 
    ## F-statistic: 16.73 on 1 and 461 DF,  p-value: 5.083e-05

``` r
attractive_score + 
  geom_jitter(width = .05, alpha = .6)+ 
  geom_smooth(se = FALSE, color = "blue", span = .5)+
  geom_abline(intercept = 3.88034, slope = 0.06664, color ="orange", size = 1)
```

    ## Warning: Using `size` aesthetic for lines was deprecated in ggplot2 3.4.0.
    ## ℹ Please use `linewidth` instead.
    ## This warning is displayed once every 8 hours.
    ## Call `lifecycle::last_lifecycle_warnings()` to see where this warning was
    ## generated.

![](lab-09_files/figure-gfm/replot%20with%20regression-1.png)<!-- -->

The regression equation should be y = 3.88 + .067x, where y is score and
x = average beauty rating.

This line tells us for every 1 unit increase in beauty rating, there is
a .067 increase in instructor score. In other words, a change in one
variable has little impact in the other. The intercept of 3.88 means
that an instructor with a beauty score of “0” would have an average
instructor score of 3.88. This means that someone perceived as
unattractive will still perform above average.

R squared is .033, an indication of the degree to which individuals
scores on one variable can predict the scores on the other. In this
case, average beauty of instructors can only account for about 3% of the
variance explained by instructor scores. I would summarize this as
“beauty doesn’t affect evaluation.”

## Linear Regression with a categorical predictor

``` r
m_gen <- lm(score ~ gender, data = evals)

summary(m_gen)
```

    ## 
    ## Call:
    ## lm(formula = score ~ gender, data = evals)
    ## 
    ## Residuals:
    ##      Min       1Q   Median       3Q      Max 
    ## -1.83433 -0.36357  0.06567  0.40718  0.90718 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)  4.09282    0.03867 105.852  < 2e-16 ***
    ## gendermale   0.14151    0.05082   2.784  0.00558 ** 
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.5399 on 461 degrees of freedom
    ## Multiple R-squared:  0.01654,    Adjusted R-squared:  0.01441 
    ## F-statistic: 7.753 on 1 and 461 DF,  p-value: 0.005583

The regression equation should be y = 4.09 + .142x, where y is score and
x is gender.

``` r
gender_score <- evals %>%
  ggplot(aes(
    x = score,
    y = gender
  ))+
  labs(
    x = "Score",
    y = "Gender",
    title = "Relationship between professor rating and Gender",
    subtitle = "Average professor evaluation score: (1) very unsatisfactory - (5) excellent"
  )

gender_score + geom_point()
```

![](lab-09_files/figure-gfm/graph%20gender%20regression-1.png)<!-- -->

With a quick graph using gender as the y variable, it looks like female
is being coded as less than a male, indicating that an increase in X is
associated with going from female to male. yfemale = 4.09, ymale = 5.51

``` r
m_rank <- lm(score ~ rank, data = evals)

m_rank
```

    ## 
    ## Call:
    ## lm(formula = score ~ rank, data = evals)
    ## 
    ## Coefficients:
    ##      (Intercept)  ranktenure track       ranktenured  
    ##           4.2843           -0.1297           -0.1452

``` r
summary(m_rank)
```

    ## 
    ## Call:
    ## lm(formula = score ~ rank, data = evals)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -1.8546 -0.3391  0.1157  0.4305  0.8609 
    ## 
    ## Coefficients:
    ##                  Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)       4.28431    0.05365  79.853   <2e-16 ***
    ## ranktenure track -0.12968    0.07482  -1.733   0.0837 .  
    ## ranktenured      -0.14518    0.06355  -2.284   0.0228 *  
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.5419 on 460 degrees of freedom
    ## Multiple R-squared:  0.01163,    Adjusted R-squared:  0.007332 
    ## F-statistic: 2.706 on 2 and 460 DF,  p-value: 0.06786

Regression equations: y = 4.28 - .130x, where y is score and x is
signifying either teaching or tenure track. y = 4.28 - .145x, where y is
score and x is signifying either teaching or tenure.

``` r
evals$rank_relevel <- relevel(evals$rank, ref = "tenure track")

m_rank_relevel <- lm(score ~ rank_relevel, data = evals)

m_rank_relevel
```

    ## 
    ## Call:
    ## lm(formula = score ~ rank_relevel, data = evals)
    ## 
    ## Coefficients:
    ##          (Intercept)  rank_relevelteaching   rank_releveltenured  
    ##               4.1546                0.1297               -0.0155

``` r
summary(m_rank_relevel)
```

    ## 
    ## Call:
    ## lm(formula = score ~ rank_relevel, data = evals)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -1.8546 -0.3391  0.1157  0.4305  0.8609 
    ## 
    ## Coefficients:
    ##                      Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)           4.15463    0.05214  79.680   <2e-16 ***
    ## rank_relevelteaching  0.12968    0.07482   1.733   0.0837 .  
    ## rank_releveltenured  -0.01550    0.06228  -0.249   0.8036    
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 0.5419 on 460 degrees of freedom
    ## Multiple R-squared:  0.01163,    Adjusted R-squared:  0.007332 
    ## F-statistic: 2.706 on 2 and 460 DF,  p-value: 0.06786

``` r
evals %>%
  ggplot(aes(
    x = score,
    y = rank
  ))+
  labs(
    x = "Score",
    y = "rank",
    title = "Relationship between professor rating and rank",
    subtitle = "Average professor evaluation score: (1) very unsatisfactory - (5) excellent"
  )+
  geom_jitter()
```

![](lab-09_files/figure-gfm/graph%20rank-1.png)<!-- -->

picture google to find cool stuff (digital cv, etc.) create website
includes all my work

## Hint

For Exercise 12, the `relevel()` function can be helpful!
