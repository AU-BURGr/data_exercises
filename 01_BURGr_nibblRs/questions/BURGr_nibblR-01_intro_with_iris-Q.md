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

| Species    |  Sepal.Length.mean|  Sepal.Width.mean|  Petal.Length.mean|  Petal.Width.mean|
|:-----------|------------------:|-----------------:|------------------:|-----------------:|
| setosa     |              5.006|             3.428|              1.462|             0.246|
| versicolor |              5.936|             2.770|              4.260|             1.326|
| virginica  |              6.588|             2.974|              5.552|             2.026|

**Part 2:**<a id="part2"></a> The aim is similar to the aim in [part 1](#part1) but the difference is to also include information about the standard deviation (sd).

Hint: Is this a mind meld or a merge op?

| Species    |  Petal.Length.mean|  Petal.Length.sd|  Petal.Width.mean|  Petal.Width.sd|  Sepal.Length.mean|  Sepal.Length.sd|  Sepal.Width.mean|  Sepal.Width.sd|
|:-----------|------------------:|----------------:|-----------------:|---------------:|------------------:|----------------:|-----------------:|---------------:|
| setosa     |              1.462|        0.1736640|             0.246|       0.1053856|              5.006|        0.3524897|             3.428|       0.3790644|
| versicolor |              4.260|        0.4699110|             1.326|       0.1977527|              5.936|        0.5161711|             2.770|       0.3137983|
| virginica  |              5.552|        0.5518947|             2.026|       0.2746501|              6.588|        0.6358796|             2.974|       0.3224966|

**Part 3:**<a id="part3"></a> In [part 1](#part1) and [part 2](#part2), we were able to create pretty decent summaries of Species data in **iris** according to both the average and standard deviation.

Hmmm... something is missing? can we make it more "publication-worthy" (sic)... yes we can!

Hint: I sense some **function**al pasting... :alien:

| Species    | Sepal.Length | Sepal.Width | Petal.Length | Petal.Width |
|:-----------|:-------------|:------------|:-------------|:------------|
| setosa     | 5.01 ± 0.35  | 3.43 ± 0.38 | 1.46 ± 0.17  | 0.25 ± 0.11 |
| versicolor | 5.94 ± 0.52  | 2.77 ± 0.31 | 4.26 ± 0.47  | 1.33 ± 0.2  |
| virginica  | 6.59 ± 0.64  | 2.97 ± 0.32 | 5.55 ± 0.55  | 2.03 ± 0.27 |

**Part 4:** This bite sized chunk of BURGr goodness is complete, thanks for joining us! The open question is, can you think of other ways to create a simple, pretty **tabular** summary of this dataset?
