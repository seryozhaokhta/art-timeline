<!-- App.vue -->

<template>
  <div id="app">
    <AppHeader />
    <div class="content-container">
      <MapApp class="content-half map-app" />
      <VerticalSlider @positionChange="handlePositionChange" class="slider" />
      <ArtTimeline :data="artData" :activeEpochIndex="activeEpochIndex" class="content-half art-timeline" />
    </div>
    <AppFooter />
  </div>
</template>

<script>
import ArtTimeline from './components/ArtTimeline.vue';
import AppHeader from './components/AppHeader.vue';
import AppFooter from './components/AppFooter.vue';
import MapApp from './components/MapApp.vue';
import VerticalSlider from './components/VerticalSlider.vue';
import artData from './data/art-periodization.json';

export default {
  name: 'App',
  components: {
    ArtTimeline,
    AppHeader,
    AppFooter,
    MapApp,
    VerticalSlider
  },
  data() {
    return {
      artData,
      activeEpochIndex: null,
    };
  },
  methods: {
    handlePositionChange(position) {
      // Преобразуем позицию слайдера в индекс активной эпохи
      const totalEpochs = this.artData['Western-European-art-periodization'].length;
      const index = Math.floor(position * totalEpochs / 100);
      this.activeEpochIndex = index;
    }
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
  /* Центрирует элементы по вертикали */
  justify-content: center;
  /* Центрирует элементы по горизонтали */
  width: 100%;
}

.content-half {
  flex: 1;
  /* Каждый занимает равную ширину */
  box-sizing: border-box;
}


@media (max-width: 768px) {
  .content-container {
    flex-direction: column;
  }

  .slider {
    position: relative;
  }
}
</style>