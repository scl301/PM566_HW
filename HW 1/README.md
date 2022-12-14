Assignment 1: Exploratory Data Analysis
================
Stephanie Lee
2022-11-30

# Main Question

Have daily concentrations of PM (particulate matter air pollution with
aerodynamic diameter less than 2.5 m) decreased in California over the
last 15 years (from 2004 to 2019)?

# Step 1

Given the formulated question from the assignment description, you will
now conduct EDA Checklist items 2-4. First, download 2004 and 2019 data
for all sites in California from the EPA Air Quality Data website. Read
in the data using data.table(). For each of the two datasets, check the
dimensions, headers, footers, variable names and variable types. Check
for any data issues, particularly in the key variable we are analyzing.
Make sure you write up a summary of all of your findings.

``` r
data04 <- data.table::fread("ad_viz_plotval_data_2004.csv")
data19 <- data.table::fread("ad_viz_plotval_data_2019.csv")
```

``` r
summary(data04)
```

    ##      Date              Source             Site ID              POC        
    ##  Length:19233       Length:19233       Min.   :60010007   Min.   : 1.000  
    ##  Class :character   Class :character   1st Qu.:60370002   1st Qu.: 1.000  
    ##  Mode  :character   Mode  :character   Median :60658001   Median : 1.000  
    ##                                        Mean   :60588026   Mean   : 1.816  
    ##                                        3rd Qu.:60750006   3rd Qu.: 2.000  
    ##                                        Max.   :61131003   Max.   :12.000  
    ##                                                                           
    ##  Daily Mean PM2.5 Concentration    UNITS           DAILY_AQI_VALUE 
    ##  Min.   : -0.10                 Length:19233       Min.   :  0.00  
    ##  1st Qu.:  6.00                 Class :character   1st Qu.: 25.00  
    ##  Median : 10.10                 Mode  :character   Median : 42.00  
    ##  Mean   : 13.13                                    Mean   : 46.32  
    ##  3rd Qu.: 16.30                                    3rd Qu.: 60.00  
    ##  Max.   :251.00                                    Max.   :301.00  
    ##                                                                    
    ##   Site Name         DAILY_OBS_COUNT PERCENT_COMPLETE AQS_PARAMETER_CODE
    ##  Length:19233       Min.   :1       Min.   :100      Min.   :88101     
    ##  Class :character   1st Qu.:1       1st Qu.:100      1st Qu.:88101     
    ##  Mode  :character   Median :1       Median :100      Median :88101     
    ##                     Mean   :1       Mean   :100      Mean   :88267     
    ##                     3rd Qu.:1       3rd Qu.:100      3rd Qu.:88502     
    ##                     Max.   :1       Max.   :100      Max.   :88502     
    ##                                                                        
    ##  AQS_PARAMETER_DESC   CBSA_CODE      CBSA_NAME           STATE_CODE
    ##  Length:19233       Min.   :12540   Length:19233       Min.   :6   
    ##  Class :character   1st Qu.:31080   Class :character   1st Qu.:6   
    ##  Mode  :character   Median :40140   Mode  :character   Median :6   
    ##                     Mean   :35328                      Mean   :6   
    ##                     3rd Qu.:41860                      3rd Qu.:6   
    ##                     Max.   :49700                      Max.   :6   
    ##                     NA's   :1253                                   
    ##     STATE            COUNTY_CODE        COUNTY          SITE_LATITUDE  
    ##  Length:19233       Min.   :  1.00   Length:19233       Min.   :32.63  
    ##  Class :character   1st Qu.: 37.00   Class :character   1st Qu.:34.07  
    ##  Mode  :character   Median : 65.00   Mode  :character   Median :36.48  
    ##                     Mean   : 58.63                      Mean   :36.23  
    ##                     3rd Qu.: 75.00                      3rd Qu.:38.10  
    ##                     Max.   :113.00                      Max.   :41.71  
    ##                                                                        
    ##  SITE_LONGITUDE  
    ##  Min.   :-124.2  
    ##  1st Qu.:-121.6  
    ##  Median :-119.3  
    ##  Mean   :-119.7  
    ##  3rd Qu.:-117.9  
    ##  Max.   :-115.5  
    ## 

