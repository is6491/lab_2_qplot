Lab 2 - Intro to ggplot, Using qplot
================
Jeremy Morris
11/2/22

# Introduction to ggplot2 - part 1

### About ggplot2

> “ggplot2 is an R package for producing statistical, or data, graphics,
> but it is unlike most other graphics packages because it has a deep
> underlying grammar. This grammar, based on the Grammar of Graphics
> (Wilkinson, 2005), is composed of a set of independent components that
> can be composed in many different ways. \[..\] Plots can be built up
> iteratively and edited later. A carefully chosen set of defaults means
> that most of the time you can produce a publication-quality graphic in
> seconds, but if you do have special formatting requirements, a
> comprehensive theming system makes it easy to do what you want. \[..\]
> ggplot2 is designed to work in a layered fashion, starting with a
> layer showing the raw data then adding layers of annotation and
> statistical summaries. \[..\]”

H.Wickham, ggplot2, Use R, DOI 10.1007/978-0-387-98141_1, © Springer
Science+Business Media, LLC 2009

# Introduction to ggplot2 - focus on qplot

The `ggplot2` library is very flexible and easy to use once you have
learned the basics of how it works. There are many ways to put together
a visualization using the `ggplot` function which we will cover in the
next lab. In this lab, we introduce the `qplot` function which is built
to mimic the base R `plot` function. Like the base plot function, there
are limits to what the `qplot` function will do. We will focus on
creating visualizations with different chart types and adjusting various
options to customize those visualizations. At the end of the lab we will
show where the `qplot` function starts to produce undesired output.

From the qplot help file:

> qplot is a shortcut designed to be familiar if you’re used to base
> plot(). It’s a convenient wrapper for creating a number of different
> types of plots using a consistent calling scheme. It’s great for
> allowing you to produce plots quickly, but I highly recommend learning
> ggplot() as it makes it easier to create complex graphics.

We’ll work with two datasets:

1.  diamonds: data regarding diamond quality
2.  mtcars: data regarding cars and fuel efficiency

### diamonds

``` r
as_tibble(diamonds)
```

    # A tibble: 53,940 × 10
       carat cut       color clarity depth table price     x     y     z
       <dbl> <ord>     <ord> <ord>   <dbl> <dbl> <int> <dbl> <dbl> <dbl>
     1  0.23 Ideal     E     SI2      61.5    55   326  3.95  3.98  2.43
     2  0.21 Premium   E     SI1      59.8    61   326  3.89  3.84  2.31
     3  0.23 Good      E     VS1      56.9    65   327  4.05  4.07  2.31
     4  0.29 Premium   I     VS2      62.4    58   334  4.2   4.23  2.63
     5  0.31 Good      J     SI2      63.3    58   335  4.34  4.35  2.75
     6  0.24 Very Good J     VVS2     62.8    57   336  3.94  3.96  2.48
     7  0.24 Very Good I     VVS1     62.3    57   336  3.95  3.98  2.47
     8  0.26 Very Good H     SI1      61.9    55   337  4.07  4.11  2.53
     9  0.22 Fair      E     VS2      65.1    61   337  3.87  3.78  2.49
    10  0.23 Very Good H     VS1      59.4    61   338  4     4.05  2.39
    # … with 53,930 more rows

### mtcars

``` r
as_tibble(mtcars)
```

    # A tibble: 32 × 11
         mpg   cyl  disp    hp  drat    wt  qsec    vs    am  gear  carb
       <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl> <dbl>
     1  21       6  160    110  3.9   2.62  16.5     0     1     4     4
     2  21       6  160    110  3.9   2.88  17.0     0     1     4     4
     3  22.8     4  108     93  3.85  2.32  18.6     1     1     4     1
     4  21.4     6  258    110  3.08  3.22  19.4     1     0     3     1
     5  18.7     8  360    175  3.15  3.44  17.0     0     0     3     2
     6  18.1     6  225    105  2.76  3.46  20.2     1     0     3     1
     7  14.3     8  360    245  3.21  3.57  15.8     0     0     3     4
     8  24.4     4  147.    62  3.69  3.19  20       1     0     4     2
     9  22.8     4  141.    95  3.92  3.15  22.9     1     0     4     2
    10  19.2     6  168.   123  3.92  3.44  18.3     1     0     4     4
    # … with 22 more rows

