# Making a start with R

*This lesson is partially derived from Data Carpentry teaching materials available under the [CC BY 4.0 license](https://creativecommons.org/licenses/by/4.0/legalcode):*

[https://datacarpentry.org/R-ecology-lesson/](https://datacarpentry.org/R-ecology-lesson/)

This hour-long intro to R is based on materials used in the 'Data Analysis with R' course offered by CSC. The course home page can be found [here](https://github.com/csc-training/da-with-r).

**Objectives**

- Understand steps involved in a typical R workflow

- Find your way around RStudio

- Get a feel for how R code works

- Import an example data set into R

- Know how to create a scatter plot using `ggplot2`

#### 1. Typical workflow

Say that we have just obtained a lot of data from an experiment and we want to use R to make sense of it all. Where to start?

We can outline a typical data analysis workflow as follows:

![](Images/workflow.png?raw=true)

#### 2. R and RStudio

- R is an [open-source programming language](https://cran.r-project.org/) for data analysis and visualization. It works on all major platforms (Windows, macOS and Linux).

- At its core, R code is text-based (commands can be run without a GUI).

- R can also be run via an open-source interface such as [RStudio](http://www.rstudio.com). 

- RStudio provides a built in-editor and offers many advantages including ease of use and project management.

During this workshop we will be using RStudio Server, which lets us access a remote instance of R via a web browser.

#### 3. R Data Analysis on notebooks.csc.fi

To launch your own instance of R:

1. Go to [notebooks.csc.fi](https://notebooks.csc.fi) and log in using the Haka authentication service.

2. Click on 'Account' and then 'Join Group'.

3. Click 'Join' after adding this joining code: **cscdataanalysisr-9c4er** 

4. Find **R Data Analysis** in the dashboard and click on 'Launch new'. Note: there is also an environment called RStudio Server, but we won't use that.

5. Wait for the virtual machine to start and click on 'Open in browser' once the link appears.

6. Your browser will open a new window with login and password details. Press 'Click to copy password & proceed'.

7. This will open a new tab where you can enter a username ('rstudio') and paste your password to open up a new RStudio session.

#### 4. Basic layout

When you first open RStudio, you will be greeted by three panels:

- The interactive R console (entire left)
- Environment / History (tabbed in upper right)
- Files / Plots / Packages / Help / Viewer (tabbed in lower right)

Your screen will look similar to this:

![](Images/01-rstudio.png?raw=true)

Once you open files, such as R scripts, an editor panel will also open in the top left.

![](Images/01-rstudio-script.png?raw=true)

#### 5. Executing code in RStudio

There are two main ways one can work within RStudio:

2. Test and play within the interactive R console
   - This works well when doing small tests
3. Write using the script editor
   - Makes it possible to save your code for later (as an .R file)
   - Far easier to keep track of long segments of code

RStudio offers great flexibility in running code from within the editor window. To run the current line, you can:

2. Click on the `Run` button above the editor panel, or
3. Select `Run Selected Lines` from the `Code` menu, or
4. Hit `Ctrl + Return` in Windows or Linux or `⌘ + Return` on OS X.

#### 6. Objects

You can get output from R simply by typing calculations in the console:

```r
3 + 5
12 / 7
```

Code without spaces (e.g. `1+1`) also works. For decimal points, R accepts the full stop symbol (`.`) instead of a comma (`,`).

To do useful and interesting things in R, we can assign *values* to *objects*. To create an object, we give it a name followed by the assignment operator `<-`, and a value. Let's take an animal's weight as an example:

```r
weight_kg <- 55
```

`<-` is the assignment operator. It assigns values on the right to objects on the left. So, after executing `x <- 3`, the value of `x` is `3`. The arrow can be read as 3 **goes into** `x`. For historical reasons, you can also use `=` for assignments, but not in every context. Because of the [slight](http://blog.revolutionanalytics.com/2008/12/use-equals-or-arrow-for-assignment.html) [differences](http://r.789695.n4.nabble.com/Is-there-any-difference-between-and-tp878594p878598.html) in syntax, it is good practice to always use `<-` for assignments.

In RStudio, typing `Alt` + `-` (push Alt at the same time as the - key) will write `<-` in a single keystroke in a PC, while typing `Option` + `-` does the same in a Mac.

**Note:** While previously we could immediately see the result of a calculation, when using objects the result is not immediately visible. We can access the output by running `weight_kg` on its own:

```r
weight_kg
```

Now that R has `weight_kg` in memory, we can do arithmetic with it. For instance, we may want to convert this weight into pounds (weight in pounds is 2.2 times the weight in kg):

```r
2.2 * weight_kg
```

We can also change an object’s value by assigning it a new one:

```r
weight_kg <- 57.5
2.2 * weight_kg
```

This means that assigning a value to one object does not change the values of other objects For example, let’s store the animal’s weight in pounds in a new object, `weight_lb`:

```r
weight_lb <- 2.2 * weight_kg
```

and then change `weight_kg` to 100.

```r
weight_kg <- 100
```

What do you think is the current content of the object `weight_lb`?

126.5 or 220?

#### 7. Code annotation

The comment character in R is `#`, anything to the right of a `#` in a script will be ignored by R. It is useful to leave notes and explanations in your scripts.

```r
mass <- 47.5 # creating an object called mass
```

#### 8. Functions and their arguments

Functions are “canned scripts” that automate more complicated sets of commands including operations and assignments etc. Many functions are predefined, or can be made available by importing R packages. Packages contain user-developed sets of functions and greatly expand the availability of different data analysis tools in R.

A function usually takes one or more inputs called *arguments*. Functions often return a *value*. An example would be the function `sqrt()`. The input must be a number, and the return value is the square root of that number.

```r
b <- sqrt(4)
```

Some functions take arguments which may be specified by the user, or, if left out, take on a *default* value: these are called *options*. Options are typically used to alter the way the function operates, e.g. what symbol to use in a plot. If you want something specific, you can specify a value of your choice which will be used instead of the default.

Let’s try a function that can take multiple arguments: `round()`.

```r
round(3.14159)
#> [1] 3
```

Here, we’ve called `round()` with just one argument, `3.14159`, and it has returned the value `3`. That’s because the default is to round to the nearest whole number.

If we wanted more digits, we could specify the option `digits` inside `round`:

```r
round(3.14159, digits = 2) # rounding to two digits
```

#### 9. Downloading and importing data

We can now use the R function `download.file()` to download a CSV file that contains a large amount of biological survey data. We will then use `read.csv()` to load into memory the content of the CSV file.

```r
download.file(url = "https://tinyurl.com/surveyscomplete", 
              destfile = "surveys_complete.csv")

surveys_complete <- read_csv("surveys_complete.csv")

# the tinyurl links to: 
# https://raw.githubusercontent.com/csc-training/da-with-r/master/DataFiles/surveys_complete.csv
```

We can have a quick look at the data using `str()` and `head()`:

```r
str(surveys_complete)
head(surveys_complete)
```

#### 10. Creating a scatterplot using `ggplot2`

`ggplot2` is a plotting package that makes it simple to create complex plots in R. It comes as part of the `tidyverse` package collection, which we can load as follows:

```r
library(tidyverse)
```

`ggplot2` graphics are built step by step by adding new elements. To build a `ggplot2` plot, we will use the following basic template that can be used for different types of plots:

```r
ggplot(data = <DATA>, 
       mapping = aes(<MAPPING>)) + 
       <GEOM_FUNCTION>()
```

- use the `ggplot()` function and bind the plot to a specific data set using the `data` argument

```r
ggplot(data = surveys_complete)
```

- define a mapping (using the `aes` function) by selecting the variables to be plotted and specifying how to present them in the graph (e.g. as x/y positions or characteristics such as size, shape and color).

```r
ggplot(data = surveys_complete, 
mapping = aes(x = weight, y = hindfoot_length, color = species_id))

# we're telling R that we want to create a plot with weight on the x axis, hindfoot length on the y axis, with colors according to the species
```

- add ‘geoms’ – graphical representations of the data in the plot (points, lines, bars). `ggplot2` offers many different geoms:
  
  ```r
  # geom_point() for scatter plots, dot plots, etc.
  # geom_bar() for bar plots
  # geom_boxplot() for box plots
  # geom_line() for trend lines, time series, etc.  
  ```

To add a geom to the plot use the `+` operator. Because we have two continuous variables, let’s use `geom_point()` first.

```r
ggplot(data = surveys_complete, mapping = aes(x = weight, 
y = hindfoot_length, color = species_id)) +
  geom_point()
```

![](Images/plot1.png?raw=true)

Excellent! This plot is already looking good. If we wanted to use it e.g. as part of a report or publication, we could do some further work to modify the appearance (such as changing the y axis label and overall font sizes).

- **Note:** The `+` sign used to add new layers must be placed at the end of the line containing the *previous* layer. If, instead, the `+` sign is added at the beginning of the line containing the new layer, `ggplot2` will not add the new layer and will return an error message.

```r
# This is the correct syntax for adding layers
surveys_plot +
  geom_point()

# This, on the other hand, will return an error message
surveys_plot
  + geom_point()
```

#### 11. Extra: box plots

If there's time, we could also use box plots to summarize the distribution of weight within each species:

```r
ggplot(data = surveys_complete, mapping = aes(x = species_id, 
y = weight)) +
    geom_boxplot()
```

![](Images/R-ecology-boxplot-1.png?raw=true)

By adding points to boxplot, we can have a better idea of the number of measurements and their distribution.

```r
ggplot(data = surveys_complete, mapping = aes(x = species_id, y = weight)) +
    geom_boxplot(alpha = 0) +
    geom_jitter(alpha = 0.3, color = "tomato")

# geom_jitter is like geom_point but adds some random
# variation to the position of each point
```

![](Images/R-ecology-boxplot-with-points-1.png?raw=true)

Notice how the boxplot layer is behind the jitter layer? What do you need to change in the code to put the boxplot in front of the points such that it’s not hidden?
