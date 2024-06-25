<template>
    <div class="slider-container" @mousedown="startDrag" @mouseup="stopDrag">
        <div class="slider-track"></div>
        <div class="slider-thumb" :style="{ top: thumbTop + 'px' }" ref="thumb"></div>
    </div>
</template>

<script>
export default {
    name: 'VerticalSlider',
    data() {
        return {
            thumbTop: 0, // Позиция кружка от верха контейнера
            dragging: false,
        };
    },
    methods: {
        startDrag() {
            this.dragging = true;
            // Добавляем обработчик перемещения мыши на весь документ
            document.addEventListener('mousemove', this.moveSlider);
        },
        stopDrag() {
            this.dragging = false;
            document.removeEventListener('mousemove', this.moveSlider);

            const epochElement = document.querySelector('.epoch.active-epoch');
            if (epochElement) {
                const epochCenter = epochElement.offsetTop + epochElement.offsetHeight / 2;

                const thumbPosition = this.thumbTop + this.$el.offsetTop;
                const thumbCenter = thumbPosition + this.$refs.thumb.offsetHeight / 2;

                let newThumbTop = this.thumbTop;
                if (thumbCenter < epochCenter) {
                    newThumbTop += epochCenter - thumbCenter;
                } else {
                    newThumbTop -= thumbCenter - epochCenter;
                }

                const animationDuration = 300;
                const startTime = performance.now();

                const animateThumb = (timestamp) => {
                    const progress = timestamp - startTime;
                    if (progress < animationDuration) {
                        const easing = progress / animationDuration;
                        this.thumbTop = this.thumbTop + (newThumbTop - this.thumbTop) * easing;
                        requestAnimationFrame(animateThumb);
                    } else {
                        this.thumbTop = newThumbTop;
                    }
                };

                requestAnimationFrame(animateThumb);
            }
        },
        moveSlider(event) {
            if (!this.dragging) return;
            const { top, height } = this.$el.getBoundingClientRect();
            let newY = event.clientY - top;
            newY = Math.max(0, Math.min(newY, height)); // Ограничиваем перемещение внутри контейнера
            this.thumbTop = newY;
            // Вычисляем и отправляем значение в процентах
            this.$emit('positionChange', 100 - (newY / height) * 100);
        },
    },
};
</script>

<style scoped>
.slider-container {
    position: relative;
    height: 1000px;
    /* Высота контейнера слайдера */
    width: 20px;
    /* Ширина контейнера слайдера */
    margin: 0 auto;
    /* Центрирование */
    user-select: none;
    /* Добавляем свойство для предотвращения выделения текста */
}

.slider-track {
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    height: 100%;
    width: 2px;
    /* Ширина трека */
    background-color: #ddd;
    user-select: none;
    /* Добавляем свойство для предотвращения выделения текста */
}

.slider-thumb {
    position: absolute;
    left: 50%;
    transform: translate(-50%, -50%);
    width: 20px;
    /* Ширина кружка */
    height: 20px;
    /* Высота кружка */
    background-color: #494949;
    border-radius: 50%;
    /* Круглая форма */
    cursor: grab;
    user-select: none;
    /* Добавляем свойство для предотвращения выделения текста */
}
</style>