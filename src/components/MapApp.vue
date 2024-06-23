<!-- components/MapApp.vue -->

<template>
    <div class="map-container">
        <div class="map-mask">
            <svg class="world-map" viewBox="0 0 1000 500">
                <image href="@/assets/World_map_blank_without_borders.svg" width="100%" height="100%" />
                <image v-for="point in points" :key="point.id" :href="pointIcon" :x="point.x" :y="point.y" width="4"
                    height="4" />
            </svg>
        </div>
    </div>
</template>

<script>
import pointIcon from '@/assets/POINT.svg'; // Путь к вашему SVG-изображению точки

export default {
    name: 'MapApp',
    data() {
        return {
            pointIcon,
            points: [
                // Координаты для изображений точек в системе координат SVG
                { id: 1, x: 537, y: 141 },
                { id: 2, x: 537, y: 136 },
                { id: 3, x: 529, y: 132 }
            ]
        };
    },
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
}

.map-mask {
    width: 900px;
    /* Установите ширину, чтобы она соответствовала максимальной ширине */
    height: 900px;
    /* Максимальная ширина для маски */
    overflow: hidden;
    position: relative;
    border-radius: 24px;
    display: flex;
    justify-content: center;
    align-items: center;
}

.world-map {
    transform: scale(4) translateX(-10%) translateY(20%);
    transform-origin: 50% 50%;
}
</style>