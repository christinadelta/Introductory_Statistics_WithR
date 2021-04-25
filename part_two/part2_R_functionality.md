## Introduction to R functionality -- Part 2

**Author:** Christina Dimitriadou (christina.delta.k@gmail.com)

**Date:** 10/04/2020

This is the second of 5 introductory notebooks to statistcs with R. In the first notebook I covered a few basic concepts on:

* Variables 
* Vectors
* Sequences
* Functions
* R scripts 

In this notebook you will learn how to handle data frames and matrices in R. 

## Data frames 
A data frame (also called tabular data) is data that is structured into raws and columns, each of which contains information about some thing. Data frames in R are extremely important and they are very frequently used. Generally, each raw in the data frame represents a unit of observation and each column to contain a different type of information about teh units of observation. 

In R, tabular data is called **tidy data**

In this notebook we will be using a collection of modern packages known as **Tidyverse**. Tidyverse has its own slightly improved form of data frame, which is called *tibble*. Tidyverse is generally safer and makes data science look easier. 

Note that this is a mere introduction to data frames. In the README.md file of this repo I also include a number of extra resources for further reading and exercising. 

## Setting up

The first step is to ensure that you have the files that will be used in this tutorial and to install the *Tidyverse* packages. 

First install the *Tidyverse* packages:

```{r}

# install tidyverse
install.packages("tidyverse")

```

Sometimes people have problems installing all the packages in Tidyverse (especially on windows machines). If that happens to you, you may be able to successfully install each package individually:

```{r}
install.packages(c("dplyr", "readr", "tidyr", "ggplot2"))
```

Now that we have loaded the data and installed our packages, the next step is to load the packages in order to be able to use them. Note that in R installing and loading packages are two different things:

* we install packages/libraries once and they are stored in our system 
* we load the packages/libraries that we need in every R session, depending on what exactly we want to do on that session.

For now, we only need to use the tidyverse packages:

```{r}
library(tidyverse)

# if you installed each package individually then:
library(dplyr)
library(readr)
library(tidyr)
library(ggplot2)
```

The tidyverse package loads various other packages setting up an R environment ready to use. In this notebook we will be using built-in functions from the ```dplyr```, ```readr```, and ```tidyr``` packages, ```ggpplot``` is used to plot and visualise data.

R is a language with mini-languages within it that solve specific domain problems. For example, ```dplyr``` uses such a mini-language, a set of *verbs* (or functions) that work well together. ```dplyr``` in combination with ```tidyr``` use some more complex operations and provide a way to perform powerful manipulations on your data frames. 

## Load the dataset

The file is in CSV format and it should be in the folder **intro_data**. I have a detailed description of the data set that we will use in the README.md file but, here is a short description: 
The dataset comes from a simple categorization task, where 24 participants (12 male, 12 female) were asked to classify stimuli briefly presented on the screen as living or non-living by a key-press. Stimuli consisted of 4 categories (scenes, objects, humans and animals), and every stimulus was presented 20 times across the entire experiment split in 8 blocks. 

We will use the ```read.csv``` function to load (this comes with base R), however, ```readr``` has a function ```read_csv``` that could also be used. CSV stands for Comma Seperated Values and is a text format used to store tabular data.

```{r}
# change this to your own path (the path of where you have stored the data set)
data = read.csv("~/Desktop/intro_toR/intro_data/rt_data/allcategoryrts.csv", header=FALSE, sep=";")

data # to visualise

```

Given that we do not have headers in our dataset the column names appear as V1, V2... (each column is a different variable and the rows are observations or units of those variables). The next step involves adding column names or headers:

```{r}
# add headers
colnames(data) = c("subj", "rt", "classes", "cat", "gender", "category")

```

Notice that we have two variables (column names) **cat** and **category**. Cat has 8 types (1:8) and cat has 4 types (1:4). This is because the stimuli belong to 4 major categories: scenes, objects, animals, humans, which can be subdivided into 4 more categories: indoor scenes & outdoor scenes, everyday small objects and vehicles, animal faces and whole bodies, human faces and whole bodies. 


Note that we can also create a dataframe ourselves inside RStutio using the ```tibble``` funcion of the tidyverse package or the ```data.frame``` function in base R:

```{r}
# create a dataframe using the tibble function
tibble(num=c(1,2,3), letter=c("a","b","c"))
```

The two arguments (num,letter) become the column names. 

