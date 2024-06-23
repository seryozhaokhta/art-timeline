<!-- components/MapApp.vue -->

<template>
    <div class="map-container">
        <div class="map-mask">
            <svg class="world-map" viewBox="0 0 1000 500">
                <!-- Используем переменную для фоновой карты -->
                <image :href="worldMap" width="100%" height="100%" />
                <!-- Используем переменную для иконок точек -->
                <image v-for="point in processedPoints" :key="point.id" :href="pointIcon" :x="point.x" :y="point.y"
                    width="4" height="4" />
            </svg>
        </div>
    </div>
</template>

<script>
import worldMap from '@/assets/World_map_blank_without_borders.svg';
import pointIcon from '@/assets/POINT.svg';

export default {
    name: 'MapApp',
    data() {
        return {
            worldMap,
            pointIcon,
            points: [
                { id: 1, x: 537, y: 141 },
                { id: 2, x: 537, y: 136 },
                { id: 3, x: 529, y: 132 }
            ]
        };
    },
    computed: {
        processedPoints() {
            return this.points;
        }
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
}

.map-mask {
    width: 900px;
    height: 450px;
    /* Установите фиксированные размеры для маски */
    overflow: hidden;
    position: relative;
    border-radius: 24px;
    display: flex;
    justify-content: center;
    align-items: center;
}

.world-map {
    transform: scale(4) translateX(-10%) translateY(20%);
}

@media (max-width: 768px) {
    .map-mask {
        width: 100%;
        height: auto;
        border-radius: 14px;
    }

    .world-map {
        /* Адаптируйте трансформацию для мобильных устройств при необходимости */
        transform: scale(2) translateX(-20%) translateY(10%);
    }
}
</style>