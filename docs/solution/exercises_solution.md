# Own exercises in actinia - solution

### SMALL EXERCISES

#### 1. altitude of the highest point in North Carolina
* statisitcs of elevation map: https://actinia.mundialis.de/api/v1/locations/nc_spm_08/mapsets/PERMANENT/raster_layers/elev_state_500m
  ```
  curl -u "foss4g2021:ahZ7eeza" -X GET https://actinia.mundialis.de/api/v1/locations/nc_spm_08/mapsets/PERMANENT/raster_layers/elev_state_500m | jq
  ```
* answer: 1952.0009637576 m (max) zum Vergleich https://de.wikipedia.org/wiki/Mount_Mitchell 2037m

#### 2. zipcode in Wake county with the most hospitals

* vector layer:
  * zipcodes: https://actinia.mundialis.de/api/v1/locations/nc_spm_08/mapsets/PERMANENT/vector_layers/zipcodes_wake/render
  * hospitals: https://actinia.mundialis.de/api/v1/locations/nc_spm_08/mapsets/PERMANENT/vector_layers/hospitals/render

* PC: exercise02_hospitals.json
* ephemeral processing
  ```
  curl -u "foss4g2021:ahZ7eeza" -X POST https://actinia.mundialis.de/api/v1/locations/nc_spm_08/processing_async -H 'accept: application/json' -H 'Content-Type: application/json' -d @exercise02_hospitals.json | jq

  curl -u "foss4g2021:ahZ7eeza" -X GET https://actinia.mundialis.de/api/v1/resources/foss4g2021/resource_id-... | jq
  ```
* answer: 2 hospitals in "RALEIGH 27603"


#### 3. water bodies from the available Landsat imagery

* PC: exercise03_ndwi.json
* ephemeral processing with export
  ```
  curl -u "foss4g2021:ahZ7eeza" -X POST https://actinia.mundialis.de/api/v1/locations/nc_spm_08/processing_async_export -H 'accept: application/json' -H 'Content-Type: application/json' -d @exercise03_ndwi.json | jq

  curl -u "foss4g2021:ahZ7eeza" -X GET https://actinia.mundialis.de/api/v1/resources/foss4g2021/... | jq
  ```
* persistent processing with color setting
  ```
  curl -u "foss4g2021:ahZ7eeza" -X POST https://actinia.mundialis.de/api/v1/locations/nc_spm_08/mapsets/workshop_gr_aw/processing_async -H 'accept: application/json' -H 'Content-Type: application/json' -d @exercise03_ndwi.json | jq

  curl -u "foss4g2021:ahZ7eeza" -X GET https://actinia.mundialis.de/api/v1/resources/foss4g2021/... | jq
  ```
  * https://actinia.mundialis.de/api/v1/locations/nc_spm_08/mapsets/workshop_gr_aw/raster_layers/ndwi_1/render
  * https://actinia.mundialis.de/api/v1/locations/nc_spm_08/mapsets/workshop_gr_aw/raster_layers/water_bodies/render


### EXERCISE: "Population at risk near coastal areas"
* Data:
  * SRTM: https://actinia.mundialis.de/api/v1/locations/latlong_wgs84/mapsets/srtmgl1_30m/raster_layers/srtmgl1_v003_30m/render
  * Population: https://actinia.mundialis.de/api/v1/locations/latlong_wgs84/mapsets/worldpop_south_america/raster_layers/worldpop_2015_1km_aggregated_UNadj/render
  * coastline: https://actinia.mundialis.de/api/v1/locations/latlong_wgs84/mapsets/coastlines/raster_layers/ne_1000m_coastline/render
  ```
  # SRTM 30m
  curl -u "foss4g2021:ahZ7eeza" -X GET https://actinia.mundialis.de/api/v1/locations/latlong_wgs84/mapsets/srtmgl1_30m/raster_layers/srtmgl1_v003_30m
  # Population
  curl -u "foss4g2021:ahZ7eeza" -X GET https://actinia.mundialis.de/api/v1/locations/latlong_wgs84/mapsets/worldpop_south_america/raster_layers/worldpop_2015_1km_aggregated_UNadj | jq
  # coastline raster
  curl -u "foss4g2021:ahZ7eeza" -X GET https://actinia.mundialis.de/api/v1/locations/latlong_wgs84/mapsets/coastlines/raster_layers/ne_1000m_coastline | jq
  ```
* PC: exercise_pop.json
* persistent processing:
  ```
  curl -u "foss4g2021:ahZ7eeza" -X POST https://actinia.mundialis.de/api/v1/locations/latlong_wgs84/mapsets/workshop_gr_aw/processing -H 'accept: application/json' -H 'Content-Type: application/json' -d @exercise_pop.json | jq

  curl -u "foss4g2021:ahZ7eeza" -X GET https://actinia.mundialis.de/api/v1/resources/foss4g2021/resource_id-... | jq
  ```
* answer: 759932 people (sum)
  * https://actinia.mundialis.de/api/v1/locations/latlong_wgs84/mapsets/workshop_gr_aw/raster_layers/peop/render
