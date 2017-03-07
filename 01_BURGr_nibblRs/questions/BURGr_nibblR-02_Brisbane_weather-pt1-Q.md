BURGr nibblR 2: Brisbane basic weather - data import
====================================================

Hello all BURGr lovers! Welcome to the first BURGr bites exercise :smile:. The dataset that we are working with in this exercise is the beloved iris, and our goal is to create a simple, beautiful summary of the data.

Before we dive into things, let's get an idea of what we are dealing with...

[total rainfall](http://www.bom.gov.au/jsp/ncc/cdio/weatherData/av?p_display_type=dailyZippedDataFile&p_stn_num=40913&p_nccObsCode=136&p_c=-334778842&p_startYear=1999) and temperature ([maximum](http://www.bom.gov.au/jsp/ncc/cdio/weatherData/av?p_display_type=dailyZippedDataFile&p_stn_num=40913&p_nccObsCode=122&p_c=-334775952&p_startYear=1999) and [minimum](http://www.bom.gov.au/jsp/ncc/cdio/weatherData/av?p_display_type=dailyZippedDataFile&p_stn_num=40913&p_nccObsCode=123&p_c=-334776148&p_startYear=1999))

Note: The data files share the same name as the zip file, but with the file extension replaced with "\_Data.csv" and the "\_Note.txt" file endings instead.

**Part 1:**<a id="part1"></a> The first task is to extract the weather data (\*.csv) from the zip files for total rainfall (IDCJAC0009\_040913\_1800.zip), maximum temperature (IDCJAC0010\_040913\_1800.zip) and minimum temperature (IDCJAC0011\_040913\_1800.zip), then extract

Hint: if you are stuck for [options](http://stackoverflow.com/questions/28404232/#39911839), you may want to check out GET and unz.

Rainfall (total):

| Product.code |  Bureau.of.Meteorology.station.number|  Year|  Month|  Day|  Rainfall.amount..millimetres.|  Period.over.which.rainfall.was.measured..days.| Quality |
|:-------------|-------------------------------------:|-----:|------:|----:|------------------------------:|-----------------------------------------------:|:--------|
| IDCJAC0009   |                                 40913|  1999|      1|    1|                             NA|                                              NA|         |
| IDCJAC0009   |                                 40913|  1999|      1|    2|                             NA|                                              NA|         |
| IDCJAC0009   |                                 40913|  1999|      1|    3|                             NA|                                              NA|         |

Temperature (max):

| Product.code |  Bureau.of.Meteorology.station.number|  Year|  Month|  Day|  Maximum.temperature..Degree.C.|  Days.of.accumulation.of.maximum.temperature| Quality |
|:-------------|-------------------------------------:|-----:|------:|----:|-------------------------------:|--------------------------------------------:|:--------|
| IDCJAC0010   |                                 40913|  1999|      1|    1|                              NA|                                           NA|         |
| IDCJAC0010   |                                 40913|  1999|      1|    2|                              NA|                                           NA|         |
| IDCJAC0010   |                                 40913|  1999|      1|    3|                              NA|                                           NA|         |

Temperature (min):

| Product.code |  Bureau.of.Meteorology.station.number|  Year|  Month|  Day|  Minimum.temperature..Degree.C.|  Days.of.accumulation.of.minimum.temperature| Quality |
|:-------------|-------------------------------------:|-----:|------:|----:|-------------------------------:|--------------------------------------------:|:--------|
| IDCJAC0011   |                                 40913|  1999|      1|    1|                              NA|                                           NA|         |
| IDCJAC0011   |                                 40913|  1999|      1|    2|                              NA|                                           NA|         |
| IDCJAC0011   |                                 40913|  1999|      1|    3|                              NA|                                           NA|         |

**Part 2:**<a id="part2"></a> Now that we have the raw data that we need, let's see if we can combine them into a single dataset.

Hint: I wonder how one could [reduce](http://stackoverflow.com/questions/8091303/) this merge this?

|  Year|  Month|  Day|  Rainfall.mm|  Temp.max.degC|  Temp.min.degC|
|-----:|------:|----:|------------:|--------------:|--------------:|
|  1999|     10|    1|           NA|             NA|             NA|
|  1999|     10|   10|           NA|             NA|             NA|
|  1999|     10|   11|           NA|             NA|             NA|
|  1999|     10|   12|           NA|             NA|             NA|
|  1999|     10|   13|           NA|             NA|             NA|
