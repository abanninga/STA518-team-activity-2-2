---
title: Activity 2.2
output: html_document
---

# Exploratory Data Analysis: Grouped Summarizing and Manipulating

Requirements:

- GitHub account
- RStudio Cloud account

Goals:

- Explore a data set by creating grouped summary tables
- Manipulate or create new variables for specific grouped data values

**Pick a lead**:
This person is not solely responsible for doing the activity, but they are responsible for organizing the collective team effort - for example, making sure all parts are completed and pushing them to the Team GitHub repo.

**Tips**:

- Be ready to troubleshoot.
  Errors will more than likely be a knit issue.
  Read the error message carefully and take note of which line is preventing a successful knit.
- Keep track of the various code chunks and keep your text and code in the right place.
- Remember that your R Markdown document file is not aware of your project's global environment (e.g., the *Console*) and can only make use of variables, functions, etc. that you have loaded or defined in the document.
- If you are unsure of how a function works or what its arguments are, type `?` and the function in the *Console* and press enter (e.g., `?read_csv`).
  The *Help* tab will provide a summary of the function as well as some examples.

## Introduction

In this activity, we are going to extend the `dplyr` functions for wrangling data sets based on groupings of observations using the `gapminder` data set.
We used some very important verbs and operators in Activity 2.1 - Exploratory Data Analysis: Summarizing and Manipulating when working with a single data set.

**Thought Question 1**:
As a team, describe the main purpose for each of the following verbs and operators:

- `arrange`
- `select`
- `mutate`
- `summarise`
- `%>%`

## Getting started

Now that your Teams are set-up in GitHub Classroom, these instructions will be how we access Team Activities from now on.
For now, we are still relying on only one Team Member (the lead) pushing the complete activity, but the others are expected to contribute to the discussion and help the lead member complete the step, but not *push* the files from their computer (they should be able to pull, though).

2. *All* Team Members:
  - Go to the Documents section on [Bb](https://mybb.gvsu.edu)
  - Click on the link titled `activity0202`
  - Click on the "Join" button next to your corresponding team name in the **Join an existing team** section
3. *All* Team Members now will:
  - In your team repo, click the green **Clone or download** button, select "Use HTTPS" if this isn't the default option, and click on the clipboard icon to copy the repo URL
  - Go to RStudio Cloud and into the course workspace.  Create a **New Project from Git Repo** - remember that you need to click on the down arrow next to the **New Project** button to see this option.
  - Paste the URL of your activity repo into the dialogue box.
  - Click "OK".
4. *All* Team Members now will Load Packages:
  - In this lab, we will work with the `tidyverse` and `gapminder` packages so we need to install and load it.
    Type the following code into your *Console*:
  
    ``` 
    install.packages("tidyverse")
    library(tidyverse)
    ```
    
    ``` 
    install.packages("gapminder")
    library(gapminder)
    ```
    
  - Note that these packages are also loaded in your R Markdown document.
5. *All* Team Members now will configure Git:
  - Go to the *Terminal* pane and type the following two lines of code, replacing the information inside the quotation marks with your GitHub account information:
  
    ```
    git config --global user.email "your email"
    git config --global user.name "your name"
    ```
    
  - Confirm that these changes have been implemented, run the following code:
  
    ```
    git config --global user.email
    git config --global user.name
    ```
        
  - Inform git that you want to store your GitHub credentials for $60 \times 60 \times 24 \times 7 = 604,800$ seconds, run the next line of code.  This needs to be done on a per-project basis.
  
    ```
    git config --global credential.helper 'cache --timeout 604800'
    ```
    
6. *All* Team Members will now name their RStudio project:
  - Currently your RStudio project is called *Untitled Project*.  Update the name of your project to be "Activity 2-2 - Grouped Summarizing and Manipulating"
7. The *Lead* Team Member to do the following in RStudio:
  - Open the `.Rmd` file and update the **YAML** by changing the author name to your **Team** name and date to today, then knit the document.
  - Go to the *Git* pane and click on **Diff** to confirm that you are happy with the changes.
  - *Stage* just your Rmd file, add a commit message like "Updated team name" in the *Commit Message* dialogue box and click **Commit**
  - Click on **Push**.  This will prompt a dialogue where you first need to enter your GitHub user name, and then your password - this should be the only time you need to do this for the current activity.
  - Verify that your changes have been made to your GitHub repo.
8. *All other* Team Members now will do the following in RStudio:
  - Go to the *Git* pane and click on **Pull** button.  This will prompt a dialogue where you first need to enter your GitHub user name, and then your password - this should be the only time you need to do this for the current activity.
  - Observe that the changes are now reflected in their project!

Again, only one team member will be pushing the changes.
All others are encouraged to work and save changes "locally" in RStudio.Cloud, but not push.

## The data

The `gapminder` data comes from [gapminder.org](https://www.gapminder.org/data/) through the amazing [Jenny Bryan](https://github.com/jennybc/gapminder).

Throughout this activity, I want you to pay close attention to i) when we evaluate statements and let the output just print to screen, versus ii) when we assign the output(`<-`) to an object (possibly overwriting an existing object).

