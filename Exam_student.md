<table>
<thead>
<tr class="header">
<th>Name</th>
<th>Studentnumber</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td>Name Student</td>
<td></td>
</tr>
<tr class="even">
<td>Name Collaborators</td>
<td></td>
</tr>
</tbody>
</table>

Introduction
============

The exam consists of 2 parts. In the first part, you have to run a
regression, test if the assumptions of a linear regression model are
met, and make 2 graphs.

In the second part of the exam, you will have to make a map of Catholic
and Protestant schools in the Netherlands.

Instruction
-----------

When you work in R studio, you will work in this Notebook. When you use
the "knit" button, a md (markdown document) will be save. You have to
upload the md document to Github as your final exam.

Packages
========

    library(tidyverse)

    ## Warning: package 'tidyverse' was built under R version 3.3.3

    ## -- Attaching packages ------------------------------------------------------------------------------------------------------------------------------------------------------- tidyverse 1.2.1 --

    ## v ggplot2 2.2.1     v purrr   0.2.4
    ## v tibble  1.3.4     v dplyr   0.7.4
    ## v tidyr   0.7.2     v stringr 1.2.0
    ## v readr   1.1.1     v forcats 0.2.0

    ## Warning: package 'ggplot2' was built under R version 3.3.3

    ## Warning: package 'tibble' was built under R version 3.3.3

    ## Warning: package 'tidyr' was built under R version 3.3.3

    ## Warning: package 'readr' was built under R version 3.3.3

    ## Warning: package 'purrr' was built under R version 3.3.3

    ## Warning: package 'dplyr' was built under R version 3.3.3

    ## Warning: package 'stringr' was built under R version 3.3.3

    ## Warning: package 'forcats' was built under R version 3.3.3

    ## -- Conflicts ---------------------------------------------------------------------------------------------------------------------------------------------------------- tidyverse_conflicts() --
    ## x dplyr::filter() masks stats::filter()
    ## x dplyr::lag()    masks stats::lag()

    library(gvlma)

Assignment 1
============

Data
----

The data are given:

    set.seed(123)
    df1 <- as.data.frame(matrix(runif(1*50, min = 1, max = 10), ncol = 1)) %>%
        mutate(epsilon = rnorm(50, mean = 0, sd = 2)) %>%
        mutate(response = 3 - 2* V1  + epsilon) %>%
        mutate(group = ifelse(V1 <= 5, 1,2))

    ## Warning: package 'bindrcpp' was built under R version 3.3.3

Asignment 1a
------------

The first assigment is to make boxplot using ggplot with group on the
x-axis and V1 on the y-axis.

Assignment 1b
-------------

Run a regression with response variable as a function of V1. Show the
summary statistics of the regression.

You can use these commands: `reg1 <-` `summary(reg1)`

check if the assumptions of linear regression are met with the `gvlma()`
function.

Assignment 1c
-------------

Make a scatterplot with:

-   V1 on the x-axis and the response on the y-axis
-   Include the regression line in red with confidence interval
-   In a classic theme
-   The x-axis should be labeled "Predictor", the y-axis should be
    labeled ("Response")

Assigment 2
===========

Packages
========

    library(thematicmaps)

    ## Loading required package: maptools

    ## Warning: package 'maptools' was built under R version 3.3.3

    ## Loading required package: sp

    ## Warning: package 'sp' was built under R version 3.3.3

    ## Checking rgeos availability: TRUE

    ## Loading required package: digest

    ## Loading required package: rgdal

    ## Warning: package 'rgdal' was built under R version 3.3.3

    ## rgdal: version: 1.2-15, (SVN revision 691)
    ##  Geospatial Data Abstraction Library extensions to R successfully loaded
    ##  Loaded GDAL runtime: GDAL 2.2.0, released 2017/04/28
    ##  Path to GDAL shared files: C:/Program Files/R/R-3.3.2/packages/rgdal/gdal
    ##  GDAL binary built with GEOS: TRUE 
    ##  Loaded PROJ.4 runtime: Rel. 4.9.3, 15 August 2016, [PJ_VERSION: 493]
    ##  Path to PROJ.4 shared files: C:/Program Files/R/R-3.3.2/packages/rgdal/proj
    ##  Linking to sp version: 1.2-5

    ## Loading required package: rgeos

    ## Warning: package 'rgeos' was built under R version 3.3.3

    ## rgeos version: 0.3-26, (SVN revision 560)
    ##  GEOS runtime version: 3.6.1-CAPI-1.10.1 r0 
    ##  Linking to sp version: 1.2-5 
    ##  Polygon checking: TRUE

    ## Loading required package: grid

    library(tidyverse)

Assignment 2a
-------------

First you have to read in the file "nld\_municipal\_map.csv". Hint: Look
at the notebook of week 6 about maps.

You can use

`map_municipal <-` `head(map_municipal)`

Assignment 2b
-------------

Now you can make an empty map of the Netherlands.

Assignment 2c
-------------

Read in the pc4 locations (nld\_pc4\_locations.csv).

Hint: Don't forget the X and Y should be numeric variables!

you can use: `pc4_locations <-`

`str(pc4_locations)`

Assignment 2d
-------------

### 2di

Read in the school data

you can use

`schools <-`

### 2dii

First, create a new dataframe schools1, which is equal to schools.

As you see POSTCODE has a structure of (1234 AB). You should create a
new variable PC4 that is equal to the first 4 numbers in POSTCODE

Hint: Use the function `substr()`

Then select the variables PC4 and DENOMINATIE

you can use:

`schools1 <- schools %>%`

### 2diii

Create the dataframe school\_loc as a join from pc4\_locations and
school1 that combines the columns from both data frames, but only keeps
rows where the value in the pc4\_locations column matches in both data
frames. And then select the observations with DENOMINATIE is equal to
"Rooms-Katholiek" or "Protestants-Christelijk"

You can use: `school_loc <-`

Assignment 2e
-------------

Create a map of Catholic and Protestant schools in the Netherlands
