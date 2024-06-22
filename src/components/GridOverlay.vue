<!-- components/GridOverlay.vue -->

<template>
    <div class="grid-overlay">
        <div v-for="point in points" :key="point.id" class="point" :style="{ top: point.y + '%', left: point.x + '%' }">
        </div>
    </div>
</template>

<script>
export default {
    name: 'GridOverlay',
    props: {
        points: {
            type: Array,
            required: true,
            default: () => []
        }
    }
};
</script>

<style scoped>
.grid-overlay {
    position: absolute;
    top: 0;
    left: 0;
    width: 100%;
    height: 100%;
    pointer-events: none;
    /* Ensures the grid doesn't block interactions with the map */
}

.point {
    width: 10px;
    height: 10px;
    background-color: red;
    border-radius: 50%;
    position: absolute;
    transform: translate(-50%, -50%);
    /* Centers the point at the given coordinates */
}
</style>