``` r
summary(data19)
```

    ##      Date              Source             Site ID              POC        
    ##  Length:53156       Length:53156       Min.   :60010007   Min.   : 1.000  
    ##  Class :character   Class :character   1st Qu.:60310004   1st Qu.: 1.000  
    ##  Mode  :character   Mode  :character   Median :60612003   Median : 3.000  
    ##                                        Mean   :60565264   Mean   : 2.573  
    ##                                        3rd Qu.:60771002   3rd Qu.: 3.000  
    ##                                        Max.   :61131003   Max.   :21.000  
    ##                                                                           
    ##  Daily Mean PM2.5 Concentration    UNITS           DAILY_AQI_VALUE 
    ##  Min.   : -2.200                Length:53156       Min.   :  0.00  
    ##  1st Qu.:  4.000                Class :character   1st Qu.: 17.00  
    ##  Median :  6.500                Mode  :character   Median : 27.00  
    ##  Mean   :  7.739                                   Mean   : 30.57  
    ##  3rd Qu.:  9.900                                   3rd Qu.: 41.00  
    ##  Max.   :120.900                                   Max.   :185.00  
    ##                                                                    
    ##   Site Name         DAILY_OBS_COUNT PERCENT_COMPLETE AQS_PARAMETER_CODE
    ##  Length:53156       Min.   :1       Min.   :100      Min.   :88101     
    ##  Class :character   1st Qu.:1       1st Qu.:100      1st Qu.:88101     
    ##  Mode  :character   Median :1       Median :100      Median :88101     
    ##                     Mean   :1       Mean   :100      Mean   :88214     
    ##                     3rd Qu.:1       3rd Qu.:100      3rd Qu.:88502     
    ##                     Max.   :1       Max.   :100      Max.   :88502     
    ##                                                                        
    ##  AQS_PARAMETER_DESC   CBSA_CODE      CBSA_NAME           STATE_CODE
    ##  Length:53156       Min.   :12540   Length:53156       Min.   :6   
    ##  Class :character   1st Qu.:31080   Class :character   1st Qu.:6   
    ##  Mode  :character   Median :40140   Mode  :character   Median :6   
    ##                     Mean   :35839                      Mean   :6   
    ##                     3rd Qu.:41860                      3rd Qu.:6   
    ##                     Max.   :49700                      Max.   :6   
    ##                     NA's   :4181                                   
    ##     STATE            COUNTY_CODE        COUNTY          SITE_LATITUDE  
    ##  Length:53156       Min.   :  1.00   Length:53156       Min.   :32.58  
    ##  Class :character   1st Qu.: 31.00   Class :character   1st Qu.:34.14  
    ##  Mode  :character   Median : 61.00   Mode  :character   Median :36.63  
    ##                     Mean   : 56.38                      Mean   :36.34  
    ##                     3rd Qu.: 77.00                      3rd Qu.:37.97  
    ##                     Max.   :113.00                      Max.   :41.76  
    ##                                                                        
    ##  SITE_LONGITUDE  
    ##  Min.   :-124.2  
    ##  1st Qu.:-121.6  
    ##  Median :-119.8  
    ##  Mean   :-119.8  
    ##  3rd Qu.:-118.1  
    ##  Max.   :-115.5  
    ## 

``` r
head(data04)
```

    ##          Date Source  Site ID POC Daily Mean PM2.5 Concentration    UNITS
    ## 1: 01/01/2004    AQS 60010007   1                            8.9 ug/m3 LC
    ## 2: 01/02/2004    AQS 60010007   1                           12.2 ug/m3 LC
    ## 3: 01/03/2004    AQS 60010007   1                           16.5 ug/m3 LC
    ## 4: 01/04/2004    AQS 60010007   1                           19.5 ug/m3 LC
    ## 5: 01/05/2004    AQS 60010007   1                           11.5 ug/m3 LC
    ## 6: 01/06/2004    AQS 60010007   1                           32.5 ug/m3 LC
    ##    DAILY_AQI_VALUE Site Name DAILY_OBS_COUNT PERCENT_COMPLETE
    ## 1:              37 Livermore               1              100
    ## 2:              51 Livermore               1              100
    ## 3:              60 Livermore               1              100
    ## 4:              67 Livermore               1              100
    ## 5:              48 Livermore               1              100
    ## 6:              94 Livermore               1              100
    ##    AQS_PARAMETER_CODE                     AQS_PARAMETER_DESC CBSA_CODE
    ## 1:              88101               PM2.5 - Local Conditions     41860
    ## 2:              88502 Acceptable PM2.5 AQI & Speciation Mass     41860
    ## 3:              88502 Acceptable PM2.5 AQI & Speciation Mass     41860
    ## 4:              88502 Acceptable PM2.5 AQI & Speciation Mass     41860
    ## 5:              88502 Acceptable PM2.5 AQI & Speciation Mass     41860
    ## 6:              88502 Acceptable PM2.5 AQI & Speciation Mass     41860
    ##                            CBSA_NAME STATE_CODE      STATE COUNTY_CODE  COUNTY
    ## 1: San Francisco-Oakland-Hayward, CA          6 California           1 Alameda
    ## 2: San Francisco-Oakland-Hayward, CA          6 California           1 Alameda
    ## 3: San Francisco-Oakland-Hayward, CA          6 California           1 Alameda
    ## 4: San Francisco-Oakland-Hayward, CA          6 California           1 Alameda
    ## 5: San Francisco-Oakland-Hayward, CA          6 California           1 Alameda
    ## 6: San Francisco-Oakland-Hayward, CA          6 California           1 Alameda
    ##    SITE_LATITUDE SITE_LONGITUDE
    ## 1:      37.68753      -121.7842
    ## 2:      37.68753      -121.7842
    ## 3:      37.68753      -121.7842
    ## 4:      37.68753      -121.7842
    ## 5:      37.68753      -121.7842
    ## 6:      37.68753      -121.7842

