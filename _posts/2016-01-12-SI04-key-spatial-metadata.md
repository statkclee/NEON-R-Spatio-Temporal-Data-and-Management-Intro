---
layout: post
title: "Lesson 04: Key Spatial Metadata to Understand"
date:   2015-10-26
authors: [Leah Wasser, Megan A. Jones, Dave Roberts, Tracy Teal, Kaitlin Stack Whitney]
contributors: [ ]
dateCreated: 2015-10-23
lastModified: 2016-01-12
packagesLibraries: [ ]
category: [self-paced-tutorial] 
tags: [R, GIS-Spatial-Data, metdata-eml, informatics]
mainTag: spatial-data-management-series
description: "Add description here."
code1: key-spatial-metadata.R
image:
  feature: NEONCarpentryHeader_2.png
  credit: A collaboration between the National Ecological Observatory Network (NEON) and Data Carpentry
  creditlink: http://www.neoninc.org
permalink: R/key-spatial-metadata
comments: false
---

{% include _toc.html %}

##About
Add description.

**R Skill Level:** Intermediate - you've got the basics of `R` down.

<div id="objectives" markdown="1">

#Goals / Objectives

After completing this activity, you will:

* Understand that there are necessary spatial metadata associated with and/or
embedded in the data
* Understand that there is potentially ancillary data associated with individual
elements in vector data files (like NEON tower data (point), road (line), watershed (polygon)).


##Things You’ll Need To Complete This Lesson
To complete this lesson: you will need the most current version of R, and 
preferably RStudio, loaded on your computer.

###Install R Packages

* **NAME:** `install.packages("NAME")`

* [More on Packages in R - Adapted from Software Carpentry.]({{site.baseurl}}R/Packages-In-R/)

###Download Data


****

{% include/_greyBox-wd-rscript.html %}

**Raster Lesson Series:** This lesson is part of a lesson series introducing
[spatial data and data management in `R` ]({{ site.baseurl }}tutorial/URL).
It is also part of a larger 
[spatio-temporal Data Carpentry Workshop ]({{ site.baseurl }}workshops/spatio-temporal-workshop)
that includes working with  
[raster data in `R` ]({{ site.baseurl }}tutorial/spatial-raster-series),
[vector data in `R` ]({{ site.baseurl }}tutorial/spatial-vector-series)
and  
[tabular time series in `R` ]({{ site.baseurl }}tutorial/tabular-time-series).

****

###Additional Resources
* Read more on coordinate systems in the
<a href="http://docs.qgis.org/2.0/en/docs/gentle_gis_introduction/coordinate_reference_systems.html" target="_blank">
QGIS documentation.</a>
* NEON Data Skills Lesson <a href="{{ site.baseurl }}/GIS-Spatial-Data/Working-With-Rasters/" target="_blank">The Relationship Between Raster Resolution, Spatial extent & Number of Pixels - in R</a>

</div>

##Spatial Metadata
There are three core spatial metadata elements that are crucial to understand
in order to effectively work with any sort of spatial data: 

* Coordinate Reference System (CRS), 
* Extent
* Resolution 

##What is a Coordinate Reference System
> A spatial reference system (SRS) or coordinate reference system (CRS) is a 
coordinate-based local, regional or global system used to locate geographical 
entities. -- Wikipedia

The earth is round. This is not an new concept by any means, however we need to 
remember this when we talk about coordinate reference systems associated with 
spatial data. When we make maps on paper or on a computer screen, we are moving 
from a 3 dimensional space (the globe) to 2 dimensions (our computer screens or 
a piece of paper). To keep this short, the projection of a dataset relates to 
how the data are "flattened" in geographic space so our human eyes and brains 
can make sense of the information in 2 dimensions. 

###Coordinate System

###Coordinate Units
x,y values for point locations - Examples: UTM coordinates: (meters) or in decimal degrees (lat long) or in feet (state plane)

###Datum - how and where 0,0 is defined), 
examples: geographic - 00 is...

##Projection for CRS
The projection refers to the mathematical calculations performed to "flatten the 
data" in into 2D space. The coordinate system references to the x and y
coordinate space that is associated with the projection used to flatten the
data. If you have the same dataset saved in two different projections, these two
files won't line up correctly when rendered together.

####How Map Projections Can Fool the Eye
Check out this short video highlighting how map projections can make continents 
seems proportionally larger or smaller than they actually are!

<iframe width="560" height="315" src="https://www.youtube.com/embed/KUF_Ckv8HbE" frameborder="0" allowfullscreen></iframe>