We are going to make changes to the `gapminder` tibble.
Therefore, we should create an explicit copy to eliminate any fear that we are damaging the data that comes with the package.
Note that this isn't really a concern in this situation because you do not have "write access" for the data, but this will be a good habit to start.

I provided you with the following code in your starter `.Rmd` file.

``` 
(my_gm <- gapminder)
```

**Thought Question 2**:
What do the parentheses surrounding this code do?
Try running it with and without the parentheses and see what is different.

### The data codebook

Descriptions of the variables are provided are provided in the gapminder help documentation (i.e., `?gapminder`).

***
**Exercise 1**:
Using the information provided in the help documentation, create a data description table (with your markdown skills).
Your table should have two columns (*variable* and *description*) and a row for each variable.
Note that the help documentation doesn't provide much detail for all variables so you might need to come up with your own brief descriptions.

***

The end goal of exploring which countries had the sharpest 5-year drop in life expectancy.
However, we will at times appear to drift away from this to work through some data manipulation strategies.
Specifically, we will do the following as we try to answer this:

- Create a new variable that are functions of existing variables
- Create a new variable that are concatenations of existing variables
- Summarise information into one quantity for subgroups of interest

## Data wrangling - adding new variables

### Adding new quantitative variables

First let's recover each country's GDP.
The Gapminder data has a variable for population and GDP per capita.
Therefore, we can multiply these two variables to get a country's GDP.
Remember that the `mutate` function allows us to create new variables.

``` 
my_gm %>% 
  mutate(gdp = pop * gdpPercap)
```

These GDP numbers are extremely large.
Perhaps converting these numbers to GDP in billions might be more useful.

***
**Exercise 2**:
In the `create-gdp` code chunk, create a new variable called `gdpBillion` that displays `gdp` in billions, rounded to two decimals.
Do not use another `mutate` pipe.
Rather, complete this within the existing one.
Note that `mutate` builds new variables sequentially so you can reference earlier ones (like `gdp`) when defining later ones (like `gdpBillion`).

***

**Aside**: Sometimes, you may want to create temporary intermediate-step variables (say `tmp`), then want to get rid of them.
This can be done by setting it to `NULL` (e.g., `tmp = NULL`)