------------------------------------------------------------------------

## Creating plots

The main purpose of this discussion is to cover the main options that
you will use when creating a plot with `qplot`. We will cover the
following topics.

-   Basics about the `qplot` function
    -   How to specify what goes on the axes
    -   How to specify which data will be used
    -   How to specify the chart type
-   How to change the chart type
-   How to add some basic customizations to a chart

### Basic qplot functionality

The following code produces a pretty standard bar chart using the
`qplot` function. The various parts of the function call are as follows.

-   `clarity` - this is one of the variables from the diamonds data set
    and functions as the variable of interest.
-   `data=diamonds` - the data set to be used when generating the plot.
-   `geom=bar` - “geom” stands for “geometric object” and tells `qplot`
    what kind of chart we’re creating (a bar chart in this case).

``` r
qplot(clarity, data=diamonds, geom="bar")
```

![](lab_2_qplot_files/figure-gfm/qplot_hist-1.png)

The “bar” geometric object is special in that it nearly always will
require some level of aggregation. If you prefer to be specific about
the aggregation you are doing, it is best to use the “col” geom as
follows. In this example, I have specified each of the options. Note
that the “col” geom requires both an x and a y parameter where the “bar”
geom only expects an x-axis.

``` r
diamond_hist <- diamonds %>% group_by(clarity) %>% summarise(n=n())
qplot(x=clarity,y=n,data=diamond_hist,geom='col')
```

![](lab_2_qplot_files/figure-gfm/qplot_calc_hist-1.png)

### Chart types

Chart types are specified using the `geom` parameter as seen above. When
using `qplot`, this just tells the charting engine what to draw. There
are many types of geometric objects, we’ll cover a subset of the most
common.

