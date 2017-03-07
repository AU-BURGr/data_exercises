BURGr bite 1: Basic data manipulation with Iris
===============================================

Hello all BURGr lovers! Welcome to the first BURGr bites exercise :smile:. The dataset that we are working with in this exercise is the beloved iris, and our goal is to create a simple, beautiful summary of the data.

Before we dive into things, let's get an idea of what we are dealing with...

``` r
data(iris)
knitr::kable(head(iris))
```

|  Sepal.Length|  Sepal.Width|  Petal.Length|  Petal.Width| Species |
|-------------:|------------:|-------------:|------------:|:--------|
|           5.1|          3.5|           1.4|          0.2| setosa  |
|           4.9|          3.0|           1.4|          0.2| setosa  |
|           4.7|          3.2|           1.3|          0.2| setosa  |
|           4.6|          3.1|           1.5|          0.2| setosa  |
|           5.0|          3.6|           1.4|          0.2| setosa  |
|           5.4|          3.9|           1.7|          0.4| setosa  |

**Part 1:**<a id="part1"></a> The aim is to produce a simple summary of the iris dataset by producing a new dataset from iris that contains the average (i.e. mean ) value of each of the variables (columns) according to species.

Hint: I wonder how one could aggregate this?

``` r
# work out which column is the "Species" variable
speciesCol = grep("Species", names(iris))
# aggregate iris by species excluding the non-numeric "Species" variable
mean_iris = aggregate(x = iris[, -speciesCol], by = list(iris$Species), FUN = mean, na.rm= T)

# we need to rename all of the columns
names(mean_iris)[1] = "Species"
names(mean_iris)[-1] = paste0(names(mean_iris)[-1], ".mean")
# display pretty table using kable function from knitr
knitr::kable(mean_iris)
```

| Species    |  Sepal.Length.mean|  Sepal.Width.mean|  Petal.Length.mean|  Petal.Width.mean|
|:-----------|------------------:|-----------------:|------------------:|-----------------:|
| setosa     |              5.006|             3.428|              1.462|             0.246|
| versicolor |              5.936|             2.770|              4.260|             1.326|
| virginica  |              6.588|             2.974|              5.552|             2.026|

**Part 2:**<a id="part2"></a> The aim is similar to the aim in [part 1](#part1) but the difference is to also include information about the standard deviation (sd).

Hint: Is this a mind meld or a merge op?

``` r
# work out which column is the "Species" variable
speciesCol = grep("Species", names(iris))
# aggregate iris by species excluding the non-numeric "Species" variable
sd_iris = aggregate(x = iris[, -speciesCol], by = list(iris$Species), FUN = sd, na.rm= T)

# we need to rename all of the columns
# warning: we are cheating a bit because we know that "Species" is the first variable
# However, most times we will not have this luxury. 
# Question: What would you do then?
names(sd_iris)[1] = "Species"
names(sd_iris)[-1] = paste0(names(sd_iris)[-1], ".sd")

# merge mean and sd data summaries
iris_snapshot = merge(mean_iris, sd_iris, by = "Species")

# similar strategy as above...same caveat (warning)
# Answer: Use grep to avoid hard coding by replacing the 
# number 1 with the results of the grep() search!
speciesCol = grep("Species", names(iris_snapshot))
sortedColNames = c(names(iris_snapshot)[speciesCol], sort(names(iris_snapshot)[-speciesCol]))

# reorganise data by column
iris_snapshot = iris_snapshot[, sortedColNames]

# display pretty table using kable function from knitr
knitr::kable(iris_snapshot)
```

| Species    |  Petal.Length.mean|  Petal.Length.sd|  Petal.Width.mean|  Petal.Width.sd|  Sepal.Length.mean|  Sepal.Length.sd|  Sepal.Width.mean|  Sepal.Width.sd|
|:-----------|------------------:|----------------:|-----------------:|---------------:|------------------:|----------------:|-----------------:|---------------:|
| setosa     |              1.462|        0.1736640|             0.246|       0.1053856|              5.006|        0.3524897|             3.428|       0.3790644|
| versicolor |              4.260|        0.4699110|             1.326|       0.1977527|              5.936|        0.5161711|             2.770|       0.3137983|
| virginica  |              5.552|        0.5518947|             2.026|       0.2746501|              6.588|        0.6358796|             2.974|       0.3224966|

**Part 3:**<a id="part3"></a> In [part 1](#part1) and [part 2](#part2), we were able to create pretty decent summaries of Species data in **iris** according to both the average and standard deviation.

Hmmm... something is missing? can we make it more "publication-worthy" (sic)... yes we can!

Hint: I sense some **function**al pasting... :alien:

``` r
# just to mix things up... we will create a new data.frame object
# using existing data

# HTML character code from here: https://www.w3schools.com/charsets/ref_html_entities_4.asp
# needed to add the ability to round down the sd
# makePlusMinus = function(x, y) return(paste(x, "&#177;", y))
makePlusMinus = function(x, y) return(paste(round(x, 2), "&#177;", round(y, 2)))

# create a new data frame out using the "Species" column from existing data
iris_plus = mean_iris["Species"]
# use the custom makePlusMinus to make pretty summaries of the data
iris_plus$Sepal.Length = makePlusMinus(mean_iris$Sepal.Length.mean, sd_iris$Sepal.Length.sd)
iris_plus$Sepal.Width = makePlusMinus(mean_iris$Sepal.Width.mean, sd_iris$Sepal.Width.sd)
iris_plus$Petal.Length = makePlusMinus(mean_iris$Petal.Length.mean, sd_iris$Petal.Length.sd)
iris_plus$Petal.Width = makePlusMinus(mean_iris$Petal.Width.mean, sd_iris$Petal.Width.sd)

# display pretty table using kable function from knitr
knitr::kable(iris_plus)
```

| Species    | Sepal.Length | Sepal.Width | Petal.Length | Petal.Width |
|:-----------|:-------------|:------------|:-------------|:------------|
| setosa     | 5.01 ± 0.35  | 3.43 ± 0.38 | 1.46 ± 0.17  | 0.25 ± 0.11 |
| versicolor | 5.94 ± 0.52  | 2.77 ± 0.31 | 4.26 ± 0.47  | 1.33 ± 0.2  |
| virginica  | 6.59 ± 0.64  | 2.97 ± 0.32 | 5.55 ± 0.55  | 2.03 ± 0.27 |

**Part 4:** This bite sized chunk of BURGr goodness is complete, thanks for joining us! The open question is, can you think of other ways to create a simple, pretty **tabular** summary of this dataset?
