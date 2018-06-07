Name: Chris Kiewit ANR 198094 SNR 2010702

Introduction
============

The exam consists of 2 parts. In the first part, you have to run a
regression, test if the assumptions of a linear regression model are
met, and make 2 graphs.

In the second part of the exam, you will have to make a map of Catholic
and Protestant schools in the Netherlands.

Packages
========

    library(tidyverse)

    ## Warning: package 'tidyverse' was built under R version 3.3.3

    ## -- Attaching packages ------------------------------------------------------------------------------------------------------------------------------- tidyverse 1.2.1 --

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

    ## -- Conflicts ---------------------------------------------------------------------------------------------------------------------------------- tidyverse_conflicts() --
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

    ggplot(df1, aes(x=group, y=V1)) + 
      geom_boxplot()

    ## Warning: Continuous x aesthetic -- did you forget aes(group=...)?

![](Exam_studentChris_files/figure-markdown_strict/unnamed-chunk-3-1.png)
\#\#\#\# Explanation I simply filled in the required information to make
the requested boc plot.

Assignment 1b
-------------

Run a regression with response variable as a function of V1. Show the
summary statistics of the regression.

    reg1 <- lm(data=df1, response~V1)
    summary(reg1)

    ## 
    ## Call:
    ## lm(formula = response ~ V1, data = df1)
    ## 
    ## Residuals:
    ##     Min      1Q  Median      3Q     Max 
    ## -4.5116 -1.1157 -0.1313  1.0985  4.3723 
    ## 
    ## Coefficients:
    ##             Estimate Std. Error t value Pr(>|t|)    
    ## (Intercept)   2.6305     0.6347   4.145 0.000138 ***
    ## V1           -1.9152     0.1014 -18.880  < 2e-16 ***
    ## ---
    ## Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
    ## 
    ## Residual standard error: 1.881 on 48 degrees of freedom
    ## Multiple R-squared:  0.8813, Adjusted R-squared:  0.8788 
    ## F-statistic: 356.4 on 1 and 48 DF,  p-value: < 2.2e-16

#### Explanation

I made a regression as requested

check if the assumptions of linear regression are met with the `gvlma()`
function.

    gvlma(reg1)

    ## 
    ## Call:
    ## lm(formula = response ~ V1, data = df1)
    ## 
    ## Coefficients:
    ## (Intercept)           V1  
    ##       2.630       -1.915  
    ## 
    ## 
    ## ASSESSMENT OF THE LINEAR MODEL ASSUMPTIONS
    ## USING THE GLOBAL TEST ON 4 DEGREES-OF-FREEDOM:
    ## Level of Significance =  0.05 
    ## 
    ## Call:
    ##  gvlma(x = reg1) 
    ## 
    ##                       Value p-value                Decision
    ## Global Stat        0.654319  0.9568 Assumptions acceptable.
    ## Skewness           0.002398  0.9609 Assumptions acceptable.
    ## Kurtosis           0.007200  0.9324 Assumptions acceptable.
    ## Link Function      0.005852  0.9390 Assumptions acceptable.
    ## Heteroscedasticity 0.638869  0.4241 Assumptions acceptable.

#### Explanation

I filled in reg1 in gvlma() to see if the assumptions are met. They are
met.

Assignment 1c
-------------

Make a scatterplot with:

-   V1 on the x-axis and the response on the y-axis
-   Include the regression line in red with confidence interval
-   In a classic theme
-   The x-axis should be labeled "Predictor", the y-axis should be
    labeled ("Response")

<!-- -->

    'Predictor'<-df1['V1']
    'Response'<-df1['response']
    ggplot(df1,aes(x=Predictor,y=Response))+
      geom_point()+ theme_classic()+
      geom_smooth(method='lm',formula=y~x, col=10)

    ## Don't know how to automatically pick scale for object of type data.frame. Defaulting to continuous.
    ## Don't know how to automatically pick scale for object of type data.frame. Defaulting to continuous.

![](Exam_studentChris_files/figure-markdown_strict/unnamed-chunk-6-1.png)
\#\#\#\# Explanation I started with renaming V1 and response to
Predictor and Response respectively. After which I made the desired
graph using ggplot.

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

    map_municipal <- 
      read.csv2("../Sourcedata/nld_municipal_map.csv", stringsAsFactors = FALSE, dec = ".")
    head(map_municipal)

    ##         name id        x        y order  hole piece group
    ## 1 Appingedam  0 251260.5 594393.8     1 FALSE     1   0.1
    ## 2 Appingedam  0 251427.8 594486.7     2 FALSE     1   0.1
    ## 3 Appingedam  0 251668.8 594646.8     3 FALSE     1   0.1
    ## 4 Appingedam  0 251713.5 594770.9     4 FALSE     1   0.1
    ## 5 Appingedam  0 251354.6 595461.4     5 FALSE     1   0.1
    ## 6 Appingedam  0 251310.3 596022.5     6 FALSE     1   0.1

