# A gentle introduction to actinia

Author: Markus Neteler, mundialis GmbH & Co. KG, Bonn

<!-- **** Begin Fork-Me-On-Gitlab-Ribbon-HTML. See MIT License at https://gitlab.com/seanwasere/fork-me-on-gitlab **** -->
<a href="https://github.com/mmacata/actinia-introduction/">
    <span style="font-family: tahoma; font-size: 18px; position:fixed; top:50px; right:-45px; display:block; -webkit-transform: rotate(45deg); -moz-transform: rotate(45deg); background-color:green; color:white; padding: 4px 30px 4px 30px; z-index:99; opacity:0.6">Fork Me On GitHub</span>
</a>
<!-- **** End Fork-Me-On-Gitlab-Ribbon-HTML **** -->


URL of this dcument: [https://mmacata.github.io/actinia-introduction/](https://mmacata.github.io/actinia-introduction/)

This workshop is a fork of https://neteler.gitlab.io/actinia-introduction. The initial workshop has a more detailed chapter about ace - the actinia command execution. This workshop focuses more on the "bare" HTTP API from actinia.

*Last update: 21 Sep 2021*

## Abstract

<img src="img/actinia_logo.svg" width="30%" align="right">

Actinia ([https://actinia.mundialis.de/](https://actinia.mundialis.de/)) is an open source REST API for scalable, distributed, high performance processing of geographical data that uses mainly GRASS GIS for computational tasks. Core functionality includes the processing of single scenes and time series of satellite images, of raster and vector data. With the existing (e.g. Landsat) and Copernicus Sentinel big geodata pools which are growing day by day, actinia is designed to follow the paradigm of bringing algorithms to the cloud stored geodata. Actinia is an OSGeo Community Project since 2019.

In this course we will briefly give a short introduction to REST API and cloud processing concepts. This is followed by an introduction to actinia processing along with hands-on to get more familiar with the topic by means of exercises.

[![DOI](https://zenodo.org/badge/DOI/10.5281/zenodo.2631917.svg)](https://doi.org/10.5281/zenodo.2631917)

## Required software for this tutorial

We will use a command line tool or browser plugin to try out some REST commands. Then we'll also use GRASS GIS and a special command to control actinia from remote. This requires some software to be installed:

* REST client (command line tool or browser plugin):
    * [cURL](https://curl.haxx.se/docs/manpage.html), to be used on command line
    * Extensions for Chrome/Chromium browser:
        * [RESTman extension](https://chrome.google.com/webstore/detail/restman/ihgpcfpkpmdcghlnaofdmjkoemnlijdi)
        * and a nice [JSON Formatter](https://chrome.google.com/webstore/detail/json-formatter/bcjindcccaagfpapjjmafapmmgkkhgoa)
* For the "ace - actinia command execution" section:
    * GRASS GIS 7.8+ ([download](https://grass.osgeo.org/download/))
    * three additional Python packages:
        * Linux: `pip3 install click requests simplejson`
        * Windows users (Installer: [OSGeo4W](https://trac.osgeo.org/osgeo4w/) > Advanced installation > Search window):
            * python3-click, python3-requests, python3-simplejson
    * ace - [actinia command execution](https://github.com/mundialis/actinia_core/blob/master/scripts/README.md) (to be run from a GRASS GIS session; installation shown below)
    * [jq, a lightweight and flexible command-line JSON processor](https://stedolan.github.io/jq/download/)
    * nice to have: <a href="https://www.qgis.org/en/site/forusers/download.html">QGIS</a>

<center>
<a href="img/osgeo4w_python_libs.png"><img src="img/osgeo4w_python_libs.png" width="30%"></a> &nbsp; &nbsp;
<a href="img/osgeo4w_grass78.png"><img src="img/osgeo4w_grass78.png" width="30%"></a>
</center>

Note: We will use the demo actinia server at [https://actinia.mundialis.de/](https://actinia.mundialis.de/) - hence Internet connection is required.

## actinia tutorial overview

**Content**

* Warming up
* Introduction
    * Why cloud computing?
    * Overview actinia
* REST API and geoprocessing basics
    * What is REST: intro
* First Hand-on: working with REST API requests
    * Step by step...
* Exploring the API: finding available actinia endpoints
    * REST actinia examples with curl
* Controlling actinia from a running GRASS GIS session
    * Further command line exercise suggestions
* Own exercises in actinia
* Conclusions and future
* See also: openEO resources
* References
* About the trainer

## Warming up

<!--
(duration: 20 min)
-->

To make you familiar with a few concepts, let's take a look at the "graphical intro to actinia" - [GRASS GIS in the cloud: actinia geoprocessing](https://mundialis.github.io/foss4g2019/grass-gis-in-the-cloud-actinia-geoprocessing/index.html) (note: it requires the Chrome/ium browser; presentation provided by <a href="https://github.com/mmacata/">Carmen Tawalika</a>, mundialis).

<center>
<img src="img/actinia_intro.png" width="60%">
</center>
