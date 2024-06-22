<!-- components/MapApp.vue -->

<template>
    <div class="map-container">
        <div class="map-mask">
            <img :src="worldMap" alt="World Map" class="world-map" />
            <GridOverlay :points="points" />
        </div>
    </div>
</template>

<script>
import worldMap from '@/assets/World_map_blank_without_borders.svg';
import GridOverlay from './GridOverlay.vue';

export default {
    name: 'MapApp',
    components: {
        GridOverlay
    },
    data() {
        return {
            worldMap,
            points: [
                { id: 1, x: 10, y: 20 },
                { id: 2, x: 30, y: 40 },
                { id: 3, x: 50, y: 60 }
            ]
        };
    }
};
</script>

<style scoped>
.map-container {
    display: flex;
    justify-content: center;
    align-items: center;
    width: 100%;
    height: 100%;
    padding: 20px;
    background-color: rgba(128, 128, 128, 0.103);
    position: relative;
    /* Ensure the container is positioned relative for absolute positioning inside */
}

.map-mask {
    width: 700px;
    height: 700px;
    overflow: hidden;
    position: relative;
    border-radius: 24px;
}

.world-map {
    position: absolute;
    top: 90%;
    left: -1%;
    transform: scale(5.5);
    transform-origin: 50% 50%;
    width: 100%;
    height: auto;
}
</style>
