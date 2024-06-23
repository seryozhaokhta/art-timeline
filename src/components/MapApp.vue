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
    /* Занимает всю ширину */
    height: 100vh;
    /* Занимает всю высоту видимой части экрана */
    padding: 20px;
    /* Добавляем немного отступа */
    box-sizing: border-box;
    /* Гарантируем, что padding не изменит размеры */
}

.map-mask {
    width: 100%;
    /* Адаптивная ширина */
    max-width: 900px;
    /* Максимальная ширина для больших экранов */
    height: auto;
    /* Автоматическая высота для сохранения пропорций */
    aspect-ratio: 16 / 9;
    /* Соотношение сторон, например, 16:9 */
    overflow: hidden;
    /* Скрываем части карты, выходящие за пределы маски */
    position: relative;
    border-radius: 24px;
    /* Скругляем углы для красивого вида */
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
    /* Добавляем немного тени для глубины */
}

.world-map {
    width: 100%;
    /* Карта занимает всю ширину маски */
    height: 100%;
    /* Карта занимает всю высоту маски */
    object-fit: cover;
    /* Гарантируем, что карта покрывает всю маску, сохраняя пропорции */
}
</style>