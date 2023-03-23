---
title: "Display JPEG historical map tiles in Mapbox"
date: 2021-01-02
description: Display JPEG historical map tiles in Mapbox
menu:
  sidebar:
    name: Display JPEG historical map tiles in Mapbox
    identifier: mapbox
    weight: 5
tags: ["mapbox", "tiles"]
categories: ["map"]
---

Mapbox is a great tool for visualizing maps especially when you have multiple layers and would like to compare them at the same location.


In my case, I have millions of small tiles in jpeg format, and I would like to overlay those tiles on OSM map. Those map tiles are generated from a neural network by taking OSM tiles as input and the resulting tiles are close to historical style. Thus it is interesting for me to compare the OSM map and the synthetic historical map to check the quality of the network output.

{{< figure src="/posts/mapbox/imgs/osm-syn.png" title="OSM and historical " >}}

&nbsp;

#### Create tiff file and upload to Mapbox

We need to create a mapbox account which can be registered [here](https://account.mapbox.com/). With mapbox we can specify different base-map (theme) and you can choose the one you like the best. A very nice tutorial about Mapbox (as well as carto) is [here](https://slides.com/arutkows/mapping-with-carto-and-mapbox-11#/6/9) by Andy Rutkowski. You could explore mapbox a little bit by following these slides. 


Mapbox could read **GeoTiff** files as **Tileset** and load them as layers.The easiest way for us is to convert the JPEG images into GeoTiff by adding the lattitude/longitude information, i.e. geo-referencing, then concatenate those small tiff files into one big tiff. Since my historical tiles share the same geo-coordinates as the OSM counterpart, it's relatively easy to generate a `.wrd` and `.point` file and use them to produce Tiff files by using the code [here](https://github.com/spatial-computing/historical_map_retrieval). We just need to do the Step 2 and Step 3 as in the ReadMe to get the Tiff files as output.

At this step, we've successfully geo-referencing the JPEG files and get a bunch of Tiff files. We could visualize the Tiff files in QGIS by loading them as layers. But if you prefer to be able to use it as a demo and save it online using Mapbox, we need to do something extra.

Fortunately this `extra` is not too much. We simply run the following command to concatenate those small tiff files into one big tiff file and name it `mosaic.tiff`.

```
gdalbuildvrt mosaic.vrt ?.tif

gdal_translate -of GTiff -co "COMPRESS=JPEG" -co "PHOTOMETRIC=YCBCR" -co "TILED=YES" -co "BIGTIFF=YES" mosaic.vrt mosaic.tif
```

#### Display in Mapbox

Then in the mapnik, follow the steps below:


{{< figure src="/posts/mapbox/imgs/step1.webp" title="step1"  width="700" align="center" >}}

&nbsp;

{{< figure src="/posts/mapbox/imgs/step2.webp" title="step2"  width="700" align="center" >}}

&nbsp;

{{< figure src="/posts/mapbox/imgs/step3.webp" title="step3"  width="700" align="center" >}}

&nbsp;

{{< figure src="/posts/mapbox/imgs/step4.webp" title="step4"  width="700" align="center" >}}

&nbsp;

Then the tile would successfully appear in Mapbox.

{{< figure src="/posts/mapbox/imgs/step5.webp" title="step5"  width="700" align="center" >}}

&nbsp;

<mark>Note: </mark>

It is possible that you might need to convert the GeoTiff from PCT to RGB format by running the command bellow:

```
pct2rgb.py -of GTiff osm_32331_21348_16.tif a.tif

```




