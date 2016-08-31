---
date: 2016-03-09T00:11:02+01:00
title: Getting started
weight: 10
---

## 1. Download Tegola
Download the [latest version](https://github.com/terranodo/tegola/releases).

Choose the binary that matches the operating system Tegola will run on. Quick links are available below for your convenience:

- [OSX](https://github.com/terranodo/tegola/releases/download/v0.2.0/tegola_darwin_amd64)
- [Windows](https://github.com/terranodo/tegola/releases/download/v0.2.0/tegola_windows_amd64.exe)
- [Linux](https://github.com/terranodo/tegola/releases/download/v0.2.0/tegola_linux_amd64)

Find the Tegola file that was downloaded and move it into a fresh directory.

## 2. Get a data provider

Tegola needs geospatial data to run. Currently, the only supported data format is PostGIS which is a format on top of PostgreSQL. If you do not have it installed, [download PostGIS](http://postgis.net/install/)

Next, you'll need a source of data. For your convenience we've provided a download of sample PostGIS data [here](www.link.com).

## 3. Create a configuration file

Tegola utilizes a single configuration file to control it's actions and coordinate with data source(s). This configuration file is written in TOML format. If you're unfamiliar with TOML, you can find documentation [here](https://github.com/toml-lang/toml).

Create your configuration file with any text editor and name it `config.toml`. Next, copy and paste the following into this configuration file:

```toml

[webserver]
port = ":9090"

# register data providers
[[providers]]
name = "bonn"           # provider name is referenced from map layers
type = "postgis"        # the type of data provider. currently only supports postgis
host = "localhost"      # postgis database host
port = 5432             # postgis database port
database = "tegola"     # postgis database name
user = "tegola"         # postgis database user
password = ""           # postgis database password
srid = 3857             # The default srid for this provider. If not provided it will be WebMercator (3857)

```

## 4. Create an HTML page

Tegola delivers geospatial vector tile data to any requesting client. For simplicity, we'll be setting up a basic HTML page as our client that will display the rendered map. We'll be using the [Open Layers](http://openlayers.org/) client side library to display and style the vector tile content.

Create a new file, copy in the contents below, and open in a browser:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>Mapbox Vector Tiles</title>
    <link rel="stylesheet" href="http://openlayers.org/en/v3.16.0/css/ol.css" type="text/css">
    <script src="http://openlayers.org/en/v3.16.0/build/ol.js"></script>
    <style>
      .map {
        width: 100%;
        height: 100%;
        position: absolute;
        background: #f8f4f0;
      }
    </style>
  </head>
  <body>
    <div id="map" class="map"></div>
      <script>
        var map = new ol.Map({
          layers: [
            new ol.layer.VectorTile({
              source: new ol.source.VectorTile({
                attributions: '© <a href="https://www.mapbox.com/map-feedback/">Mapbox</a> ' +
                '© <a href="http://www.openstreetmap.org/copyright">' +
                'OpenStreetMap contributors</a>',
                format: new ol.format.MVT(),
                tileGrid: ol.tilegrid.createXYZ({maxZoom: 22}),
                tilePixelRatio: 16,
                url:'/maps/zoning/{z}/{x}/{y}.vector.pbf?debug=true'
              })
            })
          ],
          target: 'map',
          view: new ol.View({
            center: [790793.4954921771, 6574927.869849075], //coordinates the map will center on initially
            zoom: 14
          })
        });
    </script>
  </body>
</html>
```

If everything was successful, you should see a map of Bonn in your browser. 