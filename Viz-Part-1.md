Viz Part 1
================
Zilin Huang
2023-09-28

``` r
library(ggridges)
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.3     ✔ readr     2.1.4
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.0
    ## ✔ ggplot2   3.4.3     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.2     ✔ tidyr     1.3.0
    ## ✔ purrr     1.0.2     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
knitr::opts_chunk$set(
  fig.width = 6,
  fig.asp = .6,
  out.width = "90%"
)
```

``` r
weather_df = 
  rnoaa::meteo_pull_monitors(
    c("USW00094728", "USW00022534", "USS0023B17S"),
    var = c("PRCP", "TMIN", "TMAX"), 
    date_min = "2021-01-01",
    date_max = "2022-12-31") |>
  mutate(
    name = recode(
      id, 
      USW00094728 = "CentralPark_NY", 
      USC00519397 = "Molokai_HI",
      USS0023B17S = "Waterhole_WA"),
    tmin = tmin / 10,
    tmax = tmax / 10) |>
  select(name, id, everything())
```

    ## using cached file: C:\Users\huang\AppData\Local/R/cache/R/rnoaa/noaa_ghcnd/USW00094728.dly

    ## date created (size, mb): 2023-09-28 10:20:36.026108 (8.541)

    ## file min/max dates: 1869-01-01 / 2023-09-30

    ## using cached file: C:\Users\huang\AppData\Local/R/cache/R/rnoaa/noaa_ghcnd/USW00022534.dly

    ## date created (size, mb): 2023-09-28 10:21:35.507133 (3.838)

    ## file min/max dates: 1949-10-01 / 2023-09-30

    ## using cached file: C:\Users\huang\AppData\Local/R/cache/R/rnoaa/noaa_ghcnd/USS0023B17S.dly

    ## date created (size, mb): 2023-09-28 10:20:55.484716 (0.996)

    ## file min/max dates: 1999-09-01 / 2023-09-30

``` r
weather_df |>
  filter(name=="Molokai_HI")|>
  ggplot(aes(x=date,y=tmax))+
  geom_line()+
  geom_point()
```

<img src="Viz-Part-1_files/figure-gfm/unnamed-chunk-3-1.png" width="90%" />

Density plot.

``` r
ggplot(weather_df,aes(x=tmax,fill=name))+
  geom_density(alpha=0.3,adjust=.75)
```

    ## Warning: Removed 17 rows containing non-finite values (`stat_density()`).

<img src="Viz-Part-1_files/figure-gfm/unnamed-chunk-4-1.png" width="90%" />

Boxplots.

``` r
ggplot(weather_df,aes(y=tmax,x=name))+
  geom_boxplot()
```

    ## Warning: Removed 17 rows containing non-finite values (`stat_boxplot()`).

<img src="Viz-Part-1_files/figure-gfm/unnamed-chunk-5-1.png" width="90%" />

Violin plots.

``` r
ggplot(weather_df,aes(y=tmax,x=name))+
  geom_violin()
```

    ## Warning: Removed 17 rows containing non-finite values (`stat_ydensity()`).

<img src="Viz-Part-1_files/figure-gfm/unnamed-chunk-6-1.png" width="90%" />

Ridge plot.

``` r
ggplot(weather_df,aes(x=tmax,y=name))+
  geom_density_ridges()
```

    ## Picking joint bandwidth of 1.54

    ## Warning: Removed 17 rows containing non-finite values
    ## (`stat_density_ridges()`).

<img src="Viz-Part-1_files/figure-gfm/unnamed-chunk-7-1.png" width="90%" />

``` r
  geom_violin()
```

    ## geom_violin: draw_quantiles = NULL, na.rm = FALSE, orientation = NA
    ## stat_ydensity: trim = TRUE, scale = area, na.rm = FALSE, orientation = NA
    ## position_dodge

## saving and embedding plots

``` r
ggp_weather=
  weather_df |>
  ggplot(aes(x=tmin,y=tmax))+
  geom_point()

ggsave("results/ggp_weather.pdf",ggp_weather)
```

    ## Saving 6 x 3.59 in image

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

``` r
ggp_weather
```

    ## Warning: Removed 17 rows containing missing values (`geom_point()`).

<img src="Viz-Part-1_files/figure-gfm/unnamed-chunk-9-1.png" width="90%" />
