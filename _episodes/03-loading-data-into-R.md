---
title: "Getting started with the tidyverse; loading your data"
teaching: 20
exercises: 10
questions:
- "How do I load the tidyverse?"
- "How can I read and write tabular data in R?"
objectives:
- "To know how to load the main tidyverse packages"
- "To be able to read csv data into a tibble"
- "To be aware of the different types of data."
- "To begin exploring `tibbles`"
- "To be able to extract parts of a `tibble`"
keypoints:
- "Tibbles let us store tabular data in R.  Tibbles are an extension of the base R data frame."
- "Use `read_csv` to read tabular data into a tibble R."
- "User `write_csv` to write tabular data to a comma separated value file."
- "Use factors to represent categorical data in R. You should specify the levels of your factors."
source: Rmd
---



## Installing Tidyverse

The tidyverse consists of several packages (we discuss the main ones below), which are all on CRAN.  We can install the tidyverse like any other CRAN package, using:


~~~
install.packages("tidyverse")
~~~
{: .language-r}

The `tidyverse` package acts as a "wrapper", which will install all of the packages that constitute the tidyverse.   If space is at a premium, individual packages from the tidyverse can be installed.  In practice, it's usually easier to install the whole thing.

## Loading the tidyverse

We can load the tidyverse using:

~~~
library(tidyverse)
~~~
{: .language-r}



~~~
── Attaching packages ────────────────────────────────── tidyverse 1.2.1 ──
~~~
{: .output}



~~~
✔ ggplot2 3.0.0     ✔ purrr   0.2.5
✔ tibble  1.4.2     ✔ dplyr   0.7.6
✔ tidyr   0.8.1     ✔ stringr 1.3.1
✔ readr   1.1.1     ✔ forcats 0.3.0
~~~
{: .output}



~~~
── Conflicts ───────────────────────────────────── tidyverse_conflicts() ──
✖ dplyr::filter() masks stats::filter()
✖ dplyr::lag()    masks stats::lag()
~~~
{: .output}

This loads the most commonly used packages in the tidyverse:

* `readr`: for reading data
* `ggplot2`: for plotting
* `tibble`: for creating "tibbles"; these are the tidyverse's take on data frames.
* `dplyr`: for manipulating tibbles (or data frames); creating new variables, calculating summary statistics etc.
* `tidyr`: for reshaping data (making it from long to wide format, and vice versa)
* `purrr`: for functional programming.
* `stringr`: for manipulating strings
* `forcats`: FOR CATegorical data (factors); this makes it easier to reorder and rename the levels in factor variables.

You can load each package you need for your analysis separately (e.g. `library(readr)`, etc.); most processes will involve almost all of them though, so I tend to load them all in one go.

There are other packages included in the tidyverse, but these have more specialised uses, and so need to be loaded separately. For example the [lubridate](https://lubridate.tidyverse.org/) package makes it easier to work with dates and times.  A full list of all the tidyverse packages can be found [here](https://www.tidyverse.org/packages/).


## Workshop overview

In this workshop we're going to work with some data from the [gapminder project](https://www.gapminder.org/).  The data consists of the population, GDP per capita and average life Expectancy of 142 countries, recorded at five year intervals.

We'll start by reading the data into R, using `readr`.  This will give us a `tibble`, which is the tidyverse's (improved) take on a data.frame; we'll briefly look at how to access parts of a tibble. We'll then start analysing the data, using `dplyr`.  We'll end the session using `ggplot2` (which is the tidyverse's plotting package) to explore the data graphically.

## Reading data

In this workshop we'll limit ourselves to using comma separated data.  We'll work with the gapminder data, which is included in [course data]({{ page.root }}/data/r-novice.zip).  

Although standard R has the ability to load csv files, using the `read.csv()` function, we'll use the functions in the readr Tidyverse package to load this data.   

Let's make a new script for this episode, by choosing the menu options _File_, _New File_, _R Script_.  We   should make our scripts self-contained, so we should include `library(tidyverse)` command at the start of the script.  We can read the data contained in the `gapminder-FiveYearData.csv` file to an object called `gapminder` with the following command:



~~~
library(tidyverse)

gapminder <- read_csv("./data/gapminder-FiveYearData.csv")
~~~
{: .language-r}



~~~
Parsed with column specification:
cols(
  country = col_character(),
  year = col_integer(),
  pop = col_double(),
  continent = col_character(),
  lifeExp = col_double(),
  gdpPercap = col_double()
)
~~~
{: .output}

