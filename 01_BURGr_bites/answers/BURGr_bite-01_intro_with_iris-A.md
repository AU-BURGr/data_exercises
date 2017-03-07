Hello all BURGr lovers! Welcome to the first BURGr bites exercise
:smile:. The dataset that we are working with in this exercise is the
beloved iris, and our goal is to create a simple, beautiful summary of
the data.

Before we dive into things, let's get an idea of what we are dealing
with...

    data(iris)
    knitr::kable(head(iris))

<table>
<thead>
<tr class="header">
<th align="right">Sepal.Length</th>
<th align="right">Sepal.Width</th>
<th align="right">Petal.Length</th>
<th align="right">Petal.Width</th>
<th align="left">Species</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="right">5.1</td>
<td align="right">3.5</td>
<td align="right">1.4</td>
<td align="right">0.2</td>
<td align="left">setosa</td>
</tr>
<tr class="even">
<td align="right">4.9</td>
<td align="right">3.0</td>
<td align="right">1.4</td>
<td align="right">0.2</td>
<td align="left">setosa</td>
</tr>
<tr class="odd">
<td align="right">4.7</td>
<td align="right">3.2</td>
<td align="right">1.3</td>
<td align="right">0.2</td>
<td align="left">setosa</td>
</tr>
<tr class="even">
<td align="right">4.6</td>
<td align="right">3.1</td>
<td align="right">1.5</td>
<td align="right">0.2</td>
<td align="left">setosa</td>
</tr>
<tr class="odd">
<td align="right">5.0</td>
<td align="right">3.6</td>
<td align="right">1.4</td>
<td align="right">0.2</td>
<td align="left">setosa</td>
</tr>
<tr class="even">
<td align="right">5.4</td>
<td align="right">3.9</td>
<td align="right">1.7</td>
<td align="right">0.4</td>
<td align="left">setosa</td>
</tr>
</tbody>
</table>

**Part 1:**<a id="part1"></a> The aim is to produce a simple summary of
the iris dataset by producing a new dataset from iris that contains the
average (i.e. mean ) value of each of the variables (columns) according
to species.

Hint: I wonder how one could aggregate this?

    # work out which column is the "Species" variable
    speciesCol = grep("Species", names(iris))
    # aggregate iris by species excluding the non-numeric "Species" variable
    mean_iris = aggregate(x = iris[, -speciesCol], by = list(iris$Species), FUN = mean, na.rm= T)

    # we need to rename all of the columns
    names(mean_iris)[1] = "Species"
    names(mean_iris)[-1] = paste0(names(mean_iris)[-1], ".mean")
    # display pretty table using kable function from knitr
    knitr::kable(mean_iris)

<table>
<thead>
<tr class="header">
<th align="left">Species</th>
<th align="right">Sepal.Length.mean</th>
<th align="right">Sepal.Width.mean</th>
<th align="right">Petal.Length.mean</th>
<th align="right">Petal.Width.mean</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">setosa</td>
<td align="right">5.006</td>
<td align="right">3.428</td>
<td align="right">1.462</td>
<td align="right">0.246</td>
</tr>
<tr class="even">
<td align="left">versicolor</td>
<td align="right">5.936</td>
<td align="right">2.770</td>
<td align="right">4.260</td>
<td align="right">1.326</td>
</tr>
<tr class="odd">
<td align="left">virginica</td>
<td align="right">6.588</td>
<td align="right">2.974</td>
<td align="right">5.552</td>
<td align="right">2.026</td>
</tr>
</tbody>
</table>

**Part 2:**<a id="part2"></a> The aim is similar to the aim in [part
1](#part1) but the difference is to also include information about the
standard deviation (sd).

Hint: Is this a mind meld or a merge op?

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

<table>
<thead>
<tr class="header">
<th align="left">Species</th>
<th align="right">Petal.Length.mean</th>
<th align="right">Petal.Length.sd</th>
<th align="right">Petal.Width.mean</th>
<th align="right">Petal.Width.sd</th>
<th align="right">Sepal.Length.mean</th>
<th align="right">Sepal.Length.sd</th>
<th align="right">Sepal.Width.mean</th>
<th align="right">Sepal.Width.sd</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">setosa</td>
<td align="right">1.462</td>
<td align="right">0.1736640</td>
<td align="right">0.246</td>
<td align="right">0.1053856</td>
<td align="right">5.006</td>
<td align="right">0.3524897</td>
<td align="right">3.428</td>
<td align="right">0.3790644</td>
</tr>
<tr class="even">
<td align="left">versicolor</td>
<td align="right">4.260</td>
<td align="right">0.4699110</td>
<td align="right">1.326</td>
<td align="right">0.1977527</td>
<td align="right">5.936</td>
<td align="right">0.5161711</td>
<td align="right">2.770</td>
<td align="right">0.3137983</td>
</tr>
<tr class="odd">
<td align="left">virginica</td>
<td align="right">5.552</td>
<td align="right">0.5518947</td>
<td align="right">2.026</td>
<td align="right">0.2746501</td>
<td align="right">6.588</td>
<td align="right">0.6358796</td>
<td align="right">2.974</td>
<td align="right">0.3224966</td>
</tr>
</tbody>
</table>

**Part 3:**<a id="part3"></a> In [part 1](#part1) and [part 2](#part2),
we were able to create pretty decent summaries of Species data in
**iris** according to both the average and standard deviation.

Hmmm... something is missing? can we make it more "publication-worthy"
(sic)... yes we can!

Hint: I sense some **function**al pasting... :alien:

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

<table>
<thead>
<tr class="header">
<th align="left">Species</th>
<th align="left">Sepal.Length</th>
<th align="left">Sepal.Width</th>
<th align="left">Petal.Length</th>
<th align="left">Petal.Width</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">setosa</td>
<td align="left">5.01 ± 0.35</td>
<td align="left">3.43 ± 0.38</td>
<td align="left">1.46 ± 0.17</td>
<td align="left">0.25 ± 0.11</td>
</tr>
<tr class="even">
<td align="left">versicolor</td>
<td align="left">5.94 ± 0.52</td>
<td align="left">2.77 ± 0.31</td>
<td align="left">4.26 ± 0.47</td>
<td align="left">1.33 ± 0.2</td>
</tr>
<tr class="odd">
<td align="left">virginica</td>
<td align="left">6.59 ± 0.64</td>
<td align="left">2.97 ± 0.32</td>
<td align="left">5.55 ± 0.55</td>
<td align="left">2.03 ± 0.27</td>
</tr>
</tbody>
</table>

**Part 4:** This bite sized chunk of BURGr goodness is complete, thanks
for joining us! The open question is, can you think of other ways to
create a simple, pretty **tabular** summary of this dataset?
