---
title: "Give Your Visualization Wings to Fly"
author: "Polisma Yadav"
date: "December 13, 2021"
output:
  html_document:  
    keep_md: true
    toc: true
    toc_float: true
    code_folding: hide
    fig_height: 6
    fig_width: 12
    fig_align: 'center'
---






```r
# Use this R-Chunk to import all your datasets!
flights <- nycflights13::flights
flights <- na.omit(flights)
```

## Background

You just started your internship at a big firm in New York, and your manager gave you an extensive file of flights that departed JFK, LGA, or EWR in 2013. From this data (nycflights13::flights), which you can obtain in R (install.packages("nycflights13"); library(nycflights13)), your manager wants you to answer the following questions:

1. For each origin airport (JFK, EWR, LGA), which airline has the lowest 75th percentile of delay time for flights scheduled to leave earlier than noon?
2. Which origin airport is best to minimize my chances of a late arrival when I am using Delta Airlines?
3. Which destination airport is the worst (you decide on the metric for worst) for arrival delays?

## Data Wrangling and Visualization

### Question1: For each origin airport (JFK, EWR, LGA), which airline has the lowest 75th percentile of delay time for flights scheduled to leave earlier than noon?

```r
# Use this R-Chunk to clean & wrangle your data!
flights %>% 
  filter(sched_dep_time < 1200) %>% 
  ggplot(aes(x = carrier, y = dep_delay)) +
  facet_grid(~origin) +
  geom_boxplot() +
  coord_cartesian(ylim = c(-20, 20)) +
  labs(x = "Carrier name", y = "Departure Delay")
```

![](case3_files/figure-html/unnamed-chunk-2-1.png)<!-- -->

### Question2: Which origin airport is best to minimize my chances of a late arrival when I am using Delta Airlines?


```r
# Use this R-Chunk to plot & visualize your data!
delta <- flights %>% 
  subset(carrier == "DL") %>% 
  mutate(delay_pos = dep_delay > 0) %>% 
  subset(delay_pos) %>% 
  group_by(origin) %>% 
  summarise(delay = mean(dep_delay))

delta %>% 
  ggplot(aes(origin, delay)) +
  geom_col() +
  labs(title = "Delta Airlines", x = "Origin Airport", y = "Average Arrival Delay")
```

![](case3_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

## Conclusions
Plot 1:
From the plot we can see that JFK has the lowest 75th percentile of departure delays per carrier. i.e. JFK has the lowest 75th percentile of delay time for flights scheduled to leave earlier than noon

Plot 2:
And again JFK is the best origin airport to minimize the chances of a late arrival when using Delta Airlines.
