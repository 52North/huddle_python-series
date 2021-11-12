# 52°North - Huddle - Python Series: Geospatial Libraries
## Meta

* **Authors**:
  * [SebaDro](https://github.com/SebaDro)
  * [MartinPontius](https://github.com/MartinPontius)

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

### OWSLib
https://github.com/geopython/OWSLib  
Library for client programming with OWS interface standards and their related content models.

### sos4py (52N)
https://github.com/52North/sos4py  
Extends OWSLib`s SOS capabilities by providing a convenience layer to access SOS instances without much technical knowledge of the standards.
Inspired by [sos4R](https://github.com/52North/sos4R).

### OSMnx
https://github.com/gboeing/osmnx  
Library for working with street networks. It allows to download geospatial data from OpenStreetMap and to model, project, visualize, and analyze real-world street networks and any other geospatial geometries.

### envirocar-py (52N)
https://github.com/enviroCar/envirocar-py  
Library for querying and downloading trajectory data from the enviroCar API.

### Folium
https://github.com/python-visualization/folium  
Library for creating interactive leaflet maps. It is especially useful in Jupyter Notebooks.

### Cartopy
https://github.com/SciTools/cartopy  
Library for drawing maps.

### xarray
https://github.com/pydata/xarray  
Library that provides multi-dimensional data structures ("data cubes") with support for coordinates (e.g. time, latitude, longitude).
Its data model is based on [netCDF](https://www.unidata.ucar.edu/software/netcdf/). The library also allows visualizing data.

### rioxarray
https://github.com/corteva/rioxarray  
Geospatial extension of xarray for supporting rasterio operations on multi-dimensional data structures.  

### MetPy
https://github.com/Unidata/MetPy  
Library for reading, analyzing and visualizing weather data.

### geopy
https://github.com/geopy/geopy  
Library for geocoding and distance measuring.

### landsatxplore
https://github.com/yannforget/landsatxplore  
Library to search and download Landsat scenes from EarthExplorer (free of charge but account necessary). 

### snappy
https://senbox.atlassian.net/wiki/spaces/SNAP/pages/19300362/How+to+use+the+SNAP+API+from+Python  
SNAP API from Python. SNAP (SeNtinel’s Application Platform) is a platform to process and analyze Earth Observation data (Sentinel).
Its native API is implemented in Java.

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
