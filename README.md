### clone the repository  ###
clone the repository 

### open osgeo4w shell ###
open osgeo4w shell and cd into blackmarble parent directory

### gdal_translate ###
properly assign your file a projection...
```
gdal_translate -of VRT -a_srs EPSG:4326 -gcp 0 0 -180 90 -gcp 2048 0 180 90 -gcp 2048 1024 180 -90 land_shallow_topo_2048.tif blackmarble1.vrt
```

### gdalwarp  ###
warp the image for the cesium globe...
```
gdalwarp -of VRT -t_srs EPSG:4326 blackmarble1.vrt blackmarble2.vrt
```

### generate the tiles ###
generate the png tiles and the bounding and projection information...
```
gdal2tiles -p geodetic -z 1-5 blackmarble2.vrt blackmarble
```

### Add the Cesium createTileMapServiceImageryProvider code ###
Notice the following bit of code inside blackmarble/index.html: 
```
var layers = viewer.scene.imageryLayers;
var blackMarble = layers.addImageryProvider(new Cesium.createTileMapServiceImageryProvider({
    url : '.',
    maximumLevel : 8,
    baseLayerPicker: false,
    credit : 'Black Marble imagery courtesy NASA Earth Observatory'
}));
```


### Install the required modules ###

```
npm install
```

### Start the server ###

```
node server.js
```


### Enjoy the fruit of your pain and suffering ###

Visit [http://localhost:8080/blackmarble/](http://localhost:8080/blackmarble/).