####Same Country Different Projections
<figure>
    <a href="https://source.opennews.org/media/cache/b9/4f/b94f663c79024f0048ae7b4f88060cb5.jpg">
    <img src="https://source.opennews.org/media/cache/b9/4f/b94f663c79024f0048ae7b4f88060cb5.jpg">
    </a>
    
    <figcaption>Maps of the United States in different projections. Notice the 
    differences in shape associated with each different projection. These 
    differences are a direct result of the calculations used to "flatten" the 
    data onto a 2 dimensional map. Source: opennews.org</figcaption>
</figure>

<a href="https://source.opennews.org/en-US/learning/choosing-right-map-projection/" target="_blank">Read more about projections.</a>

##What Makes Spatial Data Line Up On A Map?
There are lots of great resources that describe coordinate reference systems and 
projections in greater detail. However, for the purposes of this activity, what 
is important to understand is that data from the same location but saved in 
different projections **will not line up in any GIS or other program**. Thus 
it's important when working with spatial data in a program like `R` or `Python` 
to identify the coordinate reference system applied to the data, and to grab 
that information and retain it when you process / analyze the data.

*<a href="http://spatialreference.org/ref/epsg/" target="_blank">A great online 
library of CRS information.</a>

## CRS Strings


    library('rgdal')
    epsg = make_EPSG()
    #View(epsg)
    head(epsg)

    ##   code                                               note
    ## 1 3819                                           # HD1909
    ## 2 3821                                            # TWD67
    ## 3 3824                                            # TWD97
    ## 4 3889                                             # IGRS
    ## 5 3906                                         # MGI 1901
    ## 6 4001 # Unknown datum based upon the Airy 1830 ellipsoid
    ##                                                                                            prj4
    ## 1 +proj=longlat +ellps=bessel +towgs84=595.48,121.69,515.35,4.115,-2.9383,0.853,-3.408 +no_defs
    ## 2                                                         +proj=longlat +ellps=aust_SA +no_defs
    ## 3                                    +proj=longlat +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +no_defs
    ## 4                                    +proj=longlat +ellps=GRS80 +towgs84=0,0,0,0,0,0,0 +no_defs
    ## 5                            +proj=longlat +ellps=bessel +towgs84=682,-203,480,0,0,0,0 +no_defs
    ## 6                                                            +proj=longlat +ellps=airy +no_defs

#Geographic vs Projected Coordinate Systems
(covered on the NCEAS page too so we don’t have to go into it beyond a sentence or two and then a link out)

Lat Long: geographic, 
units: decimal degrees
Scale: global

UTM: Projected
Easting / Northing
Units (meters or feet) - most often meters
Scale - Regional (globe divided into zones)

###Coordinate Reference System Formats
Proj vs EPSG vs WKT
https://www.nceas.ucsb.edu/scicomp/recipes/projections
https://www.nceas.ucsb.edu/~frazier/RSpatialGuides/OverviewCoordinateReferenceSystems.pdf
http://spatialreference.org/ref/epsg/

##Spatial Extent
The spatial extent of a raster, represents the x,y coordinates of the corners 
of the raster in geographic space. This information, in addition to the cell 
size or spatial resolution, tells the program how to place or render each pixel 
in 2 dimensional space.  Tools like `R`, using supporting packages such as
`rgdal`, and associated raster tools have functions that allow you to view and
define the extent of a new raster. 


    # View the extent of the raster
    DEM@extent

    ## Error in eval(expr, envir, enclos): object 'DEM' not found

<figure>
    <img src="{{ site.baseurl }}/images/spatialData/raster2.png">
    <figcaption>If you double the extent value of a raster - the pixels will be
    stretched over the larger area making it look more "blury".
    </figcaption>
</figure>
###Units

###Extent in Raster Data

###Extent in Vector Data

###Calculating Raster Extent
Extent and spatial resolution are closely connected. To calculate the extent of
a raster, we first need the bottom left (x,y) coordinate of the raster. In 
the case of the UTM coordinate system which is in meters, to calculate
the raster's extent, we can add the number of columns and rows to the x,y corner 
coordinate location of the raster, multiplied by the resolution (the pixel size) 
of the raster.

Let's explore that next.


    #create a raster from the matrix
    myRaster1 <- raster(nrow=4, ncol=4)
        
    #assign some random data to the raster
    myRaster1[]<- 1:ncell(myRaster1)
        
    #view attributes of the raster
    myRaster1

    ## class       : RasterLayer 
    ## dimensions  : 4, 4, 16  (nrow, ncol, ncell)
    ## resolution  : 90, 45  (x, y)
    ## extent      : -180, 180, -90, 90  (xmin, xmax, ymin, ymax)
    ## coord. ref. : +proj=longlat +datum=WGS84 +ellps=WGS84 +towgs84=0,0,0 
    ## data source : in memory
    ## names       : layer 
    ## values      : 1, 16  (min, max)

    #is the CRS defined?
    myRaster1@crs

    ## CRS arguments:
    ##  +proj=longlat +datum=WGS84 +ellps=WGS84 +towgs84=0,0,0

    #what are the data extents?
    myRaster1@extent

    ## class       : Extent 
    ## xmin        : -180 
    ## xmax        : 180 
    ## ymin        : -90 
    ## ymax        : 90

    plot(myRaster1, main="Raster with 16 pixels")

