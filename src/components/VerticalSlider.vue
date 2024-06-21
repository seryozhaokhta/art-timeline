<!-- components/VerticalSlider.vue -->

<template>
    <div class="vertical-slider" @mousedown="startDrag" @touchstart="startDrag">
        <div class="slider-bar"></div>
        <div class="slider-handle" :style="{ top: handlePosition + '%' }"></div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            dragging: false,
            handlePosition: 0,
        };
    },
    methods: {
        startDrag(event) {
            event.preventDefault();
            this.dragging = true;
            document.addEventListener('mousemove', this.drag);
            document.addEventListener('touchmove', this.drag);
            document.addEventListener('mouseup', this.stopDrag);
            document.addEventListener('touchend', this.stopDrag);
        },
        stopDrag() {
            this.dragging = false;
            document.removeEventListener('mousemove', this.drag);
            document.removeEventListener('touchmove', this.drag);
            document.removeEventListener('mouseup', this.stopDrag);
            document.removeEventListener('touchend', this.stopDrag);
            this.$emit('positionChange', this.handlePosition);
        },
        drag(event) {
            if (!this.dragging) return;
            const clientY = event.touches ? event.touches[0].clientY : event.clientY;
            const sliderBounds = this.$el.getBoundingClientRect();
            const position = clientY - sliderBounds.top;
            const height = sliderBounds.height;
            this.handlePosition = (position / height) * 100;
            this.handlePosition = Math.max(0, Math.min(this.handlePosition, 100));
        }
    }
};
</script>

<style>
.vertical-slider {
    height: 600px;
    /* Default height */
    width: 1.5px;
    /* Default width */
    background-color: #797979;
    margin-top: 0;
    top: 0;
}

.slider-handle {
    position: relative;
    border-radius: 24px;
    left: 50%;
    transform: translateX(-50%);
    width: 8px;
    /* Default width */
    height: 42px;
    /* Default height */
    background-color: #333;
    cursor: pointer;
}

/* Адаптивные стили для мобильных устройств */
@media (max-width: 768px) {
    .vertical-slider {
        height: 300px;
        /* Уменьшаем высоту для мобильных устройств */
        width: 1.5px;
        /* Можно адаптировать ширину, если необходимо */
    }

    .slider-handle {
        width: 16px;
        /* Увеличиваем ширину ручки для лучшего взаимодействия на сенсорных экранах */
        height: 42px;
        /* Высоту ручки можно адаптировать, если необходимо */
    }
}
</style>