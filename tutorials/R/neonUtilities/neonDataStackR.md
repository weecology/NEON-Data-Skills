---
syncID: a1388c25d16342cca2643bc2df3fbd8e
title: "Use the neonDataStackR Package to Access NEON Data"
description: "Convert data downloaded from the NEON Data Portal in zipped month-by-site files into a table with all data of interest. Temperature data are used as an example. "
dateCreated: 2017-08-01
authors: [Megan A. Jones ]
contributors: [ ]
estimatedTime: 5 minutes
packagesLibraries: neonDataStackR
topics: data-management, rep-sci
languageTool: R
code1: R/neonUtilities/neonDataStackR.R
tutorialSeries:
urlTitle: neonDataStackR

---

This tutorial goes over how to convert data downloaded from the NEON Data Portal 
in zipped month-by-site files into individual files with all data from the 
given site(s) and months. Temperature data are used as an example.

## Download the Data
To start, you must have your data of interest downloaded from the 
<a href="http://data.neonscience.org" target="_blank"> NEON Data Portal</a>.  

The stacking function will only work on zipped Comma Seperated Value (.csv) 
files and not the NEON data stored in other formats (HDF5, etc). 

Your data will download in a single zipped file. 

The example data below are any single-aspirated air temperature available from 
1 January 2015 to 31 December 2016. 

## neonDataStackR package

This package was written to stack data downloaded in month-by-site files into a 
full table with all the data of interest from all sites in the downloaded date
range.  

More information on the package see the README in the associated GitHub repo 
<a href="https://github.com/NEONScience/NEON-utilities/tree/master/neonDataStackR" target="_blank"> NEONScience/NEON-utilities</a>. 

First, we must install the package from the GitHub repo. You must have the 
**devtools** package installed and loaded to do this. Then load the package. 

    # install devtools - can skip if already installed
    install.packages("devtools")

    # load devtools
    library(devtools)

    # load neonDataStackR from GitHub
    install_github("NEONScience/NEON-utilities/neonDataStackR", dependencies=TRUE)

    ## Downloading GitHub repo NEONScience/NEON-utilities@master
    ## from URL https://api.github.com/repos/NEONScience/NEON-utilities/zipball/master

    ## Installing neonDataStackR

    ## '/Library/Frameworks/R.framework/Resources/bin/R' --no-site-file  \
    ##   --no-environ --no-save --no-restore --quiet CMD INSTALL  \
    ##   '/private/var/folders/0p/x8phw1_156511_jqkryx2t8m2vn2t3/T/RtmpPD2fan/devtools52c155f1e8a2/NEONScience-NEON-utilities-5872bcd/neonDataStackR'  \
    ##   --library='/Users/mjones01/Library/R/3.4/library' --install-tests

    ## 

    # load neonDataStackR
    library (neonDataStackR)

## stackByTable

There is a single function to run in this package `stackByTable()`. The output
will yield data grouped into new files by table name. For example the single 
aspirated air temperature data product contains 1 minute and 30 minute interval
data. The output from this function is one .csv with 1 minute data and one .csv 
with 30 minute data. 

Depending on your file size this function may run for a while. The 2015 and 2016
single aspirated air temperature from two sites that I used for a 2017 workshop
took about 25 minutes to complete. 

To run the `stackByTable()` function, simply use the file path to the downloaded
and zipped file. 


    # stack files - Mac OSX file path shown
    stackByTable("~neon/Documents/data/NEON_temp-air-single.zip")


    Unpacked  NEON.D10.CPER.DP1.00002.001.2016-07.basic.20171010T230533Z.zip
    Unpacked  NEON.D10.CPER.DP1.00002.001.2016-08.basic.20171011T101525Z.zip
    Unpacked  NEON.D10.CPER.DP1.00002.001.2016-09.basic.20171010T233829Z.zip
    Joining, by = c("domainID", "siteID", "horizontalPosition", "verticalPosition", 
    "startDateTime", "endDateTime", "tempSingleMean", "tempSingleMinimum", 
    "tempSingleMaximum", "tempSingleVariance", "tempSingleNumPts", "tempSingleExpUncert", 
    "tempSingleStdErMean", "finalQF")
    Joining, by = c("domainID", "siteID", "horizontalPosition", "verticalPosition", 
    "startDateTime", "endDateTime", "tempSingleMean", "tempSingleMinimum", "tempSingleMaximum", 
    "tempSingleVariance", "tempSingleNumPts", "tempSingleExpUncert", "tempSingleStdErMean", 
    "finalQF")

    # Note that I've removed some of the "Joining" output for ease of reading

    Finished: All of the data are stacked into  2  tables!
    Copied the first available variable definition file to /stackedFiles and renamed as variables.csv
    Stacked  SAAT_1min
    Stacked  SAAT_30min

From the single-aspirated air temperature data we are given two final tables. 
One with 1 minute intervals: **SAAT_1min** and one for 30 minute intervals: 
**SAAT_30min**.  

In the same directory as the zipped file, you should now have an unzipped 
directory of the same name. When you open this you will still see all the 
month-by-site files, however, you will also see a new directory called 
**stackedFiles**. This directory contains one or more .csv files (depends on the 
data product you are working with) with all the data from the months & sites you 
downloaded. There will also be a single copy of the associated varibles.csv and 
validation.csv files. 

These .csv files are now ready for use with the program of your choice. 