Note that there are many ways to load a dataset in RStudio besides using commands:

* on the bottom right panel choose the files browser and from your home directory go to the data path, click on the dataset and select *import dataset...*

![Imgur](https://i.imgur.com/2yEikgW.png) 

Or on the top right panel press on *import Dataset*:

![Imgur](https://i.imgur.com/WM4tduc.png)

## Exploring your data

The ```View``` function gives us a spreadsheet-like view of the data frame:

```{r}
View(data) # take a look at the dataset
```

We can also use ```print``` with ```n``` arguments to show more than 10 rows on the console:

```{r}
print(data, n=50)
```

we can extract details of the data frame using further functions:

```{r}
# extract number of rows
nrow(data)

# extract number of columns
ncol(data)
```

Take a look at the column names (variable names):

```{r}
colnames(data)

```

Or print a summary of the data. A summary includes minmum and maximum values, 1st and 3rd quartiles, mean and median:

```{r}
summary(data)
```

## Indexing data frames and extracting specific information

We can extract subset of the data using [row, column] syntax:

```{r}
# extract the 4th row of the 5th column:
data[4,6]

# extarct the 5th row of the rts (1st) column
data[5,1]
```

Or we can specify columns by name:
```{r}
# extract 5th row of the column classes:
data[5, "classes"]
```

Remember: String characters **ALWAYS** go inside quotations

If we want to extract all columns of a specific row then the column is omitted (same logig is applied on rows):

```{r}
# extract all columns of the row 6:
data[6,]

# extarct all rows of the column 5(gender):
data[,5]

# or..
data[,"gender"]
```

We can use vectors to extract multiple rows or columns:

```{r}
# first combine the rows we want using the c() function
rows.toextract = c(2,5,10)
data[rows.toextract,] # all columns
```

We could also write it in one line:

```{r}
data[c(2,5,10),]
```

Or we can specify sequences of rows (or columns) we want to extract:

```{r}
# extract rows 1 to 10 from all columns
data[1:10,]
```

## Columns of a data frame are vectors 

We saw how we can extract and visualise specific elements from a data frame, but how can we actually get data out of a data frame?

If you think about it, a  data frame is just a list of column vectors. we can use the dollar sign ```$``` to retrieve a column (or columns) by using the ```head()``` function

```{r}
head(data$classes)
```

By default head shows the first 6 rows of a vector/column but if we want to visualise more we can specify it:

```{r}
head(data$classes,10)
```

If we want to look at the fiew last rows, we can use the ```tail()``` function:

```{r}
tail(data$classes)

# or 
tail(data$classes,10) # for the last 10
```

Using the dollar sign ```$``` we can also extarct specific elements without the ```head()``` or ```tail()``` functions:

```{r}
# extract 5th value  of the column classes
data$classes[5]
```

Given that in a data frame each column/vector is also a variable, think of ```$``` as a means to extract a variable.

Now, we have 5 variables in our data frame. 4 of them are categorical and 1 is continuous (the reaction times; rts). Let's plot reaction times and category:

```{r}
# create box plots 
boxplot(rt ~ category, data = data, frame = FALSE,
        names = c("objects", "scenes", "animals", "humans"))
```

Creating boxplots this way comes with R base, meaning that we do not need any packages, however we will also learn to use the package ```ggplot2``` for more powerfull plots with R.
Notice that we did not need to use the dollar sign ```$``` to specify the variables we need? This is because we have the option to specify the data frame using the ```data``` argument. We could also write it this way:

```{r}
boxplot(data$rt ~ data$category, frame = FALSE,
        names = c("objects", "scenes", "animals", "humans"))
```

## Factors in data frames 

In R factor is variable used to categorise and store the data, having a limited number of different values. A Factor stores the data as a vector of integer values or strings. A factor is also known as a categorical variable with levels used mostly in statistical modelling and exploratory data analysis with R.

In this dataset we only have 1 continuous variable (rt) which stores the reaction times of each participant for living and non-living stimuli. So, the variable classes is a categorical variable (or factor), and it has two levels: **1: living** and **2: non-living**. We can also create a new variable/vector in the dataframe which will convert the two levels of the factor **classes** in string characters:

```{r}
# create new column in the data dataframe:
data$class = factor(data$classes,
                    levels = c("1", "2"),
                    labels = c("living", "non-living"))
```


### Exersice 
Given that we have 3 categorical variables in our dataframe (classes, gender, category or cat), as an exercise, create a new variable for gender and category (or cat), in which the levels will be strings instead of numbers. 

### More on factors:

We can also use the ```count``` function from ```dplyr``` to understand the columns/vectors of the dataframe:

```{r}
# count the observations/units for each of the two levels of the class factor
count(data, class)
```

This is an easy to handle dataset, because the number of observations/units for each condition/level of the factors is the same.

When we use ```plot``` with factorial data, it outputs a bar plot:

```{r}
plot(data$class)
```

Note that the vectors class and classes are not handled in the same way in R. This is because R handles the **class** vector as factor (with levels and labels) but **classes** is just a variable with categorical data. Let's plot the vector **classes** to actually see the difference:

```{r}
plot(data$classes)
```

As sees, the levels of the factorial vector **class** has levels in order and associated labels (living vs non-living) which R can read, while the vector **classes** doesn't.

To better understand this, try the ```count``` function with the **classes** vector instead of the **class** vector.

```{r}
count(data, classes)
```
Notice the difference? R read the **class** vector as factor <fctr>, while the **classes** vector as integer <int>. This is why, converting the categorical data to factorial is highly recommended in R. It is much easier to manipulate and handle factors. 

## Logical indexing 

Logical indexing is another way of specifying the row numbers that we want to extract. We give a logical vector which is ```TRUE``` for the rows we want ```FALSE``` otherwise. We can use logical indexing as vector first using R base and the using the ```dplyr``` package.

Let's look at rts that are shorter that 0.6 sec:

```{r}
short.rts = data$rt < 0.6

short.rts
```

We can also summarise the elements of the indexed vector using the ```sum()``` function. ```Sum()``` treats TRUE as 1 and FALSE as 0, so it will return the number of TRUE elements in the vector:

```{r}
sum(short.rts)
```

We can use the vector of logical values to extract all the information of those TRUE elements from the data frame:

```{r}
data[short.rts,]
```

These are the comparison operators available:

* equal to: ```a == b```
* not equal to: ```a != b```
* less than: ```a < b```
* greater than: ```a > b```
* less than or equal to: ```a <= b```
*greater than or equal to: ```a >= b```

We can also extract more complicated conditions using logical operators:

* TRUE, only when both a and b are TRUE: ```a & b```
* TRUE, if either a or b or both are TRUE: ```a | b```
* TRUE if a is false and FALSE if a is true: ```! a```

Let's see how many rts between 800 and 900 ms we have for the living stimuli only:

```{r}
eight.onlyliving = data$rt > 0.8 
nine.onlyliving = data$rt < 0.9

eight = eight.onlyliving & nine.onlyliving
sum(eight) # how many trues in the eight vector?
eight # visualise
```

Now let's find those extracted elements in the dataframe:

```{r}
data[eight,]
```

### Exersice: logical indexing 

1. How many rts between 800 and 1000 ms do we have only for living stimuli?
2. How many rts between 600 and 800 ms do we have for female participants only? 
3. How many rts between 700 and 900 ms do we have for category 3 (animals)? 

### let's try logical indexing using the ```dplyr``` package

Using base R for logical indexing is intuitive and quite easy, however, we need to keep mentioning the name of the dataframe and there is a lot of punctuation to keep track of. The 
```dplyr``` packages has a function called ```filter``` that makes logical indexing and xomparisons very easy:

```{r}
filter(data, rt < 0.7 & classes == 2)
```

Above, we asked R to show as the rts (reaction times) that are less than 700ms but only for non-living stimuli, **all in one line of code**. This is the magic of ```dplyr```!

Try some logical indexing yourself!

## Sorting dataframes

We can sort dataframes using the ```arrange``` function in ```dplyr```.

```{r}
arrange(data, classes)
```

Numeric columns are sorted in numeric order, and string columns are sorted in alphabetical order. Factor columns are sorted in the order of their levels. 

We can also use the ```desc``` function to arrange data in descending order:

```{r}
arrange(data, desc(classes))
```

## References for further reading 

In this notebook we covered the essentials of dataframes and ```dplyr``` but there is much more to learn. Here are some links for a deeper understanding of R and dataframes:

* [R - Data Frames](https://www.tutorialspoint.com/r/r_data_frames.htm)
* [R Data Frame](https://www.datamentor.io/r-programming/data-frame/)
* [An Introduction to R](https://cran.r-project.org/doc/manuals/r-release/R-intro.pdf)

