What went wrong?
================
Robert Gruener

``` r
knitr::opts_chunk$set(echo = TRUE, error = TRUE)
```

## HW02 Part A

In this document, I will add some examples of some coding mistakes, it
is up to you to figure out why the graphs are messing up.

### First load packages

It is always best to load the packages you need at the top of a script.
It’s another common coding formatting standard (like using the
assignment operator instead of the equals sign). In this case, it helps
people realize what they need to install for the script and gives an
idea of what functions will be called.

It is also best coding practice to only call the packages you use, so if
you use a package but end up tossing the code you use for it, then make
sure to remove loading it in the first place. For example, I could use
`library("tidyverse")` but since this script will only be using ggplot2,
I only load ggplot2.

``` r
library("ggplot2")
library("magrittr") #so I can do some piping
```

### Graph Fail 1

What error is being thrown? How do you correct it? (hint, the error
message tells you)

*R-* There were two errors. First, the %\>% before geom\_point which was
replaced by a + sign. Second, there was no variable “city” in the data
table (but there was a “cty” variable)

``` r
data(mpg) #this is a dataset from the ggplot2 package

mpg %>% 
  ggplot(mapping = aes(x = cty, y = hwy, color = "blue")) + 
  geom_point()
```

![](HW02_A_Graph-Fails_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

### Graph Fail 2

Why aren’t the points blue? It is making me blue that the points in the
graph aren’t blue :\`(

*R-* The argument to change color was misplaced (should not be within
mapping = aes())

``` r
ggplot(data = mpg) + 
  geom_point(mapping = aes(x = displ, y = hwy) , color = "blue")
```

![](HW02_A_Graph-Fails_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

### Graph Fail 3

Two mistakes in this graph. First, I wanted to make the the points
slightly bolder, but changing the alpha to 2 does nothing. What does
alpha do and what does setting it to 2 do? What could be done instead if
I want the points slightly bigger?

*R-* By modifying the “alpha” argument changes the degree of
transparency of the points. To modify the size you need to use the
argument “size”.

Second, I wanted to move the legend on top of the graph since there
aren’t any points there, putting it at approximately the point/ordered
pair (5, 40). How do you actually do this? Also, how do you remove the
legend title (“class”)? Finally, how would you remove the plot legend
completely?

*R-* For the position, one should use the relative position in x and y
(from 0 to 1, where 0 is the origin in both axes and 1 the highest
values). To remove the title I added
theme(legend.title=element\_blank()).

``` r
mpg %>% 
ggplot() + 
  geom_point(mapping = aes(x = displ, y = hwy, color = class), size = 2) + 
  theme(legend.position = c(0.5, 0.75)) +
  theme(legend.title=element_blank())
```

![](HW02_A_Graph-Fails_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->

*R-* To completely remove the legend:

``` r
mpg %>% 
ggplot() + 
  geom_point(mapping = aes(x = displ, y = hwy, color = class), size = 2) + 
  theme(legend.position = "none") 
```

![](HW02_A_Graph-Fails_files/figure-gfm/unnamed-chunk-4-1.png)<!-- -->

### Graph Fail 4

I wanted just one smoothing line. Just one line, to show the general
relationship here. But that’s not happening. Instead I’m getting 3
lines, why and fix it please?

*R-* Because the grouping based on “drv” (which is shown by color) is
taking into account unless stated when making the lines. Therefore, I
added aes(color=NULL) within geom\_smooth, so the color grouping was
ignored when making the lines. I also changed the line color to black.

``` r
mpg %>% 
ggplot(mapping = aes(x = displ, y = hwy, color = drv)) + 
  geom_point() + 
  geom_smooth(se = F, aes(color=NULL), color = "black") #se = F makes it so it won't show the error in the line of fit
```

    ## `geom_smooth()` using method = 'loess' and formula 'y ~ x'

![](HW02_A_Graph-Fails_files/figure-gfm/unnamed-chunk-5-1.png)<!-- -->

### Graph Fail 5

I got tired of the points, so I went to boxplots instead. However, I
wanted the boxes to be all one color, but setting the color aesthetic
just changed the outline? How can I make the box one color, not just the
outline?

Also, the x-axis labels were overlaping, so I rotated them. But now they
overlap the bottom of the graph. How can I fix this so axis labels
aren’t on the graph?

*R-* To make the boxes all one color, you need to add the argument
“fill”. To modify the position of x-axis labels, I modified the
“hjust” (justification)
argument.

``` r
ggplot(data = mpg, mapping = aes(x = manufacturer, y = cty, color = manufacturer, fill=manufacturer)) + 
  geom_boxplot() + 
  theme(axis.text.x = element_text(angle = 45, hjust=1))
```

![](HW02_A_Graph-Fails_files/figure-gfm/unnamed-chunk-6-1.png)<!-- -->
