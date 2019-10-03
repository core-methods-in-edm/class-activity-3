class activity 3
================
Minruo Wang
10/2/2019

-   [Learn](#learn)
-   [Exercise](#exercise)

### Learn

#### Useful links

[ggplot2 website](https://ggplot2.tidyverse.org/reference/)
[R Graphic Cookbook](http://www.cookbook-r.com/Graphs/)

#### Mapping aesthetic to data to = layer

``` r
# install.packages("ggplot2")
library(ggplot2)

ggplot(diamonds, aes(x = price, y = carat)) +
  geom_point()
```

![](class-activity-3_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-1-1.png)

#### Two layers

``` r
ggplot(mpg, aes(reorder(class, hwy), hwy)) +
  geom_jitter() + # adds a small amount of random variation to the location of each point
  geom_boxplot()
```

![](class-activity-3_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-2-1.png)

``` r
#Plot count
ggplot(diamonds, aes(depth)) +
  geom_histogram(aes(y = ..count..), binwidth=0.2) +
  facet_wrap(~ cut) + xlim(50, 70)
```

    ## Warning: Removed 26 rows containing non-finite values (stat_bin).

    ## Warning: Removed 10 rows containing missing values (geom_bar).

![](class-activity-3_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-3-1.png)

``` r
#Plot density
ggplot(diamonds, aes(depth)) +
  geom_histogram(aes(y = ..density..), binwidth=0.2) +
  facet_wrap(~ cut) + xlim(50, 70)
```

    ## Warning: Removed 26 rows containing non-finite values (stat_bin).

    ## Warning: Removed 10 rows containing missing values (geom_bar).

![](class-activity-3_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-3-2.png)

``` r
ggplot(mpg, aes(displ, hwy, color = class)) +
  geom_point()
```

![](class-activity-3_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-4-1.png)

### Exercise

##### 1. Can you create a line graph using the "economics\_long" data set that shows change over time in "value01" for different categories of "variable"?

``` r
# show in one graph
ggplot(economics_long, aes(date, value01, color = variable)) +
  geom_line()
```

![](class-activity-3_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-5-1.png)

``` r
# show in facet
ggplot(economics_long, aes(x = date, y = value01, color = variable)) +
  geom_line() +
  facet_wrap(~ variable)
```

![](class-activity-3_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-6-1.png)

##### 2. If you would like to recreate the Minard graphic of Napoleon's Troops the code is below and the data is in this repo.

``` r
# load and merge the data
cities <- read.table("cities.txt", header = TRUE, sep = "")
troops <- read.table("troops.txt", header = TRUE, sep = "")
temps <- read.table("temps.txt", header = TRUE, sep = "")
Napo <- merge(cities, troops, by = c("long", "lat"), all = TRUE)
Napoleon <- merge(Napo, temps, by = "long", all = TRUE)
```

``` r
# Napoleon's troops route
route <- ggplot(Napoleon, aes(long, lat)) +
  geom_path(aes(size = survivors, colour = direction,
                group = interaction(group, direction)), 
                data = troops) +
  geom_text(aes(label = city), hjust = 0, vjust = 1, size = 4) +  
  scale_x_continuous("", limits = c(24, 39)) +
  scale_y_continuous("") +
  scale_colour_manual(values = c("grey50","red")) +
  scale_size(range = c(1, 10))
# show the graph
route
```

    ## Warning: Removed 43 rows containing missing values (geom_text).

![](class-activity-3_files/figure-markdown_github-ascii_identifiers/unnamed-chunk-8-1.png)

``` r
# save the graph using ggsave
ggsave(route, file = "Napoleon-route.png")
```

    ## Saving 7 x 5 in image

    ## Warning: Removed 43 rows containing missing values (geom_text).
