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
// Импортируем изображения напрямую
import worldMap from '@/assets/World_map_blank_without_borders.svg';
import pointIcon from '@/assets/POINT.svg';

export default {
    name: 'MapApp',
    data() {
        return {
            worldMap, // Добавляем изображение карты как переменную данных
            pointIcon, // Иконка точки теперь также переменная данных
            points: [
                // Координаты для изображений точек в системе координат SVG
                { id: 1, x: 537, y: 141 },
                { id: 2, x: 537, y: 136 },
                { id: 3, x: 529, y: 132 }
            ]
        };
    },
    computed: {
        processedPoints() {
            // Здесь может быть логика для обработки точек, например, фильтрация или сортировка
            return this.points; // Пока просто возвращаем исходный массив
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
    width: 100%;
    height: 60vh;
    overflow: hidden;
    position: relative;
    border-radius: 24px;
    justify-content: center;
    align-items: center;
}

.world-map {
    transform: translate(0, 35px);
    scale: 1;
}

/* Медиа-запрос для устройств с максимальной шириной 768px */
@media (max-width: 768px) {
    .map-mask {
        border-radius: 14px;
    }

    .world-map {
        transform: translate(0, 35px);
        max-width: 100%;
        /* Увеличиваем карту, чтобы она занимала больше места на экране */
        max-height: none;
        scale: 6;
        /* Убираем ограничение по высоте */
    }
}
</style>