``` r
tail(data04)
```

    ##          Date Source  Site ID POC Daily Mean PM2.5 Concentration    UNITS
    ## 1: 12/14/2004    AQS 61131003   1                             11 ug/m3 LC
    ## 2: 12/17/2004    AQS 61131003   1                             16 ug/m3 LC
    ## 3: 12/20/2004    AQS 61131003   1                             17 ug/m3 LC
    ## 4: 12/23/2004    AQS 61131003   1                              9 ug/m3 LC
    ## 5: 12/26/2004    AQS 61131003   1                             24 ug/m3 LC
    ## 6: 12/29/2004    AQS 61131003   1                              9 ug/m3 LC
    ##    DAILY_AQI_VALUE            Site Name DAILY_OBS_COUNT PERCENT_COMPLETE
    ## 1:              46 Woodland-Gibson Road               1              100
    ## 2:              59 Woodland-Gibson Road               1              100
    ## 3:              61 Woodland-Gibson Road               1              100
    ## 4:              38 Woodland-Gibson Road               1              100
    ## 5:              76 Woodland-Gibson Road               1              100
    ## 6:              38 Woodland-Gibson Road               1              100
    ##    AQS_PARAMETER_CODE       AQS_PARAMETER_DESC CBSA_CODE
    ## 1:              88101 PM2.5 - Local Conditions     40900
    ## 2:              88101 PM2.5 - Local Conditions     40900
    ## 3:              88101 PM2.5 - Local Conditions     40900
    ## 4:              88101 PM2.5 - Local Conditions     40900
    ## 5:              88101 PM2.5 - Local Conditions     40900
    ## 6:              88101 PM2.5 - Local Conditions     40900
    ##                                  CBSA_NAME STATE_CODE      STATE COUNTY_CODE
    ## 1: Sacramento--Roseville--Arden-Arcade, CA          6 California         113
    ## 2: Sacramento--Roseville--Arden-Arcade, CA          6 California         113
    ## 3: Sacramento--Roseville--Arden-Arcade, CA          6 California         113
    ## 4: Sacramento--Roseville--Arden-Arcade, CA          6 California         113
    ## 5: Sacramento--Roseville--Arden-Arcade, CA          6 California         113
    ## 6: Sacramento--Roseville--Arden-Arcade, CA          6 California         113
    ##    COUNTY SITE_LATITUDE SITE_LONGITUDE
    ## 1:   Yolo      38.66121      -121.7327
    ## 2:   Yolo      38.66121      -121.7327
    ## 3:   Yolo      38.66121      -121.7327
    ## 4:   Yolo      38.66121      -121.7327
    ## 5:   Yolo      38.66121      -121.7327
    ## 6:   Yolo      38.66121      -121.7327