A complete list of geoms can be found
[here](https://ggplot2.tidyverse.org/reference/) in the “Layer: geoms”
section and, to a degree, in the “Layer: stats” section.

We will cover the following geoms in this section (noting that bar and
col have been already been covered).

-   geom_point - scatterplot
-   geom_jitter - scatterplot with random noise added
-   geom_boxplot - boxplot
-   geom_density - density plot
-   geom_line - line chart

#### geom_point

A scatterplot can be created by using `geom="point"` or by leaving the
option out of the `qplot` call as in the example below.

``` r
qplot(wt, mpg, data=mtcars)
```

![](lab_2_qplot_files/figure-gfm/qplot_scatter-1.png)

You can produce the same output by being more specific as in the
following example.

``` r
qplot(x=wt, y=mpg, data=mtcars, geom='point')
```

![](lab_2_qplot_files/figure-gfm/qplot_scatter2-1.png)

#### geom_jitter

Jitter plots are often used when one of the axes is categorical. First,
we’ll look at a standard scatterplot.

``` r
qplot(x=clarity,y=table,data=diamonds,geom='point')
```

![](lab_2_qplot_files/figure-gfm/geom_point_density-1.png)

This chart does not give us a good view of the full density of points as
points layerd on top of each other don’t look any different. By changing
this to a jitter plot we get a better view of the density of the data
points.

``` r
qplot(x=clarity,y=table,data=diamonds,geom='jitter')
```

![](lab_2_qplot_files/figure-gfm/geom_jitter-1.png)

#### geom_boxplot

Box plots may not be too common in business presentations, however, I
like them a lot so I will cover them quickly here. The boxplot draws a
box around the middle half of the data. The bottom of the box starts at
the 25th percentile and the top at the 75th with a line in the middle
for the 50th (or median). A line is used for data points within the
inter-quartile range (IQR) and individual dots are used outside
1.5\*IQR. I think they are an excellent way to visualize the density of
data points. You can see how it makes the chart a lot cleaner than in
the case of the jitter plot.

``` r
qplot(x=clarity,y=table,data=diamonds,geom='boxplot')
```

![](lab_2_qplot_files/figure-gfm/geom_boxplot-1.png)

#### geom_density

Also not terribly common in business presentations but very useful for
understanding your data. The density only requires an x parameter and
requires you to use a fill or facet to visualize multiple dimensions
(more on those in future labs). The following shows a single density
plot.

``` r
qplot(x=depth,data=diamonds,geom='density')
```

![](lab_2_qplot_files/figure-gfm/geom_density_single-1.png)

#### geom_line

Standard line chart. This chart requires an x and a y parameter but also
allows you to perform aggregations if you really want to. I prefer to
pre-aggregate data as it is often much clearer to someone reading
through the code. By someone, I mean my future self who is in some ways
smarter than my current self. Line charts are most often used with a
date value in the x-axis. Otherwise, there should be some reason that
you are connecting the lines together. Here, we’ll use the NYC flight
data since it includes a date component.

``` r
flights_by_day <- nycflights13::flights %>% 
  mutate(dt=as.Date(paste(year,month,day,sep='-'),format='%Y-%m-%d')) %>%
  group_by(dt) %>% summarise(n=n())

qplot(x=dt,y=n,data=flights_by_day,geom='line')
```

![](lab_2_qplot_files/figure-gfm/geom_line-1.png)

As a little bonus, you’ll notice that the data is highly seasonal within
the week. You can apply various transforms to get the data to a
different time grain as you’ll see in the following example. Here, I use
the `cut` function to truncate or round the dates to the nearest week.

``` r
flights_by_week <- flights_by_day %>% mutate(dt_week=as.Date(cut(dt,breaks='week'))) %>%
  group_by(dt_week) %>% summarise(n=sum(n))

qplot(x=dt_week,y=n,data=flights_by_week,geom='line')
```

![](lab_2_qplot_files/figure-gfm/geom_line_week-1.png)

### Adding customizations to the chart

Add aesthetic mapping (i.e. color), recall that `qsec` is an attribute
in the mtcars dataset indicating the car’s quarter-mile time. Also note
that `qplot` recognizes that `qsec` is a quantitative variable and uses
a continuous color scale. In the second example, we’ve used the
`as.factor` function to force `qplot` to make `cyl` categorical.

``` r
qplot(wt, mpg, data=mtcars, color=qsec)
```

![](lab_2_qplot_files/figure-gfm/qplot_aes-1.png)

``` r
qplot(wt, mpg, data=mtcars, color=as.factor(cyl))
```

![](lab_2_qplot_files/figure-gfm/qplot_aes-2.png)

Change size of points. In the following example, we use a static size
for the points.

``` r
qplot(wt, mpg, data=mtcars, color=qsec, size=5)
```

![](lab_2_qplot_files/figure-gfm/qplot_size_static-1.png)

In this example, we vary the size off the points based off of the number
of cylinders.

``` r
qplot(wt, mpg, data=mtcars, color=qsec, size=cyl) 
```

![](lab_2_qplot_files/figure-gfm/qplot_size_var-1.png)

Use alpha blending changes the transparency of each of the points. This
can be useful in some cases, especially if you have a large number of
points.

``` r
qplot(wt, mpg, data=mtcars, alpha=qsec, size=5)
```

![](lab_2_qplot_files/figure-gfm/qplot_alpha-1.png)

The following example show how to use the following aesthetic mappings.

-   `shape` - controls the shape of each point.
-   `size` - controls the size of each shape. This can be static or
    variable if given a column of data.
-   `color` - colors in the points. Note that this is different than
    `fill` which controls the interior color of an object.

``` r
qplot(wt, mpg, data=mtcars, shape=factor(cyl), size=5)
```

![](lab_2_qplot_files/figure-gfm/qplot_aes2-1.png)

``` r
qplot(wt, mpg, data=mtcars, size=qsec)
```

![](lab_2_qplot_files/figure-gfm/qplot_aes2-2.png)

``` r
qplot(wt, mpg, data=mtcars, size=qsec, color=factor(carb))
```

![](lab_2_qplot_files/figure-gfm/qplot_aes2-3.png)

``` r
qplot(wt, mpg, data=mtcars, size=qsec, shape=factor(cyl))
```

![](lab_2_qplot_files/figure-gfm/qplot_aes2-4.png)

``` r
qplot(wt, mpg, data=mtcars, size=qsec, shape=factor(cyl), color=factor(cyl)) 
```

![](lab_2_qplot_files/figure-gfm/qplot_aes2-5.png)

Bar-plot

``` r
qplot(factor(cyl), data=mtcars)
```

![](lab_2_qplot_files/figure-gfm/qplot_bar-1.png)

Flip plot by 90°. Here we actually blend the use of `qplot` and a
`ggplot` function.

``` r
qplot(factor(cyl), data=mtcars) + coord_flip()
```

![](lab_2_qplot_files/figure-gfm/qplot_bar_flip-1.png)

The following shows the difference between fill/color bars. Note that
`fill` shades the interior of the object while `colour` modifies the
line around the object. Also, the author of the `ggplot2` library is
from New Zealand, hence the odd spelling of the word ‘colour’. The more
americanized version ‘color’ is also available.

``` r
qplot(factor(cyl), data=mtcars, geom="bar", fill=factor(cyl))
```

![](lab_2_qplot_files/figure-gfm/qplot_bar_color-1.png)

``` r
qplot(factor(cyl), data=mtcars, geom="bar", colour=factor(cyl))
```

![](lab_2_qplot_files/figure-gfm/qplot_bar_color-2.png)

The following shows that if you set `fill` to a different variable, you
will get a stacked bar chart. Grouped bar charts can also be created if
you’re into that sort of thing.

``` r
qplot(factor(cyl), data=mtcars, geom="bar", fill=factor(gear))
```

![](lab_2_qplot_files/figure-gfm/qplot_bar_fill-1.png)

The histogram geom allows you to change the binwidths for more
precision.

``` r
qplot(carat, data=diamonds, geom="histogram", binwidth=0.1)
```

![](lab_2_qplot_files/figure-gfm/qplot_hist_bins-1.png)

``` r
qplot(carat, data=diamonds, geom="histogram", binwidth=0.01)
```

![](lab_2_qplot_files/figure-gfm/qplot_hist_bins-2.png)

An array of geoms can be passed to the geom parameter creating more
sophisticated plots.

``` r
qplot(wt, mpg, data=mtcars, geom="point")
```

![](lab_2_qplot_files/figure-gfm/qplot_combine-1.png)

``` r
qplot(wt, mpg, data=mtcars, geom="smooth")
```

    `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](lab_2_qplot_files/figure-gfm/qplot_combine-2.png)

``` r
qplot(wt, mpg, data=mtcars, geom=c("point", "smooth"))
```

    `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](lab_2_qplot_files/figure-gfm/qplot_combine-3.png)

