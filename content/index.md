---
date: 2016-08-30T00:11:02+01:00
title: Welcome to Tegola
type: index
weight: 0
---

Tegola is a high performance Mapbox Vector Tile server written in [Go](https://golang.org). In a nutshell, Tegola takes geospatial vector data and slices it into vector tiles that can be styled on the client. For example, Tegola is responsible for generating all the map tiles in the following map:


{{< map >}}


## Features

- Simple to setup. All you need is the Tegola binary and a config file.
- Extensible. Tegola is designed to support multiple data providers. Currently supports PostGIS.