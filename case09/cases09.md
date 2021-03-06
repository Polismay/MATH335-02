---
title: "Its about time"
author: "Polisma Yadav"
date: "December 07, 2021"
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
dat <- read_csv("https://byuistats.github.io/M335/data/sales.csv") %>% 
  with_tz("America/Denver")
```

## Background

We have transaction data for a few businesses that have been in operation for three months. Each of these companies has come to your investment company for a loan to expand their business. Your boss has asked you to go through the transactions for each business and provide daily, weekly, and monthly gross revenue summaries and comparisons. Your boss would like a short write-up with tables and visualizations that help with the decision of which company did the best over the three-month period. You will also need to provide a short paragraph with your recommendation after building your analysis.

This course only looks at understanding and visualizing recorded time series data. If you would like to learn more about forecasting I would recommend Forecasting: Principles and Practice (Links to an external site.) and for a quick introduction read Exploring and Visualizing Time Series.

## Data Wrangling and Visualization


### Daily Totals

```r
dat %>% 
  mutate(Day = day(Time)) %>% 
  group_by(Name, Day, Type) %>% 
  summarize(Amount = sum(Amount)) %>% 
  ggplot(aes(x = Day, y = Amount, color = Name)) +
  geom_line(alpha = .4) +
  geom_point(size = 1) +
  labs(x = "Day of the Year", y = "Total Sales ($)",
       Title = "Sales Per Day") +
  facet_wrap(~Name, ncol = 1) +
  theme(legend.position = "none")
```

![](cases09_files/figure-html/unnamed-chunk-2-1.png)<!-- -->



### Weekly Totals

```r
dat %>% 
  mutate(Week = week(Time)) %>% 
  group_by(Name, Week, Type) %>% 
  summarize(Amount = sum(Amount)) %>% 
  ggplot(aes(x = Week, y = Amount, color = Name)) +
  geom_line(alpha = .4) +
  geom_point(size = 1) +
  labs(x = "Week of the Year", y = "Total Sales ($)",
       Title = "Sales Per Week") +
  geom_text(aes(label = Amount),hjust = 1.1, vjust = 1, size = 2.5) +
  facet_wrap(~Name, ncol = 1) +
  theme(legend.position = "none")
```

![](cases09_files/figure-html/unnamed-chunk-3-1.png)<!-- -->

### Monthly rates

```r
dat %>% 
  mutate(Month = month(Time)) %>% 
  group_by(Name, Month, Type) %>% 
  summarize(Amount = sum(Amount)) %>% 
  ggplot(aes(x = Month, y = Amount, color = Name)) +
  geom_line(alpha = .4) +
  geom_point(size = 1) +
  geom_text(aes(label = Amount),hjust = 1.1, vjust = 1, size = 2.5) +
  labs(x = "Month of the Year", y = "Total Sales ($)",
       Title = "Sales Per Month") +
  facet_wrap(~Name, ncol = 1) +
  theme(legend.position = "none")
```

![](cases09_files/figure-html/unnamed-chunk-4-1.png)<!-- -->


### Hourly Totals

```r
dat %>% 
  mutate(Hour = floor_date(Time, unit = "hour") %>% hour()) %>% 
  group_by(Name, Hour, Type) %>% 
  summarise(Amount = sum(Amount)) %>% 
  ggplot(aes(x = Hour, y = Amount, color = Name)) +
  geom_line(alpha = 0.4) +
  geom_point(size = 1) +
  labs(x = "Hour of Day", y = "Total Sales ($)",
       Title = "Sales Per Hour") +
  facet_wrap(~Type, ncol = 1) +
  coord_cartesian(xlim = c(7, 24)) +
  facet_wrap(~Name, ncol = 1) +
  theme(legend.position = "none")
```

![](cases09_files/figure-html/unnamed-chunk-5-1.png)<!-- -->

## Conclusions

I have four plots representing daily, weekly, monthly and hourly gross revenue summaries and comparisons. From monthly graph we can clearly see that HotDiggity and LeBelle performed the best and I recomment these two businesses. 
