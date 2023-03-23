---
title: "Experience with Mapnik"
date: 2020-09-05
description: Experience with Mapnik
menu:
  sidebar:
    name: Experience with Mapnik
    identifier: mapnik
    weight: 6
tags: ["mapnik", "configuration"]
categories: ["map"]
---

For some reason, I needed to use mapnik to render the .osm file into tiles (png format), and I started my long journey with mapnik. The task is to render the text label layer only, without any background geographical features.


Things started off smoothly. I found python already have pip installation for mapnik so I simply typed `pip install mapnik` to install it on my machine. I followed the tutorial [here](https://github.com/mapnik/mapnik/wiki/GettingStartedInPython) and successfully reproduced the output. 

{{< figure src="/posts/mapnik/imgs/step1.webp" title="step1"  width="700" align="center" >}}

&nbsp;

Then I tried zooming into the Britain region and here's what it looks like...Hmm, not adding any new content, but just a enlarged version of the same picture.

{{< figure src="/posts/mapnik/imgs/step2.webp" title="step2"  width="700" align="center" >}}

&nbsp;
I also tried out with [this](https://wiki.openstreetmap.org/wiki/Mapnik/Rendering_OSM_XML_data_directly), although it is out-dated, but I still (kind of) managed to have it work. The catch is that fill should be "#000000" instead of fill="#000". With "#000", the text wouldn't show up because it is a wrong color code. I was not able to have it display the real name and it is showing "name" as a placeholder.

{{< figure src="/posts/mapnik/imgs/step3.webp" title="step3"  width="700" align="center" >}}

&nbsp;

The good time didn't last long. The real suffer just began. Because the above post said it's out-dated, then I started wondering what the current method is. Turns out it's Carto. We can use Carto to generate style-sheets (xml format) and feed into mapnik for rasterization.


When I use carto generated style-sheets, it reports error of missing fonts and unrecognized attributes such as following.

```
Mapnik LOG> 2019-09-25 16:40:59: Unable to process some data while parsing 'src/openstreetmap-carto/zekun_mapnik.xml':

* attribute 'maximum-scale-denominator' with value '25000000' at line 0

* attribute 'minimum-scale-denominator' with value '750000' at line 0

* attribute 'maximum-scale-denominator' with value '750000' at line 0

* attribute 'maximum-scale-denominator' with value '50000' at line 0

* attribute 'maximum-scale-denominator' with value '50000' at line 0

* attribute 'maximum-scale-denominator' with value '50000' at line 0
```

I googled [a little bit](https://github.com/mapnik/python-mapnik/issues/132) and realized it is because mapnik version is too old. The first thing I did was replace the 'maximum-scale-denominator' with 'maxzoom', similarly for 'mininum-scale-denominator'. The error disappeared but the result image is just blank.

The second option is to upgrade mapnik to at least v3.0.22. I had to uninstall my mapnik from pip, download the [mapnik](https://github.com/mapnik/mapnik) and [python-mapnik](https://github.com/mapnik/python-mapnik) then compile from scratch. 

But this step is not easy. mapnik installation test was fine but python-mapnik test keeps failing every time. 

```
Traceback (most recent call last):

  File "render3.py", line 3, in <module>

    import mapnik

  File "/usr/local/lib/python2.7/dist-packages/mapnik-4.0.0-py2.7-linux-x86_64.egg/mapnik/__init__.py", line 1071, in <module>

    register_plugins()

  File "/usr/local/lib/python2.7/dist-packages/mapnik-4.0.0-py2.7-linux-x86_64.egg/mapnik/__init__.py", line 1053, in register_plugins

    DatasourceCache.register_datasources(path)

Boost.Python.ArgumentError: Python argument types in

    DatasourceCache.register_datasources(str)

did not match C++ signature:

    register_datasources(std::string)
```

I kept having trouble with mapnik and python-mapnik because they are not compatible with each other. I uninstalled and reinstalled the packages for about 20 times and still didn't solve the issue.

In the end, I decided switch to [docker installation](https://github.com/akx/docker-mapnik) and was able to make it work. But apparently, this docker still doesn't have the latest version either, and I was back to the same problem again. 


Sigh....


But when I was googling, I came across [this](https://help.openstreetmap.org/questions/54587/how-to-use-xml-file-generated-by-openstreetmap-carto-with-mapnik-and-postgis) question, seems like that person had the same problem as mine. So gave it a try and added the following four lines to the code to convert the coordinate system of the bounding box. 

```
prj = mapnik.Projection("+init=epsg:3857")

wgs84 = mapnik.Projection('+init=epsg:4326')

tr = mapnik.ProjTransform(wgs84, prj)

bbox = tr.forward(bbox)
```

Surprisingly, this time something showed up.

{{< figure src="/posts/mapnik/imgs/step4.webp" title="step4"  width="700" align="center" >}}

&nbsp;

I finally got a piece of code to start with. It shows the Birmingham region that I downloaded. 


It is only showing one layer because I commented out all the other layers in project.mml when run `carto project.mml mapnik.xml`. When I added all layers back (except for those require additional shp file), then adjusted the bbox range and image size, I finally got something decent. 



{{< figure src="/posts/mapnik/imgs/step5.webp" title="step5"  width="700" align="center" >}}

&nbsp;

and 

{{< figure src="/posts/mapnik/imgs/step6.webp" title="step6"  width="700" align="center" >}}

&nbsp;

Anyways, bon voyage.
