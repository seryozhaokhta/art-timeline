<!-- components/MapApp.vue -->

<template>
    <div class="map-container">
        <div class="map-mask">
            <svg class="world-map" :viewBox="viewBox">
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
// Импортируем изображения напрямую
import worldMap from '@/assets/World_map_blank_without_borders.svg';
import pointIcon from '@/assets/POINT.svg';

export default {
    name: 'MapApp',
    data() {
        return {
            worldMap, // Добавляем изображение карты как переменную данных
            pointIcon, // Иконка точки теперь также переменная данных
            points: [],
            viewBox: '0 0 1000 500', // Начальное значение viewBox
        };
    },
    mounted() {
        this.adjustViewBoxForMobile();
        window.addEventListener('resize', this.adjustViewBoxForMobile);
    },
    beforeUnmount() { // Здесь было изменено с beforeDestroy на beforeUnmount
        window.removeEventListener('resize', this.adjustViewBoxForMobile);
    },
    methods: {
        adjustViewBoxForMobile() {
            if (window.innerWidth <= 768) {
                // Уменьшаем область viewBox для мобильных устройств
                this.viewBox = '250 125 500 250';
            } else {
                // Возвращаем к исходному значению для десктопов
                this.viewBox = '0 0 1000 500';
            }
        },
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
    max-width: 100%;
    max-height: 100vh;
    overflow: hidden;
    position: relative;
    border-radius: 24px;
    display: flex;
    justify-content: center;
    align-items: center;
}

.world-map {
    width: 100%;
    height: auto;
    scale: 2;
    max-width: 900px;
    max-height: 450px;
    /* Половина изначальной высоты viewBox, чтобы сохранить аспект */
}

@media (max-width: 768px) {
    .map-mask {
        border-radius: 14px;
    }
}
</style>