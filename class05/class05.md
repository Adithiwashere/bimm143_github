# class05
Adithi (PID: A16653979)

## Using GGPLOT

The ggplot2 package needs to be installed as it doesn’t not come with R
“out of the box”

We use the ‘install.packages()’ function to do this

To use ggplot, I need to load it up using ‘library(ggplot2)’

``` r
#install.packages (gglot2)
library(ggplot2) 
ggplot()
```

![](class05_files/figure-commonmark/unnamed-chunk-1-1.png)

ALL ggplot figures have at least three things:

- data (the stuff we want to plot)

- aesthetic mapping (x,y)

- geoms

  ``` r
  ggplot (cars) + aes(x=speed, y=dist) + geom_point()
  ```

  ![](class05_files/figure-commonmark/unnamed-chunk-2-1.png)

ggplot is not the only graphing system in R. There are lots of others.
There is even “base R” graphics.

``` r
plot(cars)
```

![](class05_files/figure-commonmark/unnamed-chunk-3-1.png)

### **Creating Scatter Plots**

``` r
ggplot(cars) + 
  aes(x=speed, y=dist) +geom_point() +geom_smooth() +
  labs(title = "Speed vs. Stopping Distances of Cars", x= "Speed (MPH)", y = "Stopping Distance (ft))", caption = "Dataset: 'cars'")+ theme_bw()
```

    `geom_smooth()` using method = 'loess' and formula = 'y ~ x'

![](class05_files/figure-commonmark/unnamed-chunk-4-1.png)

**Adding More Plot Aesthetics using aes()**

``` r
url <- "https://bioboot.github.io/bimm143_S20/class-material/up_down_expression.txt" 
genes <- read.delim(url) 
head(genes)
```

            Gene Condition1 Condition2      State
    1      A4GNT -3.6808610 -3.4401355 unchanging
    2       AAAS  4.5479580  4.3864126 unchanging
    3      AASDH  3.7190695  3.4787276 unchanging
    4       AATF  5.0784720  5.0151916 unchanging
    5       AATK  0.4711421  0.5598642 unchanging
    6 AB015752.4 -3.6808610 -3.5921390 unchanging

``` r
nrow(genes) 
```

    [1] 5196

``` r
colnames(genes) 
```

    [1] "Gene"       "Condition1" "Condition2" "State"     

``` r
ncol(genes) 
```

    [1] 4

``` r
table(genes$State) 
```


          down unchanging         up 
            72       4997        127 

``` r
round( table(genes$State)/nrow(genes) * 100, 2 )
```


          down unchanging         up 
          1.39      96.17       2.44 

``` r
p <- ggplot (genes) + aes(x= Condition1, y = Condition2, col=State) + geom_point() 
p + scale_colour_manual( values = c("violet", "lightblue", "lightgreen"))+ 
  labs(title = "Gene Expression Changes upon Drug Treatment", x= "Control (No Drug)", y= "Drug Treatment")
```

![](class05_files/figure-commonmark/unnamed-chunk-7-1.png)

#### **Going Further**

``` r
#install.packages("gapminder") 
#install.packages ("dplyr")
library(gapminder) 
library(dplyr) 
```


    Attaching package: 'dplyr'

    The following objects are masked from 'package:stats':

        filter, lag

    The following objects are masked from 'package:base':

        intersect, setdiff, setequal, union

``` r
gapminder_2007 <- gapminder %>% filter(year==2007) 
ggplot(gapminder_2007)+ aes(x=gdpPercap, y=lifeExp, color=continent, size = pop)+
  geom_point(alpha=0.5) 
```

![](class05_files/figure-commonmark/unnamed-chunk-8-1.png)

``` r
ggplot(gapminder_2007)+ aes(x=gdpPercap, y=lifeExp, color = pop) +
geom_point(alpha=0.8) 
```

![](class05_files/figure-commonmark/unnamed-chunk-8-2.png)

``` r
ggplot(gapminder_2007) + geom_point(aes(x = gdpPercap, y = lifeExp,size = pop),
alpha=0.5) + scale_size_area(max_size = 10)
```

