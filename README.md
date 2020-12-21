# Spatiotemporal Fusion for Planetscope(PS) and Mapbox tiles 


#### Objective

Objective is to use PS image to predict Mapbox data to current date. This would help prevent false negatives in the model prediction of Schools/hospitals.


#### Queries/Assumptions

- Temporal difference affects fused images and inturn the model predictions. 

- PS(4.77m) & Mapbox Tiles (~0.5m pixel spacing). Course nature of PS doesn’t affect the features in the fused image. 

 
#### Tasks at hand 

- Evaluate Automated coregistration (Geofolki?) 

- Normalise both PS and Mapbox and resample PS to 0.5m 

 
#### Methodology

1) Upon NICFI signup; Planetscope data can be obtained in bulk from the following script.[https://github.com/geospoc/unc-gis-planet-download](https://github.com/geospoc/unc-gis-planet-download) 

2) Mapbox georeferenced mosaic can be obtained from MOBAC. Mapbox tiles too can be obtained from the API but it would be available without Georeference.  

3) Both Planet and Mapbox images are co-registered, resampled to the mapbox pixel spacing and normalized to fulfill requirement of the fusion algorithms. PS can be converted to Radiance or reflectance while the same isn’t possible for the Mapbox Satellite basemap. So we normalize data between [0-255] range. Bicubic interpolation to upsample PS images to the size of Mapbox satellite basemap(0.5m). 

4) Implement spatial and temporal adaptive reflectance fusion model (STARFM) algorithm to fuse Mapbox and Planet images. (Reference - Kwan, Chiman, et al. "Assessment of spatiotemporal fusion algorithms for planet and worldview images." Sensors 18.4 (2018): 1051.) .  

5) The following changes are made in (4) inorder to suit our project requirement.  

Data shortcoming in the fusion approach: If “p” is prediction date, “o” is observation date, M is mapbox basemap & P is Planetscope data; consider the fusion equation 

                                                             ''' Mp = Mo  +[Pp-Po] ''' 

for which Mo & Pp is available but Po i.e. the planetscope data for the observation date is unavailable. Planetscope archive is available (for a premium) from 2009 while mapbox basemaps at places date back to 2002. To overcome this we have requested you 2002-2007 Worldview samples on which we intend to apply a filter and bring it to PS equivalent resolution and use it as Po in the Fusion algorithm. 

6) Finally, the predicted fusion image would be used to detect school(s). 

 
