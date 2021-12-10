<template>
  <div class="container">
    <div class="p-5">Town Planning - Medscape {{ geoserverUrl }}</div>
    <map-view></map-view>
  </div>
</template>

<script>
import MapView from '@/views/MapView'
export default {
  name: 'Home',
  components: {
    MapView
  },
  data() {
    return {
      geoserverUrl: ''
    }
  },
  mounted() {
    this.geoserverUrl = process.env.VUE_APP_GEOSERVER_URL
  }
}
</script>