![](class05_files/figure-commonmark/unnamed-chunk-9-1.png)

``` r
gapminder_1957 <- gapminder %>% filter(year==1957) 
ggplot(gapminder_1957)+ 
aes(x=gdpPercap, y=lifeExp, color =continent, size =pop) +geom_point(alpha=0.7) + scale_size_area(max_size =15)
```

![](class05_files/figure-commonmark/unnamed-chunk-10-1.png)

``` r
gapminder_1957 <- gapminder %>% filter(year==2007| year == 1957) 
ggplot(gapminder_1957)+ 
aes(x=gdpPercap, y=lifeExp, color =continent, size =pop) +geom_point(alpha=0.7)+ scale_size_area(max_size =15) +facet_wrap(~year)
```

![](class05_files/figure-commonmark/unnamed-chunk-11-1.png)

##### Bar Charts

``` r
gapminder_top5 <- gapminder %>% 
  filter(year==2007) %>% 
  arrange(desc(pop)) %>% 
  top_n(5, pop)

gapminder_top5
```

    # A tibble: 5 × 6
      country       continent  year lifeExp        pop gdpPercap
      <fct>         <fct>     <int>   <dbl>      <int>     <dbl>
    1 China         Asia       2007    73.0 1318683096     4959.
    2 India         Asia       2007    64.7 1110396331     2452.
    3 United States Americas   2007    78.2  301139947    42952.
    4 Indonesia     Asia       2007    70.6  223547000     3541.
    5 Brazil        Americas   2007    72.4  190010647     9066.

``` r
ggplot(gapminder_top5)+ aes(x= country, y=pop) + geom_col() 
```

![](class05_files/figure-commonmark/unnamed-chunk-13-1.png)

``` r
ggplot(gapminder_top5)+ aes(x= country, y=lifeExp) + geom_col()
```

![](class05_files/figure-commonmark/unnamed-chunk-13-2.png)

``` r
ggplot(gapminder_top5)+ aes(x= country, y=pop, fill = continent) + geom_col()  
```

![](class05_files/figure-commonmark/unnamed-chunk-14-1.png)

``` r
ggplot(gapminder_top5)+ aes(x= country, y=pop, fill = lifeExp) + geom_col() 
```

![](class05_files/figure-commonmark/unnamed-chunk-14-2.png)

``` r
ggplot(gapminder_top5)+ 
aes(x= reorder(country, -pop), y=pop, fill = gdpPercap)+ geom_col()  
```

![](class05_files/figure-commonmark/unnamed-chunk-15-1.png)

``` r
ggplot(gapminder_top5)+ 
  aes(x= reorder(country, -pop), y=pop, fill = country) +
geom_col(col = "gray30")  +guides(fill = "none")
```

![](class05_files/figure-commonmark/unnamed-chunk-15-2.png)

``` r
head(USArrests) 
```

               Murder Assault UrbanPop Rape
    Alabama      13.2     236       58 21.2
    Alaska       10.0     263       48 44.5
    Arizona       8.1     294       80 31.0
    Arkansas      8.8     190       50 19.5
    California    9.0     276       91 40.6
    Colorado      7.9     204       78 38.7

``` r
USArrests$State <- rownames(USArrests) 
ggplot(USArrests) +aes(x=reorder(State,Murder), y=Murder) +geom_point() +
geom_segment(aes(x=State, xend=State, y=0, yend=Murder), color="violet")+
coord_flip()
```

![](class05_files/figure-commonmark/unnamed-chunk-16-1.png)

##### Combining Plots

``` r
# install.packages ("patchwork")
library(patchwork) 
p1 <-  ggplot(gapminder_top5)+ 
  aes(x= reorder(country, -pop), y=pop, fill = gdpPercap) + geom_col()   

p2 <- ggplot(gapminder_top5)+ aes(x= reorder(country, -pop), y=pop, fill = country) + geom_col(col = "gray30")  + guides(fill = "none")

(p1 |p2)
```

![](class05_files/figure-commonmark/unnamed-chunk-17-1.png)