``` r
head(data19)
```

    ##          Date Source  Site ID POC Daily Mean PM2.5 Concentration    UNITS
    ## 1: 01/01/2019    AQS 60010007   3                            5.7 ug/m3 LC
    ## 2: 01/02/2019    AQS 60010007   3                           11.9 ug/m3 LC
    ## 3: 01/03/2019    AQS 60010007   3                           20.1 ug/m3 LC
    ## 4: 01/04/2019    AQS 60010007   3                           28.8 ug/m3 LC
    ## 5: 01/05/2019    AQS 60010007   3                           11.2 ug/m3 LC
    ## 6: 01/06/2019    AQS 60010007   3                            2.7 ug/m3 LC
    ##    DAILY_AQI_VALUE Site Name DAILY_OBS_COUNT PERCENT_COMPLETE
    ## 1:              24 Livermore               1              100
    ## 2:              50 Livermore               1              100
    ## 3:              68 Livermore               1              100
    ## 4:              86 Livermore               1              100
    ## 5:              47 Livermore               1              100
    ## 6:              11 Livermore               1              100
    ##    AQS_PARAMETER_CODE       AQS_PARAMETER_DESC CBSA_CODE
    ## 1:              88101 PM2.5 - Local Conditions     41860
    ## 2:              88101 PM2.5 - Local Conditions     41860
    ## 3:              88101 PM2.5 - Local Conditions     41860
    ## 4:              88101 PM2.5 - Local Conditions     41860
    ## 5:              88101 PM2.5 - Local Conditions     41860
    ## 6:              88101 PM2.5 - Local Conditions     41860
    ##                            CBSA_NAME STATE_CODE      STATE COUNTY_CODE  COUNTY
    ## 1: San Francisco-Oakland-Hayward, CA          6 California           1 Alameda
    ## 2: San Francisco-Oakland-Hayward, CA          6 California           1 Alameda
    ## 3: San Francisco-Oakland-Hayward, CA          6 California           1 Alameda
    ## 4: San Francisco-Oakland-Hayward, CA          6 California           1 Alameda
    ## 5: San Francisco-Oakland-Hayward, CA          6 California           1 Alameda
    ## 6: San Francisco-Oakland-Hayward, CA          6 California           1 Alameda
    ##    SITE_LATITUDE SITE_LONGITUDE
    ## 1:      37.68753      -121.7842
    ## 2:      37.68753      -121.7842
    ## 3:      37.68753      -121.7842
    ## 4:      37.68753      -121.7842
    ## 5:      37.68753      -121.7842
    ## 6:      37.68753      -121.7842

``` r
tail(data19)
```

    ##          Date Source  Site ID POC Daily Mean PM2.5 Concentration    UNITS
    ## 1: 11/11/2019    AQS 61131003   1                           13.5 ug/m3 LC
    ## 2: 11/17/2019    AQS 61131003   1                           18.1 ug/m3 LC
    ## 3: 11/29/2019    AQS 61131003   1                           12.5 ug/m3 LC
    ## 4: 12/17/2019    AQS 61131003   1                           23.8 ug/m3 LC
    ## 5: 12/23/2019    AQS 61131003   1                            1.0 ug/m3 LC
    ## 6: 12/29/2019    AQS 61131003   1                            9.1 ug/m3 LC
    ##    DAILY_AQI_VALUE            Site Name DAILY_OBS_COUNT PERCENT_COMPLETE
    ## 1:              54 Woodland-Gibson Road               1              100
    ## 2:              64 Woodland-Gibson Road               1              100
    ## 3:              52 Woodland-Gibson Road               1              100
    ## 4:              76 Woodland-Gibson Road               1              100
    ## 5:               4 Woodland-Gibson Road               1              100
    ## 6:              38 Woodland-Gibson Road               1              100
    ##    AQS_PARAMETER_CODE       AQS_PARAMETER_DESC CBSA_CODE
    ## 1:              88101 PM2.5 - Local Conditions     40900
    ## 2:              88101 PM2.5 - Local Conditions     40900
    ## 3:              88101 PM2.5 - Local Conditions     40900
    ## 4:              88101 PM2.5 - Local Conditions     40900
    ## 5:              88101 PM2.5 - Local Conditions     40900
    ## 6:              88101 PM2.5 - Local Conditions     40900
    ##                                  CBSA_NAME STATE_CODE      STATE COUNTY_CODE
    ## 1: Sacramento--Roseville--Arden-Arcade, CA          6 California         113
    ## 2: Sacramento--Roseville--Arden-Arcade, CA          6 California         113
    ## 3: Sacramento--Roseville--Arden-Arcade, CA          6 California         113
    ## 4: Sacramento--Roseville--Arden-Arcade, CA          6 California         113
    ## 5: Sacramento--Roseville--Arden-Arcade, CA          6 California         113
    ## 6: Sacramento--Roseville--Arden-Arcade, CA          6 California         113
    ##    COUNTY SITE_LATITUDE SITE_LONGITUDE
    ## 1:   Yolo      38.66121      -121.7327
    ## 2:   Yolo      38.66121      -121.7327
    ## 3:   Yolo      38.66121      -121.7327
    ## 4:   Yolo      38.66121      -121.7327
    ## 5:   Yolo      38.66121      -121.7327
    ## 6:   Yolo      38.66121      -121.7327

