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
    props: {
        targetPosition: Number, // Новое свойство для получения целевой позиции от родительского компонента
    },
    data() {
        return {
            thumbTop: 0,
            dragging: false,
            lastPosition: 0, // Новая переменная для хранения последней позиции
            throttleTimeout: null // Переменная для хранения таймаута
        };
    },
    watch: {
        targetPosition(newPosition) {
            this.updateThumbPosition(newPosition);
        }
    },
    methods: {
        startDrag(event) {
            event.preventDefault();
            this.dragging = true;
            document.addEventListener('touchmove', this.moveSlider, { passive: false });
            document.addEventListener('mousemove', this.moveSlider);
        },
        stopDrag() {
            this.dragging = false;
            document.removeEventListener('touchmove', this.moveSlider);
            document.removeEventListener('mousemove', this.moveSlider);
        },
        moveSlider(event) {
            if (!this.dragging) return;

            const { top, height } = this.$el.getBoundingClientRect();
            const thumbHeight = this.$refs.thumb.offsetHeight;
            let newY = event.clientY - top || (event.touches ? event.touches[0].clientY - top : event.clientY - top);
            newY = Math.max(thumbHeight / 2, Math.min(newY, height - thumbHeight / 2));
            this.thumbTop = newY;

            const position = 100 - ((newY - thumbHeight / 2) / (height - thumbHeight)) * 100;

            if (!this.throttleTimeout) {
                this.throttleTimeout = setTimeout(() => {
                    this.$emit('positionChange', position);
                    this.throttleTimeout = null;
                }, 100);
            }

            this.lastPosition = position;
        },
        updateThumbPosition(position) {
            const { height } = this.$el.getBoundingClientRect();
            const thumbHeight = this.$refs.thumb.offsetHeight;
            const newY = (height - thumbHeight) * (1 - position / 100) + thumbHeight / 2;
            this.thumbTop = newY;
        }
    },

};
</script>

<style scoped>
.slider-container {
    position: relative;
    height: 80vh;
    margin-right: 40px;
    margin-left: 10px;
    user-select: none;
}

.slider-track {
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    height: 100%;
    width: 1px;
    background-color: #000000;
    opacity: 0.2;
    user-select: none;
}

.slider-thumb {
    position: absolute;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 20px;
    height: 20px;
    background-color: #000000;
    border: 3px solid white;
    border-radius: 50%;
    user-select: none;
}
</style>