These numbers still seem to be rather abstract.
[Randall Munrow of xkcd](https://fivethirtyeight.com/features/xkcd-randall-munroe-qanda-what-if/) offers some advice when dealing with large numbers:

> One thing that bothers me is large numbers presented without context… ‘If I added a zero to this number, would the sentence containing it mean something different to me?’ If the answer is ‘no,’ maybe the number has no business being in the sentence in the first place."

Perhaps more meaningful values would be those obtained using the following code:

``` 
us_tib <-  my_gm %>%
  filter(country == "United States")
## This is a semi-dangerous way to do this variable
## I'd prefer to join on year, but we haven't covered joins yet (Activity 2.4)
my_gm <-  my_gm %>%
  mutate(tmp = rep(us_tib$gdpPercap, nlevels(country)),
         gdpPercapRel = gdpPercap / tmp,
         tmp = NULL)
```

***
**Exercise 3**:
First, describe (step-by-step) what the `unnamed-chunk` does.
Second, update the chunk with a more meaningful name.
Third, add a new chunk with the following code (and a meaningful chunk name) that verifies that the previous code chunk provides correct values and describe how you know these values are correct.

``` 
my_gm %>% 
  filter(country == "United States") %>% 
  select(country, year, gdpPercapRel)
```

***

It is safe to say that the United States are a "high GDP" country, so we can easily predict that the distribution of `gdpPercapRel` is located below 1 - possibly even well below.

***
**Exercise 4**:
Check our intuition by plotting `gdpPercapRel` using a histogram (Activity 2.1 can help you with this if you do not remember).

***

This brings up a good motto: Trust No One - including (especially?) yourself.
When you are working with data, find a way to check that you have done what you meant to do.

### Adding new character variables

You saw how the `ifelse` and `mutate` functions could be used in Activity 2.1 to create new variables.
Another useful function when dealing with character variables is the `paste` function.
For example, if we wanted a new variable called `country_continent` that combines each country with their continent, separated by a dash we could do

``` 
my_gm %>% 
  mutate(country_continent = paste(country, continent, sep = " - "))
```

Note that this *does not* assign the `country_continent` variable in `my_gm`.
If you wanted to do this, you would need to copy over the tibble `my_gm <- ...`

## Rename and reposition variables

In this section I just provide you with some interesting `dplyr` verb usage that are frequently used.

### `rename` variables

Jenny Bryan mentions that she first cleaned the Gapminder data when she was a `camelCase` person, but is now a proud member of the `snake_case` crew.
To change variable names, the `rename` function is one method:

``` 
my_gm %>% 
  rename(life_exp = lifeExp,
         gdp_percap = gdpPercap,
         gdp_percap_rel = gdpPercapRel)
```

This doesn't change the variable names in `my_gm`.
However, this could be done when the data is originally loaded, then proceed with the new variable names.
We do not need to worry about that with this activity and will continue with the `camelCase`.

### Rename and reposition with `select`

You saw how to use `select` to specify which variables to keep in your output.
There are two other tricks that may be useful:

1. `select` can rename variables that you request to keep.
2. `select` can be used with `everything` to bring a variable to the front of the tibble.

``` 
my_gm %>% 
  filter(country == "Burundi", year > 1996) %>% 
  select(yr = year, lifeExp, gdpPercap) %>% 
  select(gdpPercap, everything())
```

`everything` is one of several tidyverse helper functions.
Read its help to see the rest.

***
**Exercise 5**:
Show all columns in the gap minder data set that start with the letter "c".

***

## `group_by`

Back to our original question: Which countries experienced the sharpest 5-year drop in life expectancy?

This is a natural, seemingly simple enough question, but if we were using a programming language that doesn't understand data, it would be incredibly annoying to answer.

`dplyr` has powerful tools to handle data questions (this table was reproduced from `@vincenzocoia`'s STAT545):

| Function type | Explanation | Examples | In `dplyr` |
|------|-----|----|----|
| Vectorized functions | These take a vector, and operate on each component to return a vector of the same length (i.e., element-wise). | `cos`, `sin`, `log`, `exp`, `round` | `mutate` |
| Aggregate functions | These take a vector, and return a vector of length 1 | `mean`, `sd`, `length`, `typeof` | `summarize` in combination with `group_by`. |
| Window functions | these take a vector, and return a vector of the same length that depends on the vector as a whole. | `lag`, `rank`, `cumsum` | `mutate` in combination `group_by` |

Later in this course we will see how to perform general computations using the `do` function and also the `purrr` package.


### Counting by groups

There are special functions that `summarise` can use:

- `n()`: Number of rows in the group
- `n_distinct()`;

and some convenience functions:

- `tally()` (= `summarise(n = n())`)
- `count(...)` (= `group_by(...) %>% tally()`).

For example, the next three pipelines all produce the same output.

``` 
my_gm %>%
  group_by(continent) %>%
  summarise(n = n())
```

``` 
my_gm %>%
  group_by(continent) %>%
  tally()
```

``` 
my_gm %>%
  count(continent)
```

The benefit of using the `summarise` method is that you can compute multiple summaries inside of it.

***
**Exercise 5**:
Add the number of distinct countries within each continent to the appropriate summarization pipeline by using the `n_distinct` function.
Call this summary `n_countries`.

***

### General summarizations

The `summarise_at` applies the same summary function(s) to multiple variables.
The following code computes the average and median life expectancy and GDP per capita by continent by year, but only for 1952 and 2007.

``` 
my_gm %>%
  filter(year %in% c(1952, 2007)) %>%
  group_by(continent, year) %>%
  summarise_at(vars(lifeExp, gdpPercap), funs(mean, median))
```

***
**Exercise 6**:
Compute the minimum and maximum life expectancy for the countries in Asia during each year.
Be sure to comment on what you notice.

***

## Grouped mutate

Unlike `summaries` which collapses the $n$ rows for each group into one row, sometimes we wish to compute within each group.

### Computing with group-wise summaries

If we wanted to see the growth in population since the first year of record *for each country*, we can use the `first` function as follows:

``` 
my_gm %>% 
  group_by(country) %>% 
  select(country, year, pop) %>% 
  mutate(pop_gain = pop - first(pop)) %>% 
  filter(year < 1963)
```

Note that the `filter` pipe just reduces the amount of information shown to easily verify the values.

This pipeline looks within a country (`group_by(country)`), we take the difference between population in year $i$ and life expectancy in 1952 (`mutate(pop_gain = pop - first(pop))`).
We should always see zeroes for 1952 and usually see a sequence of positive and increasing numbers.

***
**Exercise 7**:
Calculate the growth in life expectancy compared to `1972` *for each country*.

***

### Window functions

As the table above stated, **window functions** take $n$ inputs and give back $n$ outputs.
These outputs also depend on all of the values.
The `rank` function is an example of a window function, but the `mean` function is not.

In Exercise 5 you obtained the minimum and maximum life expectancies for Asia during each year.
Let's now figure our *which* country contributes these extreme values.

```
my_gm %>%
  filter(continent == "Asia") %>%
  select(year, country, lifeExp) %>%
  group_by(year) %>%
  filter(min_rank(desc(lifeExp)) < 2 | min_rank(lifeExp) < 2) %>% 
  arrange(year) %>%
  print(n = Inf)
```


***
**Exercise 8**:
First, after running the pipeline in your activity and reviewing the output, what does the second `filter` statement do?
Why can't this be done earlier?

Next, comment on what you notice about the output.
Are there any trends?

Finally, comment on how the output is presented.
Are you satisfied with how the information is presented or do you have any recommendations for a better presentation?

***

**Note**: If we had only wanted the min OR max for each year, an alternative approach could be obtained using `top_n`:

```
my_gm %>%
  filter(continent == "Asia") %>%
  select(year, country, lifeExp) %>%
  arrange(year) %>%
  group_by(year) %>%
  #top_n(1, wt = lifeExp)        ## gets the min
  top_n(1, wt = desc(lifeExp)) ## gets the max
```

Now, let's get back to our question:

***
**Exercise 9**:
Part 1: Which five countries had the sharpest 5-year drop in life expectancy?
The `lag` function will be useful in completing this exercise.
Be sure to show the top five countries (really the bottom five), their continent and year (to help you with the second part of this question), and the value of the change in life expectancy.

Part 2: Ponder your results from Part 1.
What unaccounted for reasons (i.e., not included in the data set) explain these drops in life expectancies?

***

Have your **Lead Team Member** commit and push the final activity to your Team's repo.