The 2019 data set has nearly 3 times the number of observations than the
2004 set. All 20 variables in the data are consistent between both sets.
There are no missing values. PM 2.5 measurements range from -0.10 to
251in the 2004 set, and -2.2 to 120 in the 2019 set, and have means of
13.3 and 7.7 respectively. The data is organized by site name and then
chronologically. Both data sets start with observations in Livermore and
end with observations on Woodland-Gibson Road.

Both data sets contain negative values of PM2.5, which are impossible
and need to be remedied.

# Step 2

Combine the two years of data into one data frame. Use the Date variable
to create a new column for year, which will serve as an identifier.
Change the names of the key variables so that they are easier to refer
to in your code.

``` r
combined <- rbind(data04, data19)
combined$Date <- as.Date(combined$Date, format="%m/%d/%Y")
combined$Year <- as.factor(format(combined$Date,'%Y'))
combined$COUNTY <- as.factor(combined$COUNTY)
combined$`Site Name` <- as.factor(combined$`Site Name`)
combined
```

    ##              Date Source  Site ID POC Daily Mean PM2.5 Concentration    UNITS
    ##     1: 2004-01-01    AQS 60010007   1                            8.9 ug/m3 LC
    ##     2: 2004-01-02    AQS 60010007   1                           12.2 ug/m3 LC
    ##     3: 2004-01-03    AQS 60010007   1                           16.5 ug/m3 LC
    ##     4: 2004-01-04    AQS 60010007   1                           19.5 ug/m3 LC
    ##     5: 2004-01-05    AQS 60010007   1                           11.5 ug/m3 LC
    ##    ---                                                                       
    ## 72385: 2019-11-17    AQS 61131003   1                           18.1 ug/m3 LC
    ## 72386: 2019-11-29    AQS 61131003   1                           12.5 ug/m3 LC
    ## 72387: 2019-12-17    AQS 61131003   1                           23.8 ug/m3 LC
    ## 72388: 2019-12-23    AQS 61131003   1                            1.0 ug/m3 LC
    ## 72389: 2019-12-29    AQS 61131003   1                            9.1 ug/m3 LC
    ##        DAILY_AQI_VALUE            Site Name DAILY_OBS_COUNT PERCENT_COMPLETE
    ##     1:              37            Livermore               1              100
    ##     2:              51            Livermore               1              100
    ##     3:              60            Livermore               1              100
    ##     4:              67            Livermore               1              100
    ##     5:              48            Livermore               1              100
    ##    ---                                                                      
    ## 72385:              64 Woodland-Gibson Road               1              100
    ## 72386:              52 Woodland-Gibson Road               1              100
    ## 72387:              76 Woodland-Gibson Road               1              100
    ## 72388:               4 Woodland-Gibson Road               1              100
    ## 72389:              38 Woodland-Gibson Road               1              100
    ##        AQS_PARAMETER_CODE                     AQS_PARAMETER_DESC CBSA_CODE
    ##     1:              88101               PM2.5 - Local Conditions     41860
    ##     2:              88502 Acceptable PM2.5 AQI & Speciation Mass     41860
    ##     3:              88502 Acceptable PM2.5 AQI & Speciation Mass     41860
    ##     4:              88502 Acceptable PM2.5 AQI & Speciation Mass     41860
    ##     5:              88502 Acceptable PM2.5 AQI & Speciation Mass     41860
    ##    ---                                                                    
    ## 72385:              88101               PM2.5 - Local Conditions     40900
    ## 72386:              88101               PM2.5 - Local Conditions     40900
    ## 72387:              88101               PM2.5 - Local Conditions     40900
    ## 72388:              88101               PM2.5 - Local Conditions     40900
    ## 72389:              88101               PM2.5 - Local Conditions     40900
    ##                                      CBSA_NAME STATE_CODE      STATE
    ##     1:       San Francisco-Oakland-Hayward, CA          6 California
    ##     2:       San Francisco-Oakland-Hayward, CA          6 California
    ##     3:       San Francisco-Oakland-Hayward, CA          6 California
    ##     4:       San Francisco-Oakland-Hayward, CA          6 California
    ##     5:       San Francisco-Oakland-Hayward, CA          6 California
    ##    ---                                                              
    ## 72385: Sacramento--Roseville--Arden-Arcade, CA          6 California
    ## 72386: Sacramento--Roseville--Arden-Arcade, CA          6 California
    ## 72387: Sacramento--Roseville--Arden-Arcade, CA          6 California
    ## 72388: Sacramento--Roseville--Arden-Arcade, CA          6 California
    ## 72389: Sacramento--Roseville--Arden-Arcade, CA          6 California
    ##        COUNTY_CODE  COUNTY SITE_LATITUDE SITE_LONGITUDE Year
    ##     1:           1 Alameda      37.68753      -121.7842 2004
    ##     2:           1 Alameda      37.68753      -121.7842 2004
    ##     3:           1 Alameda      37.68753      -121.7842 2004
    ##     4:           1 Alameda      37.68753      -121.7842 2004
    ##     5:           1 Alameda      37.68753      -121.7842 2004
    ##    ---                                                      
    ## 72385:         113    Yolo      38.66121      -121.7327 2019
    ## 72386:         113    Yolo      38.66121      -121.7327 2019
    ## 72387:         113    Yolo      38.66121      -121.7327 2019
    ## 72388:         113    Yolo      38.66121      -121.7327 2019
    ## 72389:         113    Yolo      38.66121      -121.7327 2019

