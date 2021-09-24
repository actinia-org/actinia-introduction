# Own exercises in actinia

<!--
(duration: 40 min)
-->

Meanwhile you have seen a lot of material. Time to try out some further exercises...

## EXERCISE: "Population at risk near coastal areas"

* needed geodata:
    * SRTM 30m (already available in actinia - find out the location yourself)
    * Global Population 2015 (already available in actinia - find out the location yourself)
    * vector shorelines (get from [naturalearthdata](http://www.naturalearthdata.com/downloads/))
* fetch metadata with actinia interface
* before doing any computations: what's important about projections?
* proposed workflow:
    * set computational region to a small subregion and constrain the pixel number through defined user settings
    * buffer SRTM land areas by 5000 m inwards
    * zonal statistics with population map

## EXERCISE: "Property risks from trees"

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
    * export of results through REST resources
