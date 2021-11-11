# 52°North - Huddle - Python Series: Geospatial Libraries
## Meta

* **Authors**:
  * [SebaDro](https://github.com/SebaDro)

* **License**: [CC BY 4.0](http://creativecommons.org/licenses/by/4.0/)

## Content

This documents aims to give a brief overview about libraries for handling geospatial datasets in Python.
It will give some guidance in how to use the libraries and discuss common use cases.

## Libraries

### Fiona
https://github.com/Toblerity/Fiona  
Python wrapper for vector data access functionality of the OGR library. It solely provides readers
and writers for vector data. No geoprocessing and no coordinate transformation features. Depends
on [GDAL/OGR](https://gdal.org/index.html).

### Shapely
https://github.com/Toblerity/Shapely  
Python wrapper for geoprocessing operations of the GEOS library. It focuses on manipulation and
analysis of geometric objects. It neither provides data readers and writers nor coordinate transformation
functionalities. Depends on [GEOS](https://trac.osgeo.org/geos/).

### pyproj
https://pyproj4.github.io/pyproj/stable/
Python interface for the [PROJ library](https://proj.org/index.html) to transform geospatial
coordinates between different reference coordinate systems.

### Rasterio
https://rasterio.readthedocs.io/  
Python Wrapper for raster data access and processing using GDAL. It supports reading and writing of
various raster data formats as well as basic raster processing. Depends on [GDAL/OGR](https://gdal.org/index.html).

### GeoAlchemy
https://geoalchemy-2.readthedocs.io/en/latest/  
Extends [SQLAlchemy](https://www.sqlalchemy.org/) to support spatial databases. The focus is on
PostGIS and it integrates well with Shapely.

### GeoPandas
https://github.com/geopandas/geopandas  
Extends data types of the [pandas library](https://pandas.pydata.org/) by geometric types. It supports
data access, basic geoprocessing, coordinate transformations and basic plotting. For providing these
features it depends on Fiona, Shapely, pyproj and Matplotlib.

### PySal
https://github.com/pysal/pysal  
Library for geospatial data science. Comprises a familiy of Python packages for analyzing, modelling and
visualization of spatio-temporal data.

### PyQGIS
https://docs.qgis.org/3.22/en/docs/pyqgis_developer_cookbook/index.html  
Python API for QGIS for writing scripts or developing new QGIS plugins.

### GRASS Python Scripting Library
https://grasswiki.osgeo.org/wiki/GRASS_Python_Scripting_Library  
Python API that enables the use of GRASS GIS modules within custom scripts.

### PyGRASS
https://grass.osgeo.org/grass78/manuals/libpython/script_intro.html  
Object-oriented API for an easier use of GRASS GIS modules in Python.

## Use cases
### Geoprocessing
The combination of the former three libraries (Fiona, Shapely, pyproj) is suitable for tackling basic
geo data processing use cases.

### Spatial Data Science
GeoPandas may be used for basic spatial data science use cases as it integrates well with Jupyter Notebooks.   

In addition, PySal can be used for extensive spatial analysis tasks.

### GIS Analysis/Scripting
If you are familiar with QGIS or even GRASS GIS, the respective Python APIs may be suitable for you. These
libraries can also be used for implementing custom tools that can be integrated in GRAS GIS or QGIS. 