``` r
combined <- combined %>% 
  relocate(Year, .before=Date) %>% 
  rename(
    PM2.5 = `Daily Mean PM2.5 Concentration`,
    Lat = SITE_LATITUDE,
    Long = SITE_LONGITUDE)
combined
```

    ##        Year       Date Source  Site ID POC PM2.5    UNITS DAILY_AQI_VALUE
    ##     1: 2004 2004-01-01    AQS 60010007   1   8.9 ug/m3 LC              37
    ##     2: 2004 2004-01-02    AQS 60010007   1  12.2 ug/m3 LC              51
    ##     3: 2004 2004-01-03    AQS 60010007   1  16.5 ug/m3 LC              60
    ##     4: 2004 2004-01-04    AQS 60010007   1  19.5 ug/m3 LC              67
    ##     5: 2004 2004-01-05    AQS 60010007   1  11.5 ug/m3 LC              48
    ##    ---                                                                   
    ## 72385: 2019 2019-11-17    AQS 61131003   1  18.1 ug/m3 LC              64
    ## 72386: 2019 2019-11-29    AQS 61131003   1  12.5 ug/m3 LC              52
    ## 72387: 2019 2019-12-17    AQS 61131003   1  23.8 ug/m3 LC              76
    ## 72388: 2019 2019-12-23    AQS 61131003   1   1.0 ug/m3 LC               4
    ## 72389: 2019 2019-12-29    AQS 61131003   1   9.1 ug/m3 LC              38
    ##                   Site Name DAILY_OBS_COUNT PERCENT_COMPLETE AQS_PARAMETER_CODE
    ##     1:            Livermore               1              100              88101
    ##     2:            Livermore               1              100              88502
    ##     3:            Livermore               1              100              88502
    ##     4:            Livermore               1              100              88502
    ##     5:            Livermore               1              100              88502
    ##    ---                                                                         
    ## 72385: Woodland-Gibson Road               1              100              88101
    ## 72386: Woodland-Gibson Road               1              100              88101
    ## 72387: Woodland-Gibson Road               1              100              88101
    ## 72388: Woodland-Gibson Road               1              100              88101
    ## 72389: Woodland-Gibson Road               1              100              88101
    ##                            AQS_PARAMETER_DESC CBSA_CODE
    ##     1:               PM2.5 - Local Conditions     41860
    ##     2: Acceptable PM2.5 AQI & Speciation Mass     41860
    ##     3: Acceptable PM2.5 AQI & Speciation Mass     41860
    ##     4: Acceptable PM2.5 AQI & Speciation Mass     41860
    ##     5: Acceptable PM2.5 AQI & Speciation Mass     41860
    ##    ---                                                 
    ## 72385:               PM2.5 - Local Conditions     40900
    ## 72386:               PM2.5 - Local Conditions     40900
    ## 72387:               PM2.5 - Local Conditions     40900
    ## 72388:               PM2.5 - Local Conditions     40900
    ## 72389:               PM2.5 - Local Conditions     40900
    ##                                      CBSA_NAME STATE_CODE      STATE
    ##     1:       San Francisco-Oakland-Hayward, CA          6 California
    ##     2:       San Francisco-Oakland-Hayward, CA          6 California
    ##     3:       San Francisco-Oakland-Hayward, CA          6 California
    ##     4:       San Francisco-Oakland-Hayward, CA          6 California
    ##     5:       San Francisco-Oakland-Hayward, CA          6 California
    ##    ---                                                              
    ## 72385: Sacramento--Roseville--Arden-Arcade, CA          6 California
    ## 72386: Sacramento--Roseville--Arden-Arcade, CA          6 California
    ## 72387: Sacramento--Roseville--Arden-Arcade, CA          6 California
    ## 72388: Sacramento--Roseville--Arden-Arcade, CA          6 California
    ## 72389: Sacramento--Roseville--Arden-Arcade, CA          6 California
    ##        COUNTY_CODE  COUNTY      Lat      Long
    ##     1:           1 Alameda 37.68753 -121.7842
    ##     2:           1 Alameda 37.68753 -121.7842
    ##     3:           1 Alameda 37.68753 -121.7842
    ##     4:           1 Alameda 37.68753 -121.7842
    ##     5:           1 Alameda 37.68753 -121.7842
    ##    ---                                       
    ## 72385:         113    Yolo 38.66121 -121.7327
    ## 72386:         113    Yolo 38.66121 -121.7327
    ## 72387:         113    Yolo 38.66121 -121.7327
    ## 72388:         113    Yolo 38.66121 -121.7327
    ## 72389:         113    Yolo 38.66121 -121.7327