#### Explanation

Copied from the notebook from week 6

Assignment 2b
-------------

Now you can make an empty map of the Netherlands.

    AddMapLayer(MapPlot(), map_municipal)

    ## Warning: `axis.ticks.margin` is deprecated. Please set `margin` property of
    ## `axis.text` instead

![](Exam_studentChris_files/figure-markdown_strict/unnamed-chunk-9-1.png)
\#\#\#\# Explanation Copied from the notebook from week 6

Assignment 2c
-------------

Read in the pc4 locations (nld\_pc4\_locations.csv).

Hint: Don't forget the X and Y should be numeric variables!

    pc4_locations <- 
      read.csv2("../Sourcedata/nld_pc4_locations.csv", stringsAsFactors = FALSE, dec = ".") %>%
      mutate(X=as.numeric(as.character(X)))%>%
      mutate(Y=as.numeric(as.character(Y)))

    str(pc4_locations)

    ## 'data.frame':    4066 obs. of  3 variables:
    ##  $ PC4: int  1011 1012 1013 1014 1015 1016 1017 1018 1019 1021 ...
    ##  $ X  : num  122244 121613 120325 119515 120740 ...
    ##  $ Y  : num  487223 487555 489672 489422 488009 ...

#### Explanation

read the file and turned X and Y into numerics

Assignment 2d
-------------

### 2di

Read in the school data

    schools <- read.csv2("../Sourcedata/schools.csv", stringsAsFactors = FALSE, dec = ".")
    head(schools)

    ##              PROVINCIE BEVOEGD.GEZAG.NUMMER BRIN.NUMMER VESTIGINGSNUMMER
    ## 1                                     41152        23HC           23HC04
    ## 2              Drenthe                10053        18BR           18BR00
    ## 3              Drenthe                10053        18BR           18BR01
    ## 4              Drenthe                13273        20LO           20LO00
    ## 5              Drenthe                13273        20LO           20LO01
    ## 6              Drenthe                13273        20LO           20LO02
    ##                                                              VESTIGINGSNAAM
    ## 1                                                          RSG Lingecollege
    ## 2                                       School voor Praktijkonderwijs Assen
    ## 3                                       School voor Praktijkonderwijs Assen
    ## 4 Openbare Scholengemeenschap Dr Nassau College voor Vwo Havo Mavo Vbo Lwoo
    ## 5 Openbare Scholengemeenschap Dr Nassau College voor Vwo Havo Mavo Vbo Lwoo
    ## 6 Openbare Scholengemeenschap Dr Nassau College voor Vwo Havo Mavo Vbo Lwoo
    ##                 STRAATNAAM HUISNUMMER.TOEVOEGING POSTCODE
    ## 1                                                        
    ## 2           Zwartwatersweg                   202  9406 NN
    ## 3                  Bosrand                     2  9401 SL
    ## 4  Mr Groen v Prinstererln                    98  9402 KG
    ## 5             Schoolstraat                     1  9331 AV
    ## 6              Esdoornlaan                     2  9411 AV
    ##                 PLAATSNAAM GEMEENTENUMMER             GEMEENTENAAM
    ## 1                                      NA                         
    ## 2                    ASSEN            106                    ASSEN
    ## 3                    ASSEN            106                    ASSEN
    ## 4                    ASSEN            106                    ASSEN
    ## 5                     NORG           1699              NOORDENVELD
    ## 6                   BEILEN           1731           MIDDEN-DRENTHE
    ##   DENOMINATIE TELEFOONNUMMER
    ## 1    Openbaar             NA
    ## 2    Openbaar      592340973
    ## 3    Openbaar             NA
    ## 4    Openbaar      592333111
    ## 5    Openbaar      592675750
    ## 6    Openbaar      593538160
    ##                                                                      INTERNETADRES
    ## 1                                                                                 
    ## 2                                                                 www.pro-assen.nl
    ## 3                                                                                 
    ## 4                                                          www.dr.nassaucollege.nl
    ## 5                                                                                 
    ## 6                                                                                 
    ##   ONDERWIJSSTRUCTUUR STRAATNAAM.CORRESPONDENTIEADRES
    ## 1           VBO/MAVO                         Postbus
    ## 2                PRO                  Zwartwatersweg
    ## 3                PRO                  Zwartwatersweg
    ## 4       VBO/HAVO/VWO                         Postbus
    ## 5           VBO/MAVO                         Postbus
    ## 6           VBO/MAVO                         Postbus
    ##   HUISNUMMER.TOEVOEGING.CORRESPONDENTIEADRES POSTCODE.CORRESPONDENTIEADRES
    ## 1                                         56                       4000 AB
    ## 2                                        202                       9406 NN
    ## 3                                        202                       9406 NN
    ## 4                                        186                       9400 AD
    ## 5                                        186                       9400 AD
    ## 6                                        186                       9400 AD
    ##   PLAATSNAAM.CORRESPONDENTIEADRES NODAAL.GEBIED.CODE
    ## 1                            TIEL                 NA
    ## 2                           ASSEN                 11
    ## 3                           ASSEN                 11
    ## 4                           ASSEN                 11
    ## 5                           ASSEN                  1
    ## 6                           ASSEN                 11
    ##         NODAAL.GEBIED.NAAM RPA.GEBIED.CODE          RPA.GEBIED.NAAM
    ## 1                                       NA                         
    ## 2                    Assen               3       Centraal-Groningen
    ## 3                    Assen               3       Centraal-Groningen
    ## 4                    Assen               3       Centraal-Groningen
    ## 5                Groningen               3       Centraal-Groningen
    ## 6                    Assen               5  Zuid- en Midden-Drenthe
    ##   WGR.GEBIED.CODE                         WGR.GEBIED.NAAM COROPGEBIED.CODE
    ## 1              NA                                                       NA
    ## 2               7                Noord- en Midden-Drenthe                7
    ## 3               7                Noord- en Midden-Drenthe                7
    ## 4               7                Noord- en Midden-Drenthe                7
    ## 5               7                Noord- en Midden-Drenthe                7
    ## 6               7                Noord- en Midden-Drenthe                7
    ##                      COROPGEBIED.NAAM ONDERWIJSGEBIED.CODE
    ## 1                                                       NA
    ## 2                       Noord-Drenthe                    4
    ## 3                       Noord-Drenthe                    4
    ## 4                       Noord-Drenthe                    4
    ## 5                       Noord-Drenthe                    1
    ## 6                       Noord-Drenthe                    4
    ##               ONDERWIJSGEBIED.NAAM RMC.REGIO.CODE
    ## 1                                              NA
    ## 2            Assen-Hoogeveen-Emmen              7
    ## 3            Assen-Hoogeveen-Emmen              7
    ## 4            Assen-Hoogeveen-Emmen              7
    ## 5           Groningen en omstreken              7
    ## 6            Assen-Hoogeveen-Emmen              7
    ##                             RMC.REGIO.NAAM
    ## 1                                         
    ## 2                 Noord- en Midden Drenthe
    ## 3                 Noord- en Midden Drenthe
    ## 4                 Noord- en Midden Drenthe
    ## 5                 Noord- en Midden Drenthe
    ## 6                 Noord- en Midden Drenthe

