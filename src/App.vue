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
import MapApp from './components/MapApp.vue'; // Import MapApp component
import artData from './data/art-periodization.json';

export default {
  name: 'App',
  components: {
    ArtTimeline,
    AppHeader,
    AppFooter,
    MapApp // Register MapApp component
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
  margin-top: 60px;
}

.content-container {
  display: flex;
  width: 100%;
}

.content-half {
  flex: 1 1 50%;
  box-sizing: border-box;
}

@media (max-width: 768px) {
  .content-container {
    flex-direction: column;
  }

  .content-half {
    width: 100%;
  }

  .map-app {
    order: 1;
    margin-top: 0;
    /* Ensure map is flush against the top */
  }

  .art-timeline {
    order: 2;
  }
}
</style>
