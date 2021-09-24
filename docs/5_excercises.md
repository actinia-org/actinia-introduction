# Own exercises in actinia

<!--
(duration: 40 min)
-->

Meanwhile you have seen a lot of material. Time to try out some further exercises...

### SMALL EXERCISES

1. What is the altitude of the highest point in North Carolina? Check it with actinia.
  * Find the correct raster in the North Carolina location and PERMANENT mapset.
  * Find the relevant raster layer by rendering it
  * Print the information and get altitude of the highest point

2. Find the zipcode in Wake county with the most hospitals
  * Find the relevant vector layers
  * Check the zipcode vector layer for the relevant column to get the zipcode
  * Create a process chain as a .json file to ask for the number of hospitals in the zipcodes: Use the GRASS GIS modules `g.copy` (because you are not allowed to change data from an other mapset), `v.vect.stats` and `v.db.select`
  * Post the created process chain to `https://actinia.mundialis.de/api/v1/locations/nc_spm_08/processing_async` for ephemeral processing

3. Export the water bodies from the available Landsat imagery of North Carolina
  * Create a process chain as a .json file
    * Remember to set the computational region
    * Compute the NDWI (Normalized difference water index); use `r.mapcalc` or `i.vi`
    * Filter water bodies by a threshold of e.g. 0.35 using `r.mapcalc`
  * Either export the water bodies (use the `exporter` with the ephemeral processing) or render the maps of NDWI and water bodies with a nice color (use `r.colors` and persistent processing in your own mapset)

### EXERCISE: "Population at risk near coastal areas"

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