#### Explanation

read the file and used head to get a better view of what was in it

### 2dii

First, create a new dataframe schools1, which is equal to schools.

As you see POSTCODE has a structure of (1234 AB). You should create a
new variable PC4 that is equal to the first 4 numbers in POSTCODE

Hint: Use the function `substr()`

Then select the variables PC4 and DENOMINATIE

    schools1 <- schools 
      schools1$PC4 <- substr('POSTCODE',1,4)
    head(schools1)

    ##              PROVINCIE BEVOEGD.GEZAG.NUMMER BRIN.NUMMER VESTIGINGSNUMMER
    ## 1                                     41152        23HC           23HC04
    ## 2              Drenthe                10053        18BR           18BR00
    ## 3              Drenthe                10053        18BR           18BR01
    ## 4              Drenthe                13273        20LO           20LO00
    ## 5              Drenthe                13273        20LO           20LO01
    ## 6              Drenthe                13273        20LO           20LO02
    ##                                                              VESTIGINGSNAAM
    ## 1                                                          RSG Lingecollege
    ## 2                                       School voor Praktijkonderwijs Assen
    ## 3                                       School voor Praktijkonderwijs Assen
    ## 4 Openbare Scholengemeenschap Dr Nassau College voor Vwo Havo Mavo Vbo Lwoo
    ## 5 Openbare Scholengemeenschap Dr Nassau College voor Vwo Havo Mavo Vbo Lwoo
    ## 6 Openbare Scholengemeenschap Dr Nassau College voor Vwo Havo Mavo Vbo Lwoo
    ##                 STRAATNAAM HUISNUMMER.TOEVOEGING POSTCODE
    ## 1                                                        
    ## 2           Zwartwatersweg                   202  9406 NN
    ## 3                  Bosrand                     2  9401 SL
    ## 4  Mr Groen v Prinstererln                    98  9402 KG
    ## 5             Schoolstraat                     1  9331 AV
    ## 6              Esdoornlaan                     2  9411 AV
    ##                 PLAATSNAAM GEMEENTENUMMER             GEMEENTENAAM
    ## 1                                      NA                         
    ## 2                    ASSEN            106                    ASSEN
    ## 3                    ASSEN            106                    ASSEN
    ## 4                    ASSEN            106                    ASSEN
    ## 5                     NORG           1699              NOORDENVELD
    ## 6                   BEILEN           1731           MIDDEN-DRENTHE
    ##   DENOMINATIE TELEFOONNUMMER
    ## 1    Openbaar             NA
    ## 2    Openbaar      592340973
    ## 3    Openbaar             NA
    ## 4    Openbaar      592333111
    ## 5    Openbaar      592675750
    ## 6    Openbaar      593538160
    ##                                                                      INTERNETADRES
    ## 1                                                                                 
    ## 2                                                                 www.pro-assen.nl
    ## 3                                                                                 
    ## 4                                                          www.dr.nassaucollege.nl
    ## 5                                                                                 
    ## 6                                                                                 
    ##   ONDERWIJSSTRUCTUUR STRAATNAAM.CORRESPONDENTIEADRES
    ## 1           VBO/MAVO                         Postbus
    ## 2                PRO                  Zwartwatersweg
    ## 3                PRO                  Zwartwatersweg
    ## 4       VBO/HAVO/VWO                         Postbus
    ## 5           VBO/MAVO                         Postbus
    ## 6           VBO/MAVO                         Postbus
    ##   HUISNUMMER.TOEVOEGING.CORRESPONDENTIEADRES POSTCODE.CORRESPONDENTIEADRES
    ## 1                                         56                       4000 AB
    ## 2                                        202                       9406 NN
    ## 3                                        202                       9406 NN
    ## 4                                        186                       9400 AD
    ## 5                                        186                       9400 AD
    ## 6                                        186                       9400 AD
    ##   PLAATSNAAM.CORRESPONDENTIEADRES NODAAL.GEBIED.CODE
    ## 1                            TIEL                 NA
    ## 2                           ASSEN                 11
    ## 3                           ASSEN                 11
    ## 4                           ASSEN                 11
    ## 5                           ASSEN                  1
    ## 6                           ASSEN                 11
    ##         NODAAL.GEBIED.NAAM RPA.GEBIED.CODE          RPA.GEBIED.NAAM
    ## 1                                       NA                         
    ## 2                    Assen               3       Centraal-Groningen
    ## 3                    Assen               3       Centraal-Groningen
    ## 4                    Assen               3       Centraal-Groningen
    ## 5                Groningen               3       Centraal-Groningen
    ## 6                    Assen               5  Zuid- en Midden-Drenthe
    ##   WGR.GEBIED.CODE                         WGR.GEBIED.NAAM COROPGEBIED.CODE
    ## 1              NA                                                       NA
    ## 2               7                Noord- en Midden-Drenthe                7
    ## 3               7                Noord- en Midden-Drenthe                7
    ## 4               7                Noord- en Midden-Drenthe                7
    ## 5               7                Noord- en Midden-Drenthe                7
    ## 6               7                Noord- en Midden-Drenthe                7
    ##                      COROPGEBIED.NAAM ONDERWIJSGEBIED.CODE
    ## 1                                                       NA
    ## 2                       Noord-Drenthe                    4
    ## 3                       Noord-Drenthe                    4
    ## 4                       Noord-Drenthe                    4
    ## 5                       Noord-Drenthe                    1
    ## 6                       Noord-Drenthe                    4
    ##               ONDERWIJSGEBIED.NAAM RMC.REGIO.CODE
    ## 1                                              NA
    ## 2            Assen-Hoogeveen-Emmen              7
    ## 3            Assen-Hoogeveen-Emmen              7
    ## 4            Assen-Hoogeveen-Emmen              7
    ## 5           Groningen en omstreken              7
    ## 6            Assen-Hoogeveen-Emmen              7
    ##                             RMC.REGIO.NAAM  PC4
    ## 1                                          POST
    ## 2                 Noord- en Midden Drenthe POST
    ## 3                 Noord- en Midden Drenthe POST
    ## 4                 Noord- en Midden Drenthe POST
    ## 5                 Noord- en Midden Drenthe POST
    ## 6                 Noord- en Midden Drenthe POST

#### Explanation

just tried things and ended up with this and got something that looked
like the desired result...

### 2diii

Create the dataframe school\_loc as a join from pc4\_locations and
school1 that combines the columns from both data frames, but only keeps
rows where the value in the pc4\_locations column matches in both data
frames. And then select the observations with DENOMINATIE is equal to
"Rooms-Katholiek" or "Protestants-Christelijk"

#### Explanation

tried something without succes and ran out of time

Assignment 2e
-------------

Create a map of Catholic and Protestant schools in the Netherlands

#### Explanation

tried something without succes and ran out of time