# Step 3

Create a basic map in leaflet() that shows the locations of the sites
(make sure to use different colors for each year). Summarize the spatial
distribution of the monitoring sites.

``` r
pal <- colorFactor(c('steelblue','tomato'), domain=combined$Year)
yrs <- unique(combined$Year)
```

``` r
sites <- unique(combined[ , c('Year', 'Site ID', 'Site Name', 'Lat', 'Long')])
sites
```

    ##      Year  Site ID                    Site Name      Lat      Long
    ##   1: 2004 60010007                    Livermore 37.68753 -121.7842
    ##   2: 2004 60011001         Fremont - Chapel Way 37.53583 -121.9618
    ##   3: 2004 60070002         Chico-Manzanita Ave. 39.75737 -121.8433
    ##   4: 2004 60074001    TRAFFIC, RURAL PAVED ROAD 39.32756 -121.6688
    ##   5: 2004 60090001 San Andreas-Gold Strike Road 38.20185 -120.6803
    ##  ---                                                              
    ## 262: 2019 61111004         Ojai - East Ojai Ave 34.44806 -119.2313
    ## 263: 2019 61112002   Simi Valley-Cochran Street 34.27632 -118.6837
    ## 264: 2019 61113001    El Rio-Rio Mesa School #2 34.25239 -119.1432
    ## 265: 2019 61130004             Davis-UCD Campus 38.53445 -121.7734
    ## 266: 2019 61131003         Woodland-Gibson Road 38.66121 -121.7327

``` r
site_map <- leaflet(sites) %>% 
  # The looks of the Map
  addProviderTiles('CartoDB.Positron') %>% 
  # Some circles
  addCircles(
    lat = ~Lat, lng = ~Long,
    color = ~pal(yrs),
    opacity = .7, fillOpacity = 1, radius = 500
    ) %>%
  addLegend('bottomleft', pal = pal, values = yrs,
          title='Site Locations', opacity=1)
site_map
```

![](README_files/figure-gfm/site%20map-1.png)<!-- --> There are sites
located all throughout California, with concentrations in cities along
the coast in major cities, such as San Francisco, San Jose, Los Angeles,
and San Diego.

# Step 4

Check for any missing or implausible values of PM in the combined
dataset. Explore the proportions of each and provide a summary of any
temporal patterns you see in these observations.

``` r
summary(combined$PM2.5)
```

    ##    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
    ##   -2.20    4.40    7.20    9.17   11.30  251.00

``` r
nrow(combined[PM2.5 < 0])
```

    ## [1] 283

``` r
nrow(combined[is.na(PM2.5)])
```

    ## [1] 0

``` r
combined <- combined[PM2.5 > 0]
```

``` r
combined$yday <- yday(combined$Date)

yday <- combined %>%
  group_by(yday, Year) %>%
  summarize(PM2.5_avg = mean(PM2.5))
```

    ## `summarise()` has grouped output by 'yday'. You can override using the `.groups` argument.

``` r
ggplot(yday, aes(x=PM2.5_avg, fill=Year)) +
  geom_histogram(binwidth = 0.5, position='dodge') +
  theme(legend.position='right')
```

![](README_files/figure-gfm/unnamed-chunk-14-1.png)<!-- --> 238 of the
total 72389 observations had an implausible negative PM 2.5 value,
comprising 0.4% of total measurements. PM 2.5 measurements skewed left
with the median at 7.20 with a max value of 251 (this max measurement is
valid, as California fires can cause this spike in air quality).

# Step 5

Explore the main question of interest at three different spatial levels.
Create exploratory plots (e.g. boxplots, histograms, line plots) and
summary statistics that best suit each level of data. Be sure to write
up explanations of what you observe in these data.

## State