We see that the `read_csv()` table reports a "column specification".  This shows the variable names that were read in, and the type of data that each column was interpreted as.


~~~
gapminder
~~~
{: .language-r}



~~~
# A tibble: 1,704 x 6
   country      year      pop continent lifeExp gdpPercap
   <chr>       <int>    <dbl> <chr>       <dbl>     <dbl>
 1 Afghanistan  1952  8425333 Asia         28.8      779.
 2 Afghanistan  1957  9240934 Asia         30.3      821.
 3 Afghanistan  1962 10267083 Asia         32.0      853.
 4 Afghanistan  1967 11537966 Asia         34.0      836.
 5 Afghanistan  1972 13079460 Asia         36.1      740.
 6 Afghanistan  1977 14880372 Asia         38.4      786.
 7 Afghanistan  1982 12881816 Asia         39.9      978.
 8 Afghanistan  1987 13867957 Asia         40.8      852.
 9 Afghanistan  1992 16317921 Asia         41.7      649.
10 Afghanistan  1997 22227415 Asia         41.8      635.
# ... with 1,694 more rows
~~~
{: .output}
When we enter `gapminder` by itself on the command line, it will print the contents of `gapminder``; we see that it consists of a `tibble`. A tibble is a way of storing tabular data, which is part of the tidyverse.   We see the variable names, and an (abbreviated) string indicating what type of data is stored in each variable.


> ## read_csv() vs read.csv()
>
> `read.csv()` is included as part of base R, and performs a similar job 
> to `read_csv()`.  We will be using `read_csv()` in this course; it is part of the tidyverse,
> so works well with other parts of the tidyverse, is faster than `read.csv()` and handles 
> strings in a way that is usually more useful than `read.csv()`
{: .callout}

> ## Loading other types of data
>
> Another type of file you might encounter are tab-separated value files (.tsv); these can be read with the `read_tsv()` function in the `readr` package.  To read files with other delimiters, use the `read_delim()` function. If files are fixed width format (i.e. the variable is defined by its position on the line), then use the `read_fwf()` function.
>
>  The tidyverse comes with several packages for loading data in other formats.  These include:
> * [readxl](http://readxl.tidyverse.org) for reading data from Excel spreadsheets
> * [haven](http://haven.tidyverse.org/) for reading SAS, SPSS and Stata data files
> * [xml2](http://xml2.tidyverse.org/) for reading xml data
> 
> These aren't loaded by default (when we use `library("tidyverse")`), so they will need to be loaded
> separately, e.g. `library("readxl")`, etc.  There are also tidyverse packages for getting data via 
> web APIs, or by "scraping" websites.  
{: .callout}


## Data types

Every piece of data in R is stored as either `double`, `integer`, `complex`, `logical` or `character`.
- `integer` variables can only store whole numbers
- `double` variables can store floating point numbers (i.e. with a decimal part)
- `complex` variables can store complex numbers (i.e. of the form `1+2i`)
- `logical` variables can store `TRUE` or `FALSE`
- `character` variables can store strings of characters.

When we read the data into
R using `read_csv()` it tries to work out what data type each variable is, which it does by looking at the data contained in the first 1000 rows of the data file.   We can see from the displayed message that `read_csv()` has treated the `country` variable as a character variable, the `gdpPercap` variable as a floating point number and `pop` variable as an integer variable.

We can override these guesses using the `col_types` argument to `read_csv()`.  Although `read_csv()` has correctly guessed the correct column types for our file, it is a good idea to explicitly tell it what sort of data to expect in each column.  This way our analysis will be robust to the wrong type of data being stored in a column (or to readr changing the algorithm it uses to guess data types).   

`read_csv()` helpfully formats the column specficiation in the format that the `col_types` argument expect, so we can cut and paste this into our command (editing it if we need to override any of the guesses). 


~~~
gapminder <- read_csv("./data/gapminder-FiveYearData.csv",
                      col_types = cols(
                        country = col_character(),
                        year = col_integer(),
                        pop = col_double(),
                        continent = col_character(),
                        lifeExp = col_double(),
                        gdpPercap = col_double()
                      ))
~~~
{: .language-r}


> ## Importing data using RStudio
> 
> You may have noticed when we viewed the `gapminder-FiveYearData.csv` file in RStudio, before importing it, that another option  appeared, labelled "Import Dataset".  This lets us import the data interactively.   It can be more convenient to use this approach, rather than manually writing the required code.   If you do this, you will find that the code RStudio has written is put into the console and run (and will appear in the history tab in RStudio).  It's fine to do this initially, but *you should copy the generated code to your script, so that you can reproduce your analysis*. 
{: .callout}

## Exploring tibbles

We can "unpick" the contents of a tibble in several ways.  We can return a vector containing the values
of a variable using the dollar symbol, `$`:


~~~
gapminder$pop
~~~
{: .language-r}

We can also use the subsetting operator `[]` directly on tibbles.  In contrast to a vector, a tibble
is two dimensional.   We pass two arguments to the `[]` operator; the first indicates the row(s) we 
require and the second indicates the columns.  So to return rows 1 and 2, and columns 2 and 3 we can use:


~~~
gapminder[1:2,2:3]
~~~
{: .language-r}



~~~
# A tibble: 2 x 2
   year     pop
  <int>   <dbl>
1  1952 8425333
2  1957 9240934
~~~
{: .output}

If we leave an index blank, this acts as a wildcard and matches all of the rows or columns:


~~~
gapminder[1,]
~~~
{: .language-r}



~~~
# A tibble: 1 x 6
  country      year     pop continent lifeExp gdpPercap
  <chr>       <int>   <dbl> <chr>       <dbl>     <dbl>
1 Afghanistan  1952 8425333 Asia         28.8      779.
~~~
{: .output}



~~~
gapminder[,1]
~~~
{: .language-r}



~~~
# A tibble: 1,704 x 1
   country    
   <chr>      
 1 Afghanistan
 2 Afghanistan
 3 Afghanistan
 4 Afghanistan
 5 Afghanistan
 6 Afghanistan
 7 Afghanistan
 8 Afghanistan
 9 Afghanistan
10 Afghanistan
# ... with 1,694 more rows
~~~
{: .output}

Subsetting a tibble returns another tibble; using `$` to extract a variable returns a vector:


~~~
gapminder$country
gapminder[,1]
~~~
{: .language-r}

> ## Tibbles vs data frames
> 
> Tibbles are used to represent tabular data in the tidyverse.  In contrast, base R
> uses data frames to represent tabular data. One of the differences between these 
> two types of obbject is *what* is returned when you extract a subset of rows/columns.
> In contrast to a tibble, taking a subset of a data frame doesn't always return
> another data frame. For more details see the callout at the end of this episode.
{: .callout}

## Writing data in R

We can save a tibble (or data frame) to a csv file, using `readr`'s `write_csv()` function.  For example, to save the gapminder data to `mygapminder.csv`:


~~~
write_csv(gapminder, "data/mygapminder.csv")
~~~
{: .language-r}

> ## Differences with base R
> 
> In this lesson we've taught you how to read files and make factors using the functionality in the
> `readr` package, which is part of the tidyverse.  
> This section highlights some of the differences between the tidyverse and its equivalent
> functionality in base R.
>
> R's standard data structure for tabular data is the `data.frame`.  In
> contrast, `read_csv()` creates a `tibble` (also referred to, for historic
> reasons, as a `tbl_df`).  This extends the functionality of  a `data.frame`,
> and can, for the most part, be treated like a `data.frame`
> 
> You may find that some older functions don't work on tibbles.   A tibble
> can be converted to a dataframe using `as.data.frame(mytibble)`.  To convert
> a data frame to a tibble, use `as.tibble(mydataframe)`
> 
> Tibbles behave more consistently than data frames when subsetting with `[]`; 
> this will always return another tibble.  This isn't the case when working with 
> data.frames.  You can find out more about the differences  between data.frames and 
> tibbles by typing `vignette("tibble")`.
> 
> `read_csv()` will always read variables containing text as character variables.  In contrast,
> the base R function `read.csv()` will, by default, convert any character variable to a factor.
> This is often not what you want, and can be overridden by passing the option `stringsAsFactors = FALSE` to `read.csv()`.  
>
> We used `parse_factor()` to define factors.  The base R equivalent is the `factor()` function.
> The main differences between the two approaches are:
>
> * `factor()` will assign factor levels automatically; it does not require us to pass `levels = NULL`.
> * The automatic levels generated by `factor()` will be alphabetical (rather than according to the order
> that each level is encountered in `parse_factor()`)
> * `factor()` does not warn us if we have data that doesn't match any of the levels we have specified
> 
> It is chiefly for the final reason that we recommend using `parse_factor()` instead of `factor()`.  You
> should be aware that the default ordering of factor levels differs between `factor()` and `parse_factor()`
{: .callout}
