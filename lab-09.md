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

*Provide your answer here.*  
Add code chunks as needed.

``` r
# Add your R code here
```

## Additional Exercises

*Repeat the format above for additional exercises.*

## Hint

For Exercise 12, the `relevel()` function can be helpful!
