<!-- components/ArtTimeline.vue -->

<template>
    <div class="timeline" @click.self="closeAllEpochs">
        <VerticalSlider @positionChange="handlePositionChange" class="slider" :targetPosition="sliderPosition" />
        <div class="epochs-container">
            <ArtEpoch v-for="(epoch, index) in data['Western-European-art-periodization']" :key="epoch.id"
                :epoch="epoch" :index="index" :activeTag="activeTag" @toggleExpand="toggleExpand"
                @changePeriodization="changePeriodization" />
        </div>
    </div>
</template>

<script>
import VerticalSlider from './VerticalSlider.vue';
import ArtEpoch from './Epoch.vue';

export default {
    name: 'ArtTimeline',
    components: {
        VerticalSlider,
        ArtEpoch,
    },
    props: {
        data: Object,
    },
    data() {
        return {
            activeTag: null,
            activeEpoch: null,
            activeEpochIndex: null,
            sliderPosition: 100,
        };
    },
    created() {
        this.initializeExpansionStates();
    },
    methods: {
        handlePositionChange(position) {
            const totalEpochs = this.data['Western-European-art-periodization'].length;
            const index = totalEpochs - 1 - Math.floor(position * totalEpochs / 100);
            this.activeEpochIndex = index;
            this.activeTag = null;
            this.initializeExpansionStates();
        },
        initializeExpansionStates() {
            this.data['Western-European-art-periodization'].forEach((epoch, index) => {
                epoch.expanded = (index === this.activeEpochIndex);
                if (epoch.expanded && this.activeTag) {
                    this.changePeriodization(this.activeTag, epoch);
                }
            });
        },
        toggleExpand(epoch, index) {
            if (epoch.expanded) {
                epoch.expanded = false;
                this.activeEpochIndex = null;
            } else {
                this.data['Western-European-art-periodization'].forEach(e => e.expanded = false);
                epoch.expanded = true;
                this.activeEpochIndex = index;
                this.activeTag = null;
                this.updateSliderPosition(index);
            }
        },
        changePeriodization(tag, epoch) {
            this.activeTag = tag;
            if (epoch.tags && epoch.tags.includes(tag)) {
                epoch.expanded = true;
                if (epoch.subItems && Array.isArray(epoch.subItems)) {
                    epoch.subItems.forEach(subItem => {
                        subItem.expanded = true;
                    });
                }
            }
            if (epoch[this.activeTag] && Array.isArray(epoch[this.activeTag])) {
                epoch[this.activeTag].forEach(tagSpecificSubItem => {
                    tagSpecificSubItem.expanded = true;
                });
            }
        },
        closeAllEpochs() {
            this.data['Western-European-art-periodization'].forEach(epoch => {
                epoch.expanded = false;
            });
            this.activeEpochIndex = null;
            this.activeTag = null;
        },
        updateSliderPosition(index) {
            const totalEpochs = this.data['Western-European-art-periodization'].length;
            const position = 100 - (index / (totalEpochs - 1)) * 100;
            this.sliderPosition = position;
        },
    },
};
</script>

<style>
.timeline {
    display: flex;
    align-items: flex-start;
    max-width: 20%;
    background-color: #f8f8f8;
    border-radius: 10px;
    box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
}

.epochs-container {
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
}

.epoch {
    width: 100%;
    margin-bottom: 5px;
    display: flex;
}

.epoch-content {
    background-color: #fff;
    padding: 5px 5px;
    border-radius: 8px;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
    width: 70%;
}

.epoch-header:hover {
    background-color: transparent;
}

h2 {
    font-size: 25px;
    font-style: normal;
    font-weight: bold;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

.sub-items {
    overflow: hidden;
    transition: max-height 0.3s ease;
    max-height: 0;
}

.sub-items .tags {
    margin-bottom: 10px;
}

.tag {
    display: inline-block;
    padding: 5px 10px;
    background-color: #ddd;
    color: #333;
    border-radius: 15px;
    margin-right: 5px;
    margin-bottom: 5px;
    cursor: pointer;
}

.tag:hover {
    background-color: #ccc;
}

.sub-item {
    margin-bottom: 10px;
}

.sub-item h3 {
    margin: 0;
    margin-bottom: 0px;
    cursor: pointer;
    display: flex;
    align-items: center;
}

.sub-item .icon {
    margin-left: 10px;
    transition: transform 0.3s ease;
}

.rotated {
    transform: rotate(90deg);
}

.nested-sub-items {
    margin-top: 10px;
}

.nested-sub-items li {
    margin-bottom: 5px;
}

.active-epoch .sub-items {
    max-height: var(--sub-items-max-height);
}

.expand-enter-active,
.expand-leave-active {
    transition: all 0.3s ease;
}

.expand-enter-from,
.expand-leave-to {
    opacity: 0;
    transform: translateY(-10px);
}

.icon img {
    filter: invert(100%);
}

.sub-item p {
    margin-top: 0;
    padding-top: 0;
    position: relative;
    top: -5px;
}

@media (max-width: 768px) {
    .timeline {
        display: flex;
        align-items: flex-start;
        max-width: 100%;
        margin: 50px auto;
        padding: 30px;
        background-color: #f8f8f8;
        border-radius: 10px;
        box-shadow: 0 4px 10px rgba(0, 0, 0, 0.1);
    }

    .epoch-content {
        background-color: #fff;
        padding: 20px 25px;
        border-radius: 8px;
        height: auto;
        box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
        width: 100%;
    }
}
</style>
