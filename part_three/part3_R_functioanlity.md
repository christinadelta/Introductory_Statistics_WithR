## Introduction to R functionality -- Part 3

**Author:** Christina Dimitriadou

**Date:** 12/04/2021

This is the third of 5 introductory notebooks to statistics with R. In the previous two notebooks I covered a few basic concepts on:

* Variables, vectors, sequences
* Functions
* R scripts
* Data frames 
* Installing and loading packages

In this notebook I will cover essential information on plotting and data visualisation with **ggplot2** and **R base**.

## Plotting with R -- Intro to ggplot2

In the previous notebook we already saw R's built-in plotting function ```plot()```. This is one of the most frequently used plotting function of type generic (meaning it comes with R base, we do not need to install it). It produces plots that are dependent to the class of the first argument (x). 

**Let's assume that we want to plot: ```plot(x,y)```:

* If ```x``` and ```y``` are both vectors, ```plot(x,y)``` will produce a scatterplot of *y* against *x*.
* If ```x``` is a time-series, ```plot(x,y)``` will produce a time-series plot.
* If ```x``` is a factor, then ```plot(x,y)``` will produce a box-plot. 

A more recent and powerful library for creating plots is ```ggplot2```. This package/library is like an R mini-language (meaning that it has its own way of typing the commands within the R environment). This package was created by Hadley Wickham and it implements ideas based on Leland Wilkinson's [The Grammar of Graphics](https://www.springer.com/gp/book/9780387245447). 

Here, I will cover some basic aspects of the ```ggplot``` syntax, but more information can be found in the [online documentation](https://ggplot2.tidyverse.org)

We saw int he previous notebook that ```ggplot2()``` is part of *Tidyverse*, so loading the *Tidyverse* will also load *ggplot* automatically:

```{r}
library(tidyverse)
```

Or if *ggplot* was installed independently of *Tidyverse*:

```{r}
library(ggplot2)
```

We will keep using this simple **reaction times** dataset from the previous notebook, lo let's load it:

```{r}
# change this to your own path (the path of where you have stored the data set)
data = read.csv("~/Desktop/intro_toR/intro_data/rt_data/allcategoryrts.csv", header=FALSE, sep=";")

data # to visualise

# add headers
colnames(data) = c("subj", "rt", "classes", "cat", "gender", "category")

# convert categorical variables into factors 
# classes - class
data$class = factor(data$classes,
                    levels = c("1", "2"),
                    labels = c("living", "non-living"))

# category - fcat
data$fcat = factor(data$category,
                   levels = c("1", "2", "3", "4"),
                   labels = c("objects", "scenes", "animals", "humans"))

# gender - fgender
data$fgender = factor(data$gender,
                      levels = c("1", "2"),
                      labels = c("male", "female"))


```

Note that we converted the categorical variables to factors in order to visualise and manipulate them easier.

### Pieces of a ggplot()

In order to produce a plot with ```ggplot2()``` we need three things:

* A data-frame with the data that we want to visualise (df)
* A way to translate the columns/vectors of the *df* into colours, positions, sizes, grouping (aesthetics or aes)
* The actual things that we want to display (the geometric object)

Let's make a first ```ggplot```:

```{r}
ggplot(data, aes(x=category, y=rt)) + 
  geom_point()
```

Let's explain the above command:
Note that we use the ```geom_point``` function to create a scatterplot and in the ```asc()``` we give the *x* and *y* positions. In order to initialise a ```ggplot``` we give the dataframe as the first argument and ```aes```. We could also provide specific arguments for colour, size, etc. ```aes``` is an example of non-standard evaluation and arguments of ```aes``` can refer to columns/vectors of the dataframe directly. Once we have the base: ```ggplot()```, we then add layers of graphics using ```+``` and the ```geom_``` functions. 

The above is a basic illustration, we can use more aesthetics either numeric or categorical:

```{r}
ggplot(data, aes(x=fcat, y=rt, color=fgender)) + 
  geom_point()
```

Let's explain the above command:
Note that we use the ```geom_point``` function to create a scatterplot and in the ```asc()``` we give the *x* and *y* positions. In order to initialise a ```ggplot``` we give the dataframe as the first argument and ```aes```. We could also provide specific arguments for colour, size, etc. ```aes``` is an example of non-standard evaluation and arguments of ```aes``` can refer to columns/vectors of the dataframe directly. Once we have the base: ```ggplot()```, we then add layers of graphics using ```+``` and the ```geom_``` functions. 

The above is a basic illustration, we can use more aesthetics either numeric or categorical:

```{r}
ggplot(data, aes(x=fcat, y=rt, color=fgender)) + 
  geom_point()
```


### First challenge using ```ggplot()```

Create a ```ggplot()``` using:

* *rt* as the x position
* *class* as the y position
* *fcat* as colour

### More geoms with ```ggplot()```

Given that we have a lot of factors in this dataset, lets plot boxplots:

```{r}
ggplot(data, aes(x=classes, y=rt, group=classes)) +
  geom_boxplot()
```

We can also plot data as lines:

```{r}
ggplot(data, aes(x=rt, y=cat, group=category, color=subj)) + 
  geom_line()
```

We can also use a bar plot:

```{r}
ggplot(data, aes(fcat)) +
  geom_bar()
```

or map the **fcat** to y instead to flip the orientation

```{r}
ggplot(data, aes(y=fcat)) +
  geom_bar()

```

We can also group bar plots. For example, the classes column defines living and non-living, but these are further defined as objects, scenes, etc in the category column:

```{r}
ggplot(data, aes(x=classes, fill=fcat)) + 
  geom_bar()
```

### Print or Save a ```ggplot()```

We can store plots in variables and print them:

```{r}
plt = ggplot(data, aes(classes)) + geom_bar()

plt # 
```


This will plot the "plt" object on the console not in the plots section where we can save them. In order to save the plot as a .png or.csv file:

```{r}
# first print the plot:
print(plt)

# save the plot
ggsave("test.png", p) # this will save the plot as a png file in your home directory
```





