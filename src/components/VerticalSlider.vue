<!-- components/VerticalSlider.vue -->

<template>
    <div class="slider-container">
        <input type="range" min="0" max="100" v-model="sliderValue" @input="updateTimeline" class="vertical-slider" />
    </div>
</template>

<script>
export default {
    name: 'VerticalSlider',
    data() {
        return {
            sliderValue: 100
        };
    },
    methods: {
        updateTimeline() {
            this.$emit('positionChange', this.sliderValue);
        }
    }
};
</script>

<style scoped>
.slider-container {
    width: 10%;
    display: flex;
    align-items: center;
    justify-content: center;
    position: relative;
}

.vertical-slider {
    -webkit-appearance: none;
    appearance: none;
    height: 4px;
    background: #ddd;
    outline: none;
    opacity: 0.7;
    transition: opacity 0.2s;
    transform: rotate(90deg);
    transform-origin: 25%;
    direction: rtl;
}

.vertical-slider:hover {
    opacity: 1;
}

.vertical-slider::-webkit-slider-thumb {
    -webkit-appearance: none;
    appearance: none;
    width: 16px;
    height: 16px;
    background: #494949;
    cursor: pointer;
    border-radius: 50%;
}

.vertical-slider::-moz-range-thumb {
    width: 16px;
    height: 16px;
    background: #acacac;
    cursor: pointer;
    border-radius: 50%;
}
</style>
