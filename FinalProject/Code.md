## Main Code (in Python)

``` Python
import pandas as pd
import numpy as np
from tensorflow import keras
from google.colab import files

uploaded = files.upload()

# From the gpx file
track_points = pd.read_csv('track_points.csv')

columns_to_keep = ['X','Y','time']
track_points = track_points[columns_to_keep]
track_points['time'] = track_points['time'].str.slice(0,-7)
track_points

# Download skmob
!apt-get install -qq curl g++ make
!curl -L http://download.osgeo.org/libspatialindex/spatialindex-src-1.8.5.tar.gz | tar xz
import os
os.chdir('spatialindex-src-1.8.5')
!./configure
!make
!make install
!pip install rtree
!ldconfig
!pip install scikit-mobility

import skmob

# Create a TrajDataFrame
tdf = skmob.TrajDataFrame(track_points)
tdf.columns = ['lng','lat','datetime']
tdf['uid'] = 1

tdf.plot_trajectory(zoom=12, weight=3, opacity=0.9, tiles='Stamen Toner')

from skmob.preprocessing import filtering
# filter out all points with a speed (in km/h) from the previous point higher than 500 km/h
ftdf = filtering.filter(tdf, max_speed_kmh=500.)

n_deleted_points = len(tdf) - len(ftdf) # number of deleted points
print(n_deleted_points)

from skmob.preprocessing import detection
# compute the stops for each individual in the TrajDataFrame
stdf = detection.stops(tdf, stop_radius_factor=0.5, minutes_for_a_stop=20.0, spatial_radius_km=0.2, leaving_time=True)

print('Points of the original trajectory:\t%s'%len(tdf))
print('Points of stops:\t\t\t%s'%len(stdf))

from skmob.measures.individual import jump_lengths, radius_of_gyration, home_location

# compute the radius of gyration for each individual
rg_df = radius_of_gyration(tdf)

# compute the jump lengths for each individual
jl_df = jump_lengths(tdf.sort_values(by='datetime'))

# compute the home location for each individual
hl_df = home_location(tdf)

import folium
from folium.plugins import HeatMap
m = folium.Map(tiles = 'openstreetmap', zoom_start=12, control_scale=True)
HeatMap(hl_df[['lat', 'lng']].values).add_to(m)
m

from skmob.models.epr import DensityEPR
# load a spatial tesellation on which to perform the simulation
tessellation = gpd.read_file('jamescity.shp')
# starting and end times of the simulation
start_time = pd.to_datetime('2021/05/19 09:00:00')
end_time = pd.to_datetime('2021/05/23 23:00:00')
# instantiate a DensityEPR object
depr = DensityEPR()
# start the simulation
tdf = depr.generate(start_time, end_time, tessellation, relevance_column='population', n_agents=20)

tdf.plot_trajectory(zoom=12, weight=3, opacity=0.9, tiles='Stamen Toner')

```


## Code to Generate Shapefiles of James City (in R)

```R
rm(list=ls(all=TRUE))

install.packages("tidyverse", dependencies = TRUE)
install.packages("tidymodels")
install.packages("randomForest", dependencies = TRUE)
install.packages("vip")
install.packages("sf", dependencies = TRUE)
install.packages("raster", dependencies = TRUE)
install.packages("exactextractr", dependencies = TRUE)
install.packages("rmapshaper", dependencies = TRUE)
install.packages("rgeos", dependencies = TRUE)
install.packages("rgdal", dependencies = TRUE)
install.packages("mapsRinteractive", dependencies = TRUE)
install.packages("spatstat", dependencies = TRUE)

# install.packages("maptools", dependencies = TRUE)
install.packages("exactextractr")
install.packages("geojsonsf")



library(tidyverse)
library(tidymodels)
library(sf)
library(raster)
library(exactextractr)
library(rmapshaper)
library(rgeos)
library(rgdal)
library(mapsRinteractive)
library(spatstat)
library(maptools)

library(exactextractr)
library(geojsonsf)


setwd("~/Desktop/frank_final")

### Import Administrative Boundaries ###

usa_adm0 <- read_sf("gadm36_USA_0.shp")
usa_adm1 <- read_sf("gadm36_USA_1.shp")
usa_adm1 <- read_sf("gadm36_USA_2.shp")

usa_adm0 <- ms_simplify(usa_adm0)
usa_adm1 <- ms_simplify(usa_adm1)
usa_adm1 <- ms_simplify(usa_adm1)

# not sure which package/library this needs
usa_adm0 <- st_as_sf(usa_adm0)
usa_adm1 <- st_as_sf(usa_adm1)
usa_adm1 <- st_as_sf(usa_adm1)

james_city <- usa_adm1 %>%
  filter(NAME_2 == "James City")

ggplot() +
  geom_sf(data = jor_adm2) +
  geom_sf(data = sahab, aes(color = "red"))

### Import WorldPop ppp raster ###

usa_pop20 <- raster("usa_ppp_2020.tif")

jamescity_pop20 <- crop(usa_pop20, james_city)
jamescity_pop20 <- mask(jamescity_pop20, jamescity_pop20)

writeRaster(jamescity_pop20, "jamescity_ppp_2020.tif")

plot(jamescity_pop20)
plot(st_geometry(james_city), add = TRUE)

jamescity_pop20 <- crop(jamescity_pop20, james_city)
jamescity_pop20 <- mask(jamescity_pop20, james_city)
jamescity_pop20[is.na(jamescity_pop20)] <- 0 #this fixes NA
jamescity_pop20 <- crop(jamescity_pop20, james_city)
jamescity_pop20 <- mask(jamescity_pop20, james_city)

plot(jamescity_pop20)
plot(st_geometry(james_city), add = TRUE)

pop <- floor(cellStats(jamescity_pop20, 'sum'))

st_write(james_city, "james_city.shp", delete_dsn=TRUE)
james_city_mt <- readShapeSpatial("james_city.shp")
win <- as(james_city_mt, "owin")

# james_city_ppl <- rpoint(pop, f = as.im(jamescity_pop20), win = win)
james_city_ppl100 <- rpoint(1000, f = as.im(jamescity_pop20), win = win)

# plot(win, main = NULL)
# plot(james_city_ppl100, cex = .15)

### make voronoi polygons ###

mk_bb <- st_bbox(james_city) %>% st_as_sfc() #getting bounding box and turning into a poly

urban_centroids <- st_as_sf(james_city_ppl100)[-1,]

st_crs(urban_centroids) <- st_crs(james_city)

urban_centroids <-  urban_centroids %>% 
  st_cast("MULTIPOINT")

mk_voronoi <- st_voronoi(st_union(urban_centroids), mk_bb) #producing voronoi polys

james_city_voronoi <- ms_clip(mk_voronoi, james_city) %>% 
  st_as_sf()

ggplot() +
  geom_sf(data = james_city_voronoi)

plot(jamescity_pop20)
plot(st_geometry(james_city_voronoi), add = TRUE)

jamescity_pops <- exact_extract(jamescity_pop20, james_city_voronoi, fun=c('sum'))

james_city_voronoi <- bind_cols(james_city_voronoi, population = jamescity_pops)

ggplot() +
  geom_sf(data = james_city_voronoi, aes(fill = population), size = 0)
  
st_write(james_city_voronoi, "jamescity.shp")

```
