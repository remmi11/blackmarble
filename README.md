### clone the repository  ###
clone the repository 

### open osgeo4w shell ###
open osgeo4w shell and cd into bluemarble parent directory

### gdal_translate ###
Instead of using EPSG:4326 we need to use the EPSG:3857 mercator projection...
```
gdal_translate -of VRT -a_srs EPSG:3857 -gcp 0 0 -180 90 -gcp 21600 0 180 90 -gcp 21600 10800 180 -90 land_shallow_topo_21600.tif bluemarble1.vrt
```

### gdalwarp  ###
warp the image for the cesium globe
```
gdalwarp -of VRT -t_srs EPSG:3857 bluemarble1.vrt bluemarble2.vrt
```

### generate the tiles ###
generate the tiles and save to bluemarble directory
```
gdal2tiles -z 1-5 bluemarble2.vrt bluemarble 
```

### Add the Cesium createTileMapServiceImageryProvider code ###
Notice the following bit of code inside bluemarble/index.html: 
```
var layers = viewer.scene.imageryLayers;
var blackMarble = layers.addImageryProvider(new Cesium.createTileMapServiceImageryProvider({
    url : '.',
    baseLayerPicker: false,
    maximumLevel : 5,
    credit : 'Black Marble imagery courtesy NASA Earth Observatory'
}));
```


### Download Node ###

Visit the [Downloads page](http://cesiumjs.org/downloads.html) or use the npm module:


### Install the required modules ###

```
npm install
```

### Start the server ###

```
node server.js
```