``` r
state_level <- combined %>%
  group_by(yday,Year) %>%
  summarize(PM2.5_avg = mean(PM2.5))
```

    ## `summarise()` has grouped output by 'yday'. You can override using the `.groups` argument.

``` r
state_plot <- ggplot(state_level, aes(x=yday, y=PM2.5_avg, color = Year)) +
  geom_line(aes(fill=Year)) +
  labs(title="Temporal Patterns of Average PM 2.5 For All Sites",
        x ="Day of the Year", y = "PM2.5 (ug/m3)",
        color = "Year")
```

    ## Warning: Ignoring unknown aesthetics: fill

``` r
state_plot
```

![](README_files/figure-gfm/unnamed-chunk-16-1.png)<!-- --> Compared to
2019, the PM 2.5 levels of 2004 visually shows to be overall higher
throughout the year. Especially towards the beginning and ending months.

## County

``` r
county_level <- combined %>%
  group_by(COUNTY, Year) %>%
  summarize(PM2.5_avg = mean(PM2.5))
```

    ## `summarise()` has grouped output by 'COUNTY'. You can override using the `.groups` argument.

``` r
county_level
```

    ## # A tibble: 98 x 3
    ## # Groups:   COUNTY [51]
    ##    COUNTY       Year  PM2.5_avg
    ##    <fct>        <fct>     <dbl>
    ##  1 Alameda      2004      11.1 
    ##  2 Alameda      2019       7.34
    ##  3 Butte        2004      10.1 
    ##  4 Butte        2019       7.01
    ##  5 Calaveras    2004       7.61
    ##  6 Calaveras    2019       5.46
    ##  7 Colusa       2004      10.0 
    ##  8 Colusa       2019       6.63
    ##  9 Contra Costa 2004      12.8 
    ## 10 Contra Costa 2019       7.20
    ## # ... with 88 more rows

``` r
county_bar <- ggplot(county_level, aes(x=PM2.5_avg, y=COUNTY, fill=Year)) +
  geom_bar(stat = 'identity', alpha=0.5) +
  theme(legend.position="right")

county_bar
```

![](README_files/figure-gfm/unnamed-chunk-18-1.png)<!-- --> The same
pattern is shown between PM 2.5 averages by county, where 2004 PM 2.5
levels are much higher than in 2019.

## Site in Los Angeles

``` r
sites_la <- combined[COUNTY=="Los Angeles"] %>%
  group_by(`Site Name`,Year)

sites_la
```

    ## # A tibble: 7,155 x 22
    ## # Groups:   Site Name, Year [24]
    ##    Year  Date       Source `Site ID`   POC PM2.5 UNITS    DAILY_AQI_VALUE
    ##    <fct> <date>     <chr>      <int> <int> <dbl> <chr>              <int>
    ##  1 2004  2004-01-01 AQS     60370002     1  18   ug/m3 LC              63
    ##  2 2004  2004-01-02 AQS     60370002     1  20.4 ug/m3 LC              68
    ##  3 2004  2004-01-03 AQS     60370002     1   8   ug/m3 LC              33
    ##  4 2004  2004-01-07 AQS     60370002     1  23.6 ug/m3 LC              75
    ##  5 2004  2004-01-08 AQS     60370002     1  28.3 ug/m3 LC              85
    ##  6 2004  2004-01-09 AQS     60370002     1  21.9 ug/m3 LC              72
    ##  7 2004  2004-01-10 AQS     60370002     1   9.1 ug/m3 LC              38
    ##  8 2004  2004-01-11 AQS     60370002     1  12.5 ug/m3 LC              52
    ##  9 2004  2004-01-12 AQS     60370002     1  42.2 ug/m3 LC             117
    ## 10 2004  2004-01-13 AQS     60370002     1   9.1 ug/m3 LC              38
    ## # ... with 7,145 more rows, and 14 more variables: Site Name <fct>,
    ## #   DAILY_OBS_COUNT <int>, PERCENT_COMPLETE <dbl>, AQS_PARAMETER_CODE <int>,
    ## #   AQS_PARAMETER_DESC <chr>, CBSA_CODE <int>, CBSA_NAME <chr>,
    ## #   STATE_CODE <int>, STATE <chr>, COUNTY_CODE <int>, COUNTY <fct>, Lat <dbl>,
    ## #   Long <dbl>, yday <dbl>

``` r
sites_violin <- ggplot(sites_la, aes(x=PM2.5, y=`Site Name`, fill=Year, color=Year), fig(5, 30)) +
  geom_violin(position=position_dodge(1)) +
  theme(legend.position="right")

sites_violin
```

![](README_files/figure-gfm/unnamed-chunk-20-1.png)<!-- -->
