# Own exercises in actinia

<!--
(duration: up to 2h)
-->

Meanwhile you have seen a lot of material. Time to try out some further exercises...


### 1. What is the altitude of the highest point in North Carolina? Check it with actinia.

  * Find the correct raster in the North Carolina location and PERMANENT mapset.
  * Find the relevant raster layer by rendering it
  * Print the information and get altitude of the highest point
  * [List of available data](https://www.grassbook.org/wp-content/uploads/grasslocations/nc_spm_08_contents.html) in the North Carolina sample dataset.

### 2. Find the zipcode in Wake county with the most hospitals

  * Find the relevant vector layers
  * Check the zipcode vector layer for the relevant column to get the zipcode
  * Create a process chain as a .json file to ask for the number of hospitals in the zipcodes: Use the GRASS GIS modules `g.copy` (because you are not allowed to change data from an other mapset), `v.vect.stats` and `v.db.select`
  * Post the created process chain to `https://actinia.mundialis.de/api/v3/locations/nc_spm_08/processing_async` for ephemeral processing
  * Related GRASS GIS manual pages:
      [g.copy](https://grass.osgeo.org/grass-stable/manuals/g.copy.html),
      [v.vect.stats](https://grass.osgeo.org/grass-stable/manuals/v.vect.stats.html),
      [v.db.select](https://grass.osgeo.org/grass-stable/manuals/v.db.select.html).

### 3. Export the water bodies from the available Landsat imagery of North Carolina

  * Create a process chain as a .json file
    * Remember to set the computational region
    * Compute the NDWI (Normalized difference water index); use `r.mapcalc` or `i.vi`
    * Filter water bodies by a threshold of e.g. 0.35 using `r.mapcalc`
  * Either export the water bodies (use the `exporter` with the ephemeral processing) or render the maps of NDWI and water bodies with a nice color (use `r.colors` and persistent processing in your own mapset)
  * Related GRASS GIS manual pages:
      [r.mapcalc](https://grass.osgeo.org/grass-stable/manuals/r.mapcalc.html),
      [i.vi](https://grass.osgeo.org/grass-stable/manuals/i.vi.html),
      [exporter](https://github.com/mundialis/exporter/).

### 4. Population at risk near coastal areas

  * needed geodata:
    * Worldwide SRTM 30m (already available in actinia as `srtmgl1_v003_30m` - find out the location yourself)
    * South America Population 2015 (already available in actinia as `worldpop_2015_1km_aggregated_UNadj`- find out the location yourself)
    * raster shorelines (already available in actinia as `ne_1000m_coastlines`- find out the location yourself)
  * fetch metadata with actinia interface and render input data
  * proposed workflow:
    * set computational region to a small subregion (hint: `align` the region resolution to the population raster) and check the pixel number against user constraints
    * buffer the coastlines by 5000 m and set a mask to the result
    * Extract only the peopulation below 10 m
    * Calculate the statistic to get the population at risk near coastal areas
  * Hints for example GRASS modules to use in process chain: `g.region`, `r.buffer`, `r.mapcalc`, `r.mask`, `r.univar`
  * Related GRASS GIS manual pages:
      [g.region](https://grass.osgeo.org/grass-stable/manuals/g.region.html),
      [r.buffer](https://grass.osgeo.org/grass-stable/manuals/r.buffer),
      [r.mask](https://grass.osgeo.org/grass-stable/manuals/r.mask),
      [r.univar](https://grass.osgeo.org/grass-stable/manuals/r.univar.html).

<!-- ### EXERCISE: "Property risks from trees"

(draft idea only, submit your suggestion to trainer how to solve this task)

* define your region of interest
* needed geodata:
    * building footprints
        * download from OSM (via [https://overpass-turbo.eu/](https://overpass-turbo.eu/) | Wizard > building > ok > Export > Geojson)
        * these data are now on your machine and not on the actinia server
        * use "ace importer" or cURL to upload
    * select Sentinel-2 scene
* proposed workflow:
    * actinia "ace" importer for building footprint upload
    * `v.buffer` of 10m and 30m around footprints
    * select S2 scene, compute NDVI with `i.vi`
    * filter NDVI threshold > 0.6 (map algebra) to get the tree pixels - more exiting would be a ML approach (with previously prepared training data ;-)) (`r.learn.ml` offers RF and SVM)
    * on binary tree map (which corresponds to risk exposure)
    * count number of tree pixels in 5x5 moving window (`r.neighbors` with method "count")
    * compute property risk statistics using buffers and tree count map and upload to buffered building map (`v.rast.stats`, method=maximum)
    * export of results through REST resources -->
