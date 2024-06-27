<!-- components/MapApp.vue -->

<template>
    <div class="map-container">
        <div class="map-mask">
            <svg class="world-map" viewBox="0 0 1000 500">
                <image :href="worldMap" width="100%" height="100%" />
                <g v-for="point in points" :key="point.id">
                    <text :x="point.x + 2" :y="point.y - 2" class="point-label">{{ point.name }}</text>
                    <image :href="pointIcon" :x="point.x" :y="point.y" width="4" height="4"
                        @click="handleClick(point)" />
                </g>
                <g v-for="monument in monuments" :key="monument.id">
                    <text :x="monument.x + 2" :y="monument.y - 2" class="monument-label">{{ monument.name }}</text>
                    <image :href="monumentIcon" :x="monument.x" :y="monument.y" width="4" height="4"
                        @click="handleClick(monument)" />
                </g>
            </svg>
        </div>
    </div>
</template>

<script>
import worldMap from '@/assets/World_map_blank_without_borders.svg';
import pointIcon from '@/assets/POINT.svg';
import monumentIcon from '@/assets/monuments.svg';
import pointsData from '@/data/map-points.json';
import monumentsData from '@/data/monuments-db.json';

export default {
    name: 'MapApp',
    data() {
        return {
            worldMap,
            pointIcon,
            monumentIcon,
            points: pointsData,
            monuments: monumentsData
        };
    },
    methods: {
        handleClick(item) {
            alert(`You clicked on ${item.name}`);
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
    height: max-content;
    padding: 20px;
    background-color: rgba(0, 0, 0, 0.13);
    position: relative;
}

.map-mask {
    width: 100%;
    height: 100vh;
    overflow: hidden;
    border-radius: 24px;
    justify-content: center;
    align-items: center;
}

.world-map {
    transform: translate(50px, 560px) scale(4);
    transition: transform 0.3s ease;
    /* Переносим сюда */
}

.world-map:hover {
    transform: translate(50px, 560px) scale(5);
    /* Добавляем transform здесь */
}

.point-label {
    font-size: 4px;
    fill: rgb(255, 255, 255);
    text-anchor: middle;
    font-weight: bold;
}

.monument-label {
    font-size: 4px;
    fill: rgb(255, 255, 255);
    /* Золотой цвет для различия с точками */
    text-anchor: middle;
    font-weight: bold;
}

/* Медиа-запрос для устройств с максимальной шириной 768px */
@media (max-width: 768px) {
    .map-mask {
        border-radius: 14px;
        height: 50vh;
    }

    .world-map {
        transform: translate(0, 55px);
        max-width: 100%;
        max-height: none;
        scale: 6;
    }
}
</style>
