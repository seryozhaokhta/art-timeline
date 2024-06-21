<!-- components/MapApp.vue -->

<template>
    <div class="map-container">
        <div class="map-mask">
            <img :src="worldMap" alt="World Map" class="world-map" />
        </div>
    </div>
</template>

<script>
import worldMap from '@/assets/World_map_blank_without_borders.svg';

export default {
    name: 'MapApp',
    data() {
        return {
            worldMap
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
}

.map-mask {
    width: 700px;
    /* Define the size of the mask */
    height: 700px;
    overflow: hidden;
    /* Ensure the image does not overflow the mask */
    position: relative;
    border-radius: 24px;
}

.world-map {
    position: absolute;
    top: 90%;
    /* Adjust to position the map */
    left: -1%;
    /* Adjust to position the map */
    transform: scale(5.5);
    /* Zoom into the map */
    transform-origin: 50% 50%;
    /* Center the zoom on the map */
    width: 100%;
    /* Ensure the map covers the entire area */
    height: auto;
}
</style>
