<!-- components/VerticalSlider.vue -->

<template>
    <div class="vertical-slider" @mousedown="startDrag" @touchstart="startDrag">
        <div class="slider-bar">
            <div v-for="(epoch, index) in epochs" :key="epoch.value" class="epoch-marker"
                :style="{ top: calculateEpochPosition(epoch, index) + '%' }">
                <div v-for="subItem in epoch.subItems" :key="subItem" class="subitem-marker"
                    :style="{ top: subItemOffset(epoch, subItem, index) + '%' }"></div>
            </div>
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
            epochs: [
                { value: 1, subItems: [100, 200, 300, 400] },
                { value: 20, subItems: [100, 200, 300, 400] },
                { value: 40, subItems: [100, 200, 300, 400] },
                { value: 60, subItems: [100, 200] },
                { value: 80, subItems: [100, 200] },
                { value: 95, subItems: [100, 200, 300, 400, 500] }
            ],
            expandedEpochIndex: null,
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
            let nearestEpoch = this.epochs[0].value;
            let minDistance = Math.abs(this.handlePosition - this.epochs[0].value);
            let nearestIndex = 0;

            for (let i = 1; i < this.epochs.length; i++) {
                const distance = Math.abs(this.handlePosition - this.epochs[i].value);
                if (distance < minDistance) {
                    minDistance = distance;
                    nearestEpoch = this.epochs[i].value;
                    nearestIndex = i;
                }
            }

            this.handlePosition = nearestEpoch;
            this.expandedEpochIndex = nearestIndex;
        },
        calculateEpochPosition(epoch, index) {
            const baseOffset = 5; // Базовый отступ от верха слайдера в процентах
            // Исключение для первой эпохи, когда она активна
            if (index === 0 && this.expandedEpochIndex === 0) {
                return epoch.value; // Первая эпоха остаётся на месте
            } else if (index === this.expandedEpochIndex) {
                return Math.max(epoch.value, baseOffset);
            } else {
                const offset = index > this.expandedEpochIndex ? 10 : -10;
                return Math.max(epoch.value + offset, baseOffset);
            }
        },
        subItemOffset(epoch, subItem, epochIndex) {
            const expansionFactor = 5; // Коэффициент расширения
            const baseOffset = 5; // Базовый отступ от верха слайдера в процентах

            if (epochIndex === this.expandedEpochIndex) {
                return Math.max(epoch.value + subItem * expansionFactor, baseOffset);
            } else {
                return Math.max(epoch.value + (subItem / 2), baseOffset);
            }
        }
    }
};
</script>

<style>
.vertical-slider {
    position: absolute;
    height: 1000px;
    width: 3.5px;
    background-color: #5c5c5c;
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

.subitem-marker {
    position: absolute;
    left: 50%;
    transform: translateX(-50%);
    width: 5px;
    height: 5px;
    background-color: #00ff00;
    border: 1px solid white;
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