![ ]({{ site.baseurl }}/images/rfigs/04-key-spatial-metadata/calculate-raster-extent-1.png) 

##Spatial Resolution
A raster consists of a series of pixels, each with the same dimensions 
and shape. In the case of rasters derived from airborne sensors, each pixel 
represents an area of space on the Earth's surface. The size of the area on the 
surface that each pixel covers is known as the spatial resolution of the image. 
For instance, an image that has a 1 m spatial resolution means that each pixel in 
the image represents a 1 m x 1 m area.

<figure>
    <a href="{{ site.baseurl }}/images/hyperspectral/pixelDetail.png">
    <img src="{{ site.baseurl }}/images/hyperspectral/pixelDetail.png"></a>
    <figcaption>The spatial resolution of a raster refers the size of each cell 
    in meters. This size in turn relates to the area on the ground that the pixel 
    represents.</figcaption>
</figure>

<figure>
    <img src="{{ site.baseurl }}/images/spatialData/raster1.png">
    <figcaption>A raster at the same extent with more pixels will have a higher
    resolution (it looks more "crisp"). A raster that is stretched over the same
    extent with fewer pixels will look more blury and will be of lower resolution.
    </figcaption>
</figure>

Let's open up a raster in `R` to see how the attributes are stored.


    #load raster library
    library(raster)
    library(rgdal)
        
    # Load raster in an R object called 'DEM'
    DEM <- raster("DigitalTerrainModel/SJER2013_DTM.tif")  

    ## Error in .rasterObjectFromFile(x, band = band, objecttype = "RasterLayer", : Cannot create a RasterLayer object from this file. (file does not exist)

    # View raster attributes 
    DEM

    ## Error in eval(expr, envir, enclos): object 'DEM' not found

Notice that this raster (in GeoTIFF format) already has defined:

* extent
* resolution (1 in both x and y directions), and
* CRS (units in meters).

###Adjust Raster Resolution
We can resample the raster as well to adjust the resolution. If we want a higher
resolution raster, we will apply a grid with MORE pixels within the same extent.
If we want a LOWER resolution raster, we will apply a grid with LESS pixels
within the same extent.


    myRaster2 <- raster(nrow=8, ncol=8)
    myRaster2 <- resample(myRaster1, myRaster2, method='bilinear')
    myRaster2

    ## class       : RasterLayer 
    ## dimensions  : 8, 8, 64  (nrow, ncol, ncell)
    ## resolution  : 45, 22.5  (x, y)
    ## extent      : -180, 180, -90, 90  (xmin, xmax, ymin, ymax)
    ## coord. ref. : +proj=longlat +datum=WGS84 +ellps=WGS84 +towgs84=0,0,0 
    ## data source : in memory
    ## names       : layer 
    ## values      : -0.25, 17.25  (min, max)

    plot(myRaster2, main="Raster with 32 pixels")

![ ]({{ site.baseurl }}/images/rfigs/04-key-spatial-metadata/change-raster-resolution-1.png) 

    myRaster3 <- raster(nrow=2, ncol=2)
    myRaster3 <- resample(myRaster1, myRaster3, method='bilinear')
    myRaster3

    ## class       : RasterLayer 
    ## dimensions  : 2, 2, 4  (nrow, ncol, ncell)
    ## resolution  : 180, 90  (x, y)
    ## extent      : -180, 180, -90, 90  (xmin, xmax, ymin, ymax)
    ## coord. ref. : +proj=longlat +datum=WGS84 +ellps=WGS84 +towgs84=0,0,0 
    ## data source : in memory
    ## names       : layer 
    ## values      : 3.5, 13.5  (min, max)

    plot(myRaster3, main="Raster with 4 pixels")

![ ]({{ site.baseurl }}/images/rfigs/04-key-spatial-metadata/change-raster-resolution-2.png) 

    myRaster4

    ## Error in eval(expr, envir, enclos): object 'myRaster4' not found

    plot(myRaster4, main="Raster with 1 pixels")

    ## Error in plot(myRaster4, main = "Raster with 1 pixels"): error in evaluating the argument 'x' in selecting a method for function 'plot': Error: object 'myRaster4' not found

***
*** 
##Additional Resources
ESRI help on CRS
QGIS help on CRS
NCEAS cheatsheets
