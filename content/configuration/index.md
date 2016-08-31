---
date: 2016-08-30T00:11:02+01:00
title: Configuration
weight: 20
---

## Overview
To use Tegola, you only need two things: Tegola and a Tegola config file. The Tegola config file use [TOML](https://github.com/toml-lang/toml) syntax and is comprised of three primary sections:


- [Webserver](#webserver): webserver configuration.
- [Providers](#providers): data provider configuration (i.e. PostGIS).
- [Maps](#maps): map configuration including map names, layers and zoom levels.


## Webserver

The webserver part of the config has the following parameters:

| Param    | Required |  Default | Description                                        |
|----------|:--------:|:--------:|---------------------------------------------------:|
| port     | No       | :8080    | A string with the value for port.                  |
| log-file | No       |          | Location of a log file to write webserver logs to. |

Example config

```toml
[webserver]
port = ":8080"
```

## Providers

The providers configuration tells Tegola where your data lives. Currently Tegola supports PostGIS as a data provider, but it's positioned to support additional data providers. Data providers each have their own specific configuration, but all are required to have the following two config params:

| Param    | Description                                                                                |
|----------|-------------------------------------------------------------------------------------------:|
| name     | User defined data provider name. This is used by map layers to reference the data provider.|
| type     | The type of data provider.                                                                  |


### PostGIS

In addition to the required `name` and `type` parameters, a PostGIS data provider has the following additional params:

| Param    | Required |  Default | Description                                     |
|----------|:--------:|:--------:|------------------------------------------------:|
| host     | Yes      |          | The database host.                              |
| port     | No       | 5432     | The port the database is listening on.          |
| database | Yes      |          | The name of the database                        |
| user     | Yes      |          | The database user                               |
| password | Yes      |          | The database user's password                    |
| srid     | No       | 3857     | The default SRID for this data provider         |

#### Example PostGIS config

```toml
[[providers]]
name = "test_postgis"   # provider name is referenced from map layers
type = "postgis"        # the type of data provider. currently only supports postgis
host = "localhost"      # postgis database host
port = 5432             # postgis database port
database = "tegola"     # postgis database name
user = "tegola"         # postgis database user
password = ""           # postgis database password
srid = 3857             # The default srid for this provider. If not provided it will be WebMercator (3857)
```

### Provider layers

```toml
[[providers.layers]]
name = "landuse"                    # will be encoded as the layer name in the tile
tablename = "gis.zoning_base_3857"  # sql or table_name are required
geometry_fieldname = "geom"         # geom field. default is geom
id_fieldname = "gid"                # geom id field. default is gid
srid = 4326                         # the srid of table's geo data.
```

## Maps

```toml
[[maps]]
name = "zoning"		# used in the URL to reference this map (/maps/:map_name)
```

### Map layers

```toml
[[maps.layers]]
provider_layer = "test_postgis.landuse" # must match a data provider layer
min_zoom = 12                       	# minimum zoom level to include this layer
max_zoom = 16                       	# maximum zoom level to include this layer
```

### Map layers default tags

```toml
[maps.layers.default_tags]      
class = "park"			# a default tag to encode into the feature
```