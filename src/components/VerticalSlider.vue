<!-- components/VerticalSlider.vue -->

<template>
    <div class="vertical-slider" @mousedown="startDrag" @touchstart="startDrag">
        <div class="slider-bar">
            <div v-for="epoch in epochs" :key="epoch" class="epoch-marker" :style="{ top: epoch + '%' }"></div>
        </div>
        <div class="slider-handle" :style="{ top: handlePosition + '%' }"></div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            dragging: false,
            handlePosition: 0,
            epochs: [1, 20, 40, 60, 80, 95] // Позиции эпох в процентах
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
            this.snapToNearestEpoch();
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
        },
        snapToNearestEpoch() {
            let nearestEpoch = this.epochs[0];
            let minDistance = Math.abs(this.handlePosition - this.epochs[0]);

            for (let i = 1; i < this.epochs.length; i++) {
                const distance = Math.abs(this.handlePosition - this.epochs[i]);
                if (distance < minDistance) {
                    minDistance = distance;
                    nearestEpoch = this.epochs[i];
                }
            }

            this.handlePosition = nearestEpoch;
        }
    }
};
</script>

<style>
.vertical-slider {
    position: absolute;
    height: 900px;
    width: 1.5px;
    background-color: #797979;
}

.slider-bar {
    position: absolute;
    width: 100%;
    height: 100%;
}

.epoch-marker {
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    width: 10px;
    height: 10px;
    background-color: #ff0000;
    border: 2px solid white;
    border-radius: 50%;
}

.slider-handle {
    position: absolute;
    border-radius: 24px;
    left: 50%;
    transform: translateX(-50%);
    width: 17px;
    height: 42px;
    background-color: #333;
    cursor: pointer;
}
</style>
