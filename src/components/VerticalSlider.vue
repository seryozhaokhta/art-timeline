<!-- components/VerticalSlider.vue -->

<template>
    <div class="vertical-slider" @mousedown="startDrag" @mouseup="stopDrag">
        <div class="slider-bar"></div>
        <div class="slider-handle" :style="{ top: handlePosition + '%' }"></div>
    </div>
</template>

<script>
export default {
    data() {
        return {
            dragging: false,
            handlePosition: 0, // Позиция в процентах от верха
        };
    },
    methods: {
        startDrag() {
            this.dragging = true;
            // Добавляем обработчик перемещения мыши ко всему документу,
            // чтобы регулятор можно было тянуть, даже выйдя за его пределы
            document.addEventListener('mousemove', this.drag);
        },
        stopDrag() {
            this.dragging = false;
            document.removeEventListener('mousemove', this.drag);
            // Отправляем значение позиции в родительский компонент или событийную шину
            this.$emit('positionChange', this.handlePosition);
        },
        drag(e) {
            if (this.dragging) {
                const sliderBounds = this.$el.getBoundingClientRect();
                const position = e.clientY - sliderBounds.top; // Позиция курсора относительно регулятора
                const height = sliderBounds.height;
                this.handlePosition = (position / height) * 100;
                this.handlePosition = Math.max(0, Math.min(this.handlePosition, 100)); // Ограничиваем диапазон от 0 до 100%
            }
        }
    }
};
</script>

<style>
.vertical-slider {
    height: 600px;
    /* Примерная высота */
    width: 1.5px;
    /* Ещё более уменьшаем ширину для сужения слайдера */
    background-color: #797979;
    margin-top: 0;
    /* Убираем верхний отступ, если он есть */
    top: 0;
    /* Поднимаем слайдер вверх */
}

.slider-handle {
    position: relative;
    border-radius: 24px;
    left: 50%;
    /* Центрируем ручку по горизонтали */
    transform: translateX(-50%);
    /* Смещаем ручку на половину её ширины назад, чтобы она была точно по центру */
    width: 8px;
    /* Уменьшаем ширину ручки */
    height: 42px;
    /* Высота ручки */
    background-color: #333;
    cursor: pointer;
}
</style>