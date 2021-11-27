<template>
  <div class="container">
    <div class="p-5">Medscape Project Pilot</div>
    <map-view></map-view>
  </div>
</template>

<script>
import MapView from '@/views/MapView'
export default {
  name: 'Home',
  components: {
    MapView
  }
}
</script>
