<template>
  <div id="ol-map">

  </div>
</template>

<script>
import {Map, View} from 'ol'
// import TileLayer from "ol/layer/Tile";
import OSM from "ol/source/OSM";
import { fromLonLat } from "ol/proj";
import TileWMS from 'ol/source/TileWMS';
import ImageWMS from 'ol/source/ImageWMS';
import {Image as ImageLayer, Tile as TileLayer} from 'ol/layer';

// http://localhost:8081/geoserver/medscape/wms?service=WMS&version=1.1.0&request=GetMap&layers=medscape%3ACyprus_LVL2&bbox=432298.625%2C3823859.0%2C644787.6875%2C3952037.75&width=768&height=463&srs=EPSG%3A32636&styles=&format=application/openlayers
export default {
  data() {
    return {
      olmap: null
    }
  },
  mounted() {
    let geoserverUrl = process.env.VUE_APP_GEOSERVER_URL
    let usa = new TileLayer({
      extent: [-13884991, 2870341, -7455066, 6338219],
      source: new TileWMS({
        url: 'https://ahocevar.com/geoserver/wms',
        params: {'LAYERS': 'topp:states', 'TILED': true},
        serverType: 'geoserver',
        transition: 0,
      }),
    });

    let admb_dist = new TileLayer({
      source: new TileWMS({
        url: `${geoserverUrl}/medscape/wms?`,
        params: {'LAYERS': 'medscape:admb_dist'},
        serverType: 'geoserver',
        transition: 0,
      }),
    });

    let admb_dist_image = new ImageLayer({
      source: new ImageWMS({
        url: `${geoserverUrl}/medscape/wms?`,
        params: {'LAYERS': 'medscape:admb_dist'},
        ratio: 1,
        serverType: 'geoserver',
      }),
    });

    let medscape_polygons = new TileLayer({
      source: new TileWMS({
        url: `${geoserverUrl}/medscape/wms?`,
        params: {'LAYERS': 'medscape:Cyprus_LVL2', 'TILED': true},
        serverType: 'geoserver',
        transition: 0,
      }),
    });

    let medscape_polygons_image = new ImageLayer({
      source: new ImageWMS({
        url: `${geoserverUrl}/medscape/wms?`,
        params: {'LAYERS': 'medscape:Cyprus_LVL2'},
        ratio: 1,
        serverType: 'geoserver',
      }),
    });

    let indxcas_cas5000_image = new ImageLayer({
      source: new ImageWMS({
        url: `${geoserverUrl}/medscape/wms?`,
        params: {'LAYERS': 'medscape:indxcas_cas5000, medscape:indxcas_cas2500'},
        ratio: 1,
        serverType: 'geoserver',
      }),
    });

    this.olmap = new Map({
      target: 'ol-map',
      layers: [
        new TileLayer({
          source: new OSM(),
        }),
        medscape_polygons_image,
        admb_dist_image,
        indxcas_cas5000_image
      ],
      view: new View({
        center: fromLonLat([33.3587, 35.17025]),
        zoom: 9,
        maxZoom: 19,
      })
    })    
  }
}
</script>

<style>
#ol-map {
  width: 100%;
  height: calc(100vh - 60px);
}
</style>