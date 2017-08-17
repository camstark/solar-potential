#### Calgary Solar Potential Images

1. Download the TIFF images from [data.calgary.ca](https://data.calgary.ca/Environment/Solar-Potential-images-tiff-/yius-inbn)

2. Unzip Solar_Potential_TIFFS.zip to a folder location: [/data](/data)

3. Ensure GDAL is installed and combine all the .tifs in one file:
`gdal_merge.py -o combined.tif *.tif`

4. Check spatial reference to confirm -s_srs for next command:
`gdalinfo combined.tif`

5. Then [warp to Google Web Mercator for TileMill](https://tilemill-project.github.io/tilemill/docs/guides/reprojecting-geotiff/):
~~~~
gdalwarp -0 -s_srs EPSG:4326 -t_srs EPSG:3857 -r bilinear \
   combined.tif warped.tif
~~~~

6. With TileMill installed, import `warped.tif` in to TileMill with SRS `900913` and Advanced `band=1` ([like this](https://tilemill-project.github.io/tilemill/docs/guides/colorizing-single-band-raster-data/)); click 'Save & Style'

7. Adjust the CartoCSS with stop-points and colours:
~~~~
#warped {
  raster-opacity:1;
  raster-scaling:lanczos;
  raster-colorizer-default-mode: linear;
  raster-colorizer-default-color: transparent;
  raster-colorizer-stops:
    stop(0.694871, rgb(168,55,0))
    stop(0.87, rgb(212,107,0))
    stop(1.74, rgb(255,169,0))
    stop(2.61, rgb(255,239,55));
}
~~~~

8. Export layer as .MBTiles file.

9. Upload to Mapbox as a [tileset](https://www.mapbox.com/help/define-tileset/)

10. Use [that tileset](https://www.mapbox.com/studio/tilesets/camstark.d2tv0ze2/) as a layer in any webmap you want! Like [this one using MapboxGL](http://camstark.github.io/solar-potential), or [this one using Leaflet](http://camstark.github.io/solar-potential/index-leaflet.html)