Tweaking the smooth plot (“loess”-method: polynomial surface using local
fitting)

``` r
qplot(wt, mpg, data=mtcars, geom="smooth")
```

    `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](lab_2_qplot_files/figure-gfm/qplot_smooth-1.png)

Removing standard error

``` r
qplot(wt, mpg, data=mtcars, geom="smooth", se=FALSE)
```

    `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](lab_2_qplot_files/figure-gfm/qplot_smooth_se-1.png)

Making line more or less wiggly (span: 0-1)

``` r
qplot(wt, mpg, data=mtcars, geom="smooth", span=0.6)
```

    `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](lab_2_qplot_files/figure-gfm/qplot_smooth_span-1.png)

``` r
qplot(wt, mpg, data=mtcars, geom="smooth", span=1)
```

    `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](lab_2_qplot_files/figure-gfm/qplot_smooth_span-2.png)

Using linear modelling

``` r
qplot(wt, mpg, data=mtcars, geom="smooth", method="lm")
```

    `geom_smooth()` using formula 'y ~ x'

![](lab_2_qplot_files/figure-gfm/qplot_lm-1.png)

Sometimes qplot doesn’t give us what we’re looking for

``` r
qplot(wt, mpg, data=mtcars, color=factor(cyl), geom=c("smooth", "point"))
```

    `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](lab_2_qplot_files/figure-gfm/qplot_fail-1.png)

Need to upgrade to `ggplot`

``` r
ggplot() +
  geom_point(data=mtcars, aes(x=wt, y=mpg, colour=factor(cyl))) +
  geom_smooth(data=mtcars, aes(x=wt, y=mpg))
```

    `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](lab_2_qplot_files/figure-gfm/ggplot-1.png)

`ggplot2` official documentation HTTP://docs.ggplot2.org/current/
