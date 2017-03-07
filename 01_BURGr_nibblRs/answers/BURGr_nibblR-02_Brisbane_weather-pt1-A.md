BURGr nibblR 2: Brisbane basic weather - data import
====================================================

Hello all BURGr lovers! Welcome to the first BURGr bites exercise :smile:. The dataset that we are working with in this exercise is the beloved iris, and our goal is to create a simple, beautiful summary of the data.

Before we dive into things, let's get an idea of what we are dealing with...

[total rainfall](http://www.bom.gov.au/jsp/ncc/cdio/weatherData/av?p_display_type=dailyZippedDataFile&p_stn_num=40913&p_nccObsCode=136&p_c=-334778842&p_startYear=1999) and temperature ([maximum](http://www.bom.gov.au/jsp/ncc/cdio/weatherData/av?p_display_type=dailyZippedDataFile&p_stn_num=40913&p_nccObsCode=122&p_c=-334775952&p_startYear=1999) and [minimum](http://www.bom.gov.au/jsp/ncc/cdio/weatherData/av?p_display_type=dailyZippedDataFile&p_stn_num=40913&p_nccObsCode=123&p_c=-334776148&p_startYear=1999))

Note: The data files share the same name as the zip file, but with the file extension replaced with "\_Data.csv" and the "\_Note.txt" file endings instead.

**Part 1:**<a id="part1"></a> The first task is to extract the weather data (\*.csv) from the zip files for total rainfall (IDCJAC0009\_040913\_1800.zip), maximum temperature (IDCJAC0010\_040913\_1800.zip) and minimum temperature (IDCJAC0011\_040913\_1800.zip), then extract

Hint: if you are stuck for [options](http://stackoverflow.com/questions/28404232/#39911839), you may want to check out GET and unz.

``` r
# Brisbane (station ID: 040913)
# The strategy: Separate the URLs into common and unique compoments. 
# I had to examine the URLs and test out a few things to get a common 
# pattern that worked.

bom_base_query_url = paste0("http://www.bom.gov.au/jsp/ncc/cdio/weatherData/av?", 
    "p_display_type=dailyZippedDataFile&p_stn_num=")
bne_station_id = 040913
data_start_year = 1999
data_dir = "./data"

# Use these patterns to create a set of URLs.

bne_weather_src = list()
bne_weather_src$rain = c(url = paste0(bom_base_query_url, bne_station_id, "&p_nccObsCode=136&p_c=-334778842&p_startYear=", data_start_year), 
    zip = "IDCJAC0009_040913_1800.zip")

bne_weather_src$temp_max = c(url = paste0(bom_base_query_url, bne_station_id,
"&p_nccObsCode=122&p_c=-334775952&p_startYear=", data_start_year), 
    zip = "IDCJAC0010_040913_1800.zip")

bne_weather_src$temp_min = c(url = paste0(bom_base_query_url, bne_station_id, "&p_nccObsCode=123&p_c=-334776148&p_startYear=", data_start_year), 
    zip = "IDCJAC0011_040913_1800.zip")

# function to download data 
getWeatherData = function(dataUrl, dataFile, dataDir = ".", verbose=T){
    require(httr)
    if(!dir.exists(dataDir)) dir.create(dataDir)
    filePath = file.path(dataDir, dataFile)
    if(!file.exists(filePath)) GET(dataUrl, write_disk(filePath))
    if(verbose) cat("Downloaded file: ", dataFile, " to ", filePath, ".\n")
}

# function to read data into R
loadZipWeatherData = function(dataDir, dataFile, dataType = "csv", verbose=T){
    dataType = match.arg(dataType, choices = c("csv", "txt"))
    # construct data filename
    dataFilePrefix = gsub(".zip", "", dataFile)
    fileExtension = switch(dataType, csv = "Data.csv", txt="Note.txt")
    fileName = paste0(dataFilePrefix, "_", fileExtension)
    # corrects for difference in station ID representation
    fileName = gsub("_0", "_" , fileName, fixed=T)
    # construct file path to retrieve data file based on type 
    filePath = file.path(dataDir, dataFile)
    rawData = switch(dataType, 
        csv = read.csv(unz(filePath, fileName)), 
        txt = readLines(unz(filePath, fileName)))
    if(verbose) cat("Read", dataType, "from", fileName, "at", filePath, ".\n")
    return(rawData)
}
```

``` r
lapply(bne_weather_src, FUN = function(x, dirName){
    getWeatherData(dataUrl = x[["url"]], dataFile = x[["zip"]], dirName)
}, dirName = data_dir)
```

    ## Downloaded file:  IDCJAC0009_040913_1800.zip  to  ./data/IDCJAC0009_040913_1800.zip .
    ## Downloaded file:  IDCJAC0010_040913_1800.zip  to  ./data/IDCJAC0010_040913_1800.zip .
    ## Downloaded file:  IDCJAC0011_040913_1800.zip  to  ./data/IDCJAC0011_040913_1800.zip .

    ## $rain
    ## NULL
    ## 
    ## $temp_max
    ## NULL
    ## 
    ## $temp_min
    ## NULL

``` r
csvDataList = list()
for(i in 1:length(bne_weather_src)){
    dataNameID = names(bne_weather_src)[i]
    csvDataList[[dataNameID]] = loadZipWeatherData(
        dataDir = data_dir,
        dataFile = bne_weather_src[[i]][["zip"]],
        dataType = "csv")
}
```

    ## Read csv from IDCJAC0009_40913_1800_Data.csv at ./data/IDCJAC0009_040913_1800.zip .
    ## Read csv from IDCJAC0010_40913_1800_Data.csv at ./data/IDCJAC0010_040913_1800.zip .
    ## Read csv from IDCJAC0011_40913_1800_Data.csv at ./data/IDCJAC0011_040913_1800.zip .

``` r
rm(i, dataNameID)
```

Rainfall (total):

``` r
knitr::kable(head(csvDataList$rain, 3))
```

| Product.code |  Bureau.of.Meteorology.station.number|  Year|  Month|  Day|  Rainfall.amount..millimetres.|  Period.over.which.rainfall.was.measured..days.| Quality |
|:-------------|-------------------------------------:|-----:|------:|----:|------------------------------:|-----------------------------------------------:|:--------|
| IDCJAC0009   |                                 40913|  1999|      1|    1|                             NA|                                              NA|         |
| IDCJAC0009   |                                 40913|  1999|      1|    2|                             NA|                                              NA|         |
| IDCJAC0009   |                                 40913|  1999|      1|    3|                             NA|                                              NA|         |

Temperature (max):

``` r
knitr::kable(head(csvDataList$temp_max, 3))
```

| Product.code |  Bureau.of.Meteorology.station.number|  Year|  Month|  Day|  Maximum.temperature..Degree.C.|  Days.of.accumulation.of.maximum.temperature| Quality |
|:-------------|-------------------------------------:|-----:|------:|----:|-------------------------------:|--------------------------------------------:|:--------|
| IDCJAC0010   |                                 40913|  1999|      1|    1|                              NA|                                           NA|         |
| IDCJAC0010   |                                 40913|  1999|      1|    2|                              NA|                                           NA|         |
| IDCJAC0010   |                                 40913|  1999|      1|    3|                              NA|                                           NA|         |

Temperature (min):

``` r
knitr::kable(head(csvDataList$temp_min, 3))
```

| Product.code |  Bureau.of.Meteorology.station.number|  Year|  Month|  Day|  Minimum.temperature..Degree.C.|  Days.of.accumulation.of.minimum.temperature| Quality |
|:-------------|-------------------------------------:|-----:|------:|----:|-------------------------------:|--------------------------------------------:|:--------|
| IDCJAC0011   |                                 40913|  1999|      1|    1|                              NA|                                           NA|         |
| IDCJAC0011   |                                 40913|  1999|      1|    2|                              NA|                                           NA|         |
| IDCJAC0011   |                                 40913|  1999|      1|    3|                              NA|                                           NA|         |

**Part 2:**<a id="part2"></a> Now that we have the raw data that we need, let's see if we can combine them into a single dataset.

Hint: I wonder how one could [reduce](http://stackoverflow.com/questions/8091303/) this merge this?

``` r
# keep only columns 3 to 6. The relevant data is here
csvDataList = lapply(csvDataList, FUN = function(x) (return(x[, c(3:6)]) ))
bne_raw_weather = Reduce( function(...) merge(..., by = c("Year", "Month", "Day")), csvDataList)

# cleanup column names
names(bne_raw_weather) = gsub("^(.)?Rainfall(.)+", "Rainfall.mm", names(bne_raw_weather))
names(bne_raw_weather) = gsub("^(.)?Maximum\\.temp(.)+", "Temp.max.degC", names(bne_raw_weather))
names(bne_raw_weather) = gsub("^(.)?Minimum\\.temp(.)+", "Temp.min.degC", names(bne_raw_weather))

knitr::kable(head(bne_raw_weather, 5))
```

|  Year|  Month|  Day|  Rainfall.mm|  Temp.max.degC|  Temp.min.degC|
|-----:|------:|----:|------------:|--------------:|--------------:|
|  1999|     10|    1|           NA|             NA|             NA|
|  1999|     10|   10|           NA|             NA|             NA|
|  1999|     10|   11|           NA|             NA|             NA|
|  1999|     10|   12|           NA|             NA|             NA|
|  1999|     10|   13|           NA|             NA|             NA|
