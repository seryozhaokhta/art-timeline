<!-- components/ArtTimeline.vue -->

<template>
    <div class="timeline-container">
        <h1 class="timeline-title">Western European Art History</h1>
        <div class="timeline" @click.self="closeAllEpochs">
            <VerticalSlider @positionChange="handlePositionChange" class="slider" :targetPosition="sliderPosition" />
            <div class="epochs-container">
                <ArtEpoch v-for="(epoch, index) in data['Western-European-art-periodization']" :key="epoch.id"
                    :epoch="epoch" :index="index" :activeTag="activeTag" @toggleExpand="toggleExpand"
                    @changePeriodization="changePeriodization" />
            </div>
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
.timeline-container {
    max-width: 25%;
    margin: 0 auto;
    padding: 20px;
    background-color: #fff;
    box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.timeline-title {
    font-size: 30px;
    font-weight: normal;
    margin-bottom: 30px;
    text-align: center;
    color: #bebebe;
    padding-bottom: 10px;
    border: none;
}

.timeline {
    display: flex;
    align-items: flex-start;
    max-width: 100%;
    background-color: transparent;
    border-radius: 10px;
}

.epochs-container {
    width: 100%;
    display: flex;
    flex-direction: column;
    align-items: flex-start;
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

@media (max-width: 768px) {
    .timeline {
        display: flex;
        align-items: flex-start;
        width: 100%;
        background-color: transparent;
    }

    .timeline-container {
        max-width: 100%;
    }

    .epoch-content {
        background-color: #fff;
        border-radius: 0;
        height: auto;
        width: 100%;
    }
}
</style>
