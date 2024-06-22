<!-- App.vue -->

<template>
  <div id="app">
    <AppHeader />
    <div class="content-container">
      <MapApp class="content-half map-app" />
      <ArtTimeline :data="artData" class="content-half art-timeline" />
    </div>
    <AppFooter />
  </div>
</template>

<script>
import ArtTimeline from './components/ArtTimeline.vue';
import AppHeader from './components/AppHeader.vue';
import AppFooter from './components/AppFooter.vue';
import MapApp from './components/MapApp.vue';
import artData from './data/art-periodization.json';

export default {
  name: 'App',
  components: {
    ArtTimeline,
    AppHeader,
    AppFooter,
    MapApp
  },
  data() {
    return {
      artData
    };
  }
};
</script>

<style>
#app {
  font-family: Helvetica, sans-serif;
  color: #2c3e50;
}

.content-container {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
}

.content-half {
  flex: 1;
  box-sizing: border-box;
}

@media (max-width: 768px) {
  .content-container {
    flex-direction: column;
  }
}
</style>
