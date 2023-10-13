---
layout: post
title:  "ggplot is forever"
author: Daphne LeSueur
description: A basic tutorial for your basic ggplot needs. 
img: lines.jpg
--- 

<!-- ### Introduction -->
Base R plotting is out, ggplot is in. Actually, Base R plotting has been old hat since 2005 thanks to Hadley Wickham with the creation of ggplot. In this article we are going to go over the BASIC basic elements necessary for plotting in ggplot, as well as how to troubleshoot some common errors. In this article, we will be demonstrating with the `penguins` data set. Start by installing the `palmerpenguins` package and loading the package to your machine. 
```{r}
install.packages('palmerpenguins')
library(palmerpenguins)
```

## Create a ggplot
The very first thing you need to do to create your visualization is to create a ggplot, give it some data, and decide what aesthetics you want to appear on your plot. The basic arguments for a ggplot are as follows:
```{r}
ggplot(data, mapping = aes(x, y, ...))
```
The data part is mostly self-explanatory. Feed the plot whatever data you are wishing to visualize!

However, the mapping part is a little more confusing. Think to yourself, *What do I want to actually appear in the visualizaiton? What do I want to map to the graph?* This goes inside the `aes()` part of the argument. This will include your variable(s), colors, and a variety of different other ways to visualize the data which we will go over later. 

```{r}
base <- ggplot(data = penguins, mapping = aes(x = flipper_length_mm, y = body_mass_g))
```
We're going to assign this plot to the variable `base` so we can refer to it later. This is not required, but can make the code simpler when you are changing lots of things about the plot. 

## Choose your visualization type
If you haven't already chosen what type of graph you would like, now is the time. After your initial ggplot, you need to add what's called a geom. The geom dictates which of the things you put in your `aes()` go where on the graph. 

Some common geoms:
- `geom_point`(scatterplot)
- `geom_histogram` (histogram)
- `geom_bar`(bar chart)
- `geom_boxplot` (box plot)
- `geom_smooth` (line)

Note that different graphs require different numbers of variables. A scatterplot requires two variables, but a histogram, bar chart, and box plot only require one variable each. 

Since we chose two variables, (`flipper_length_mm` and `body_mass_g`) we will visualize this by adding a `geom_point`.

```{r}
base +
    geom_point()
```
![ggplot]("assets/img/basic_ggplot.jpg")
## Customize the plot
Now that you have a plot plot with a geom, you can start to customize the way it looks. First, let's add some labels by adding a `labs()` element. 

The arguments of `labs()` are:
```
labs(x = "X axis label", 
    y = "Y axis label", 
    title ="title above plot",
    subtitle = "subtitle below title",
    caption = "caption below plot",
    alt = "Add alt text to the plot")
```
The labels we want for this plot will look something like this: 
```
base +
    geom_point() +
    labs(x = "Flipper Length (mm)", 
    y = "Body Mass (g)", 
    title = "Flipper Length vs Body Mass")
```

![plot]("assets/img/plot_with_titles.jpg")

The other customizable elements we're going to explore in this tutorial are the color and shape of our points. In other graph types, this translates to color and line type, color and fill type, etc. 

We're going to specify the color and shape of our points in our `geom_point()`. 

```
base +
    geom_point(color = "blue", shape = 25) +
    labs(x = "Flipper Length (mm)", 
    y = "Body Mass (g)", 
    title = "Flipper Length vs Body Mass")
```
![plot]("assets/img/blue_triangle_plot.jpg")

There are plently of colors and shapes that are built into R Studio. Play around with some colors you like and see if they're included. You an also find plently of lists of descriptive colors and shapes online. 

The last type of customization I want to demonstrate is using a variable as one of our customizable aesthetics. 

In the plot we've been working with, we're looking at the flipper length compared to the body mass of these penguins. Also included in the dataset is the island on which each observed penguin lives. It would be nice to see in our graph the differences between the flipper length/body mass comparison for each island. 

Since we're going to be calling a variable from the dataset, we want to put this inside an `aes()` in our `geom_point`. A good rule of thumb is that all calls to variables go inside the `aes()`. 

``` {r}
base +
    geom_point(aes(col = island)) +
    labs(x = "Flipper Length (mm)", 
    y = "Body Mass (g)", 
    title = "Flipper Length vs Body Mass")
```
![plot]("assets/img/islands.jpg")

## Conclusion
Ggplot isn't as intimidating as it may be, even if you're well seasoned in Base R plotting. Next time you're visualizing your data, try it in ggplot!
