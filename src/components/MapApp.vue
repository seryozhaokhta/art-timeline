<!-- components/MapApp.vue -->

<template>
    <div class="map-container">
        <div class="map-mask">
            <svg class="world-map" :viewBox="viewBox"
                :style="{ transform: `translate(${translateX}px, ${translateY}px)` }">
                <!-- Используем переменную для фоновой карты -->
                <image :href="worldMap" />
                <!-- Используем переменную для иконок точек -->
                <image v-for="point in processedPoints" :key="point.id" :href="pointIcon" :x="point.x" :y="point.y"
                    width="4" height="4" />
            </svg>
        </div>
    </div>
</template>

<script>
// Импортируем изображения напрямую
import worldMap from '@/assets/World_map_blank_without_borders.svg';
import pointIcon from '@/assets/POINT.svg';

export default {
    name: 'MapApp',
    data() {
        return {
            worldMap,
            pointIcon,
            points: [],
            viewBoxX: 0, // Добавляем начальное значение X для viewBox
            viewBoxY: 0, // Добавляем начальное значение Y для viewBox
            viewBoxWidth: 1000, // Значения width и height оставляем без изменений
            viewBoxHeight: 500,
        };
    },
    mounted() {
        this.adjustViewBoxForMobile();
        window.addEventListener('resize', this.adjustViewBoxForMobile);
    },
    beforeUnmount() {
        window.removeEventListener('resize', this.adjustViewBoxForMobile);
    },
    methods: {
        adjustViewBoxForMobile() {
            if (window.innerWidth <= 768) {
                this.viewBoxWidth = 500;
                this.viewBoxHeight = 250;
            } else {
                this.viewBoxWidth = 1000;
                this.viewBoxHeight = 500;
            }
        },
        moveMap(x, y) {
            // Изменяем viewBoxX и viewBoxY для "перемещения" карты
            this.viewBoxX += x;
            this.viewBoxY += y;
        },
    },
    computed: {
        viewBox() {
            // Возвращаем строку viewBox для использования в шаблоне
            return `${this.viewBoxX} ${this.viewBoxY} ${this.viewBoxWidth} ${this.viewBoxHeight}`;
        },
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
    max-width: 100%;
    height: 100vh;
    overflow: hidden;
    position: relative;
    border-radius: 24px;
    display: flex;
    justify-content: center;
    align-items: center;
}

.world-map {
    width: 100%;
    height: 400px;
    max-width: 900px;
    max-height: 450px;
}

@media (max-width: 768px) {
    .map-mask {
        border-radius: 14px;
    }
}
</style>