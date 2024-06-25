<!-- components/VerticalSlider.vue -->

<template>
    <div class="slider-container" @touchstart="startDrag" @touchend="stopDrag" @mousedown="startDrag"
        @mouseup="stopDrag">
        <div class="slider-track"></div>
        <div class="slider-thumb" :style="{ top: thumbTop + 'px' }" ref="thumb"></div>
    </div>
</template>

<script>
export default {
    name: 'VerticalSlider',
    data() {
        return {
            thumbTop: 0,
            dragging: false,
        };
    },
    methods: {
        startDrag(event) {
            event.preventDefault(); // Предотвращаем действие по умолчанию для события touchstart или mousedown
            this.dragging = true;
            document.addEventListener('touchmove', this.moveSlider, { passive: false });
            document.addEventListener('mousemove', this.moveSlider);
        },
        stopDrag() {
            this.dragging = false;
            document.removeEventListener('touchmove', this.moveSlider);
            document.removeEventListener('mousemove', this.moveSlider);

            // Ваш код для плавного перемещения кружка к центру выбранной эпохи
        },
        moveSlider(event) {
            if (!this.dragging) return;
            const { top, height } = this.$el.getBoundingClientRect();
            let newY = event.clientY || event.touches[0].clientY - top;
            newY = Math.max(0, Math.min(newY, height));
            this.thumbTop = newY;
            this.$emit('positionChange', 100 - (newY / height) * 100);
        },
    },
};
</script>

<style scoped>
.slider-container {
    position: relative;
    height: 1000px;
    width: 20px;
    margin: 0 auto;
    user-select: none;
}

.slider-track {
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    height: 100%;
    width: 2px;
    background-color: #ddd;
    user-select: none;
}

.slider-thumb {
    position: absolute;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 20px;
    height: 20px;
    background-color: #494949;
    border-radius: 50%;
    cursor: grab;
    user-select: none;
}
</style>