<!-- components/ArtTimeline.vue -->
<template>
    <div class="timeline" @click.self="closeAllEpochs">
        <VerticalSlider @positionChange="handlePositionChange" class="slider" :targetPosition="sliderPosition" />
        <div class="epochs-container">
            <div v-for="(epoch, index) in data['Western-European-art-periodization']" :key="epoch.id" class="epoch">
                <div class="epoch-content" :class="{ 'active-epoch': epoch.expanded }">
                    <div class="epoch-header" @click.stop="toggleExpand(epoch, index)">
                        <h2>{{ epoch.epoch }}</h2>
                    </div>
                    <transition name="expand">
                        <div v-show="epoch.expanded" class="sub-items">
                            <div v-if="epoch.tags" class="tags">
                                <span v-for="tag in epoch.tags" :key="tag" class="tag"
                                    @click.stop="changePeriodization(tag, epoch)">
                                    {{ tag }}
                                </span>
                            </div>
                            <div class="sub-items-content">
                                <div v-for="subItem in getActiveSubItems(epoch)" :key="subItem.title" class="sub-item">
                                    <h3 @click.stop="toggleNestedExpand(subItem)">
                                        {{ subItem.title }}
                                        <span v-if="subItem.subperiodsIcon" class="icon subperiods-icon"
                                            :class="{ rotated: subItem.expanded }">
                                            <img :src="getIconPath('subperiodsIcon.svg')" alt="Subperiods Icon">
                                        </span>
                                        <span v-if="subItem.crisisIcon" class="icon crisis-icon"
                                            :class="{ rotated: subItem.expanded }">
                                            <img :src="getIconPath('crisisIcon.svg')" alt="Crisis Icon">
                                        </span>
                                        <span v-if="subItem.transitionIcon" class="icon transition-icon"
                                            :class="{ rotated: subItem.expanded }">
                                            <img :src="getIconPath('transitionIcon.svg')" alt="Transition Icon">
                                        </span>
                                        <span v-if="subItem.schoolsIcon" class="icon schools-icon"
                                            :class="{ rotated: subItem.expanded }">
                                            <img :src="getIconPath('schoolsIcon.svg')" alt="Schools Icon">
                                        </span>
                                    </h3>
                                    <p v-if="subItem.dates">{{ subItem.dates }}</p>
                                    <p :class="['trend', subItem.trend]">{{ subItem.trend }}</p>
                                    <transition name="expand">
                                        <ul v-if="subItem.expandable && subItem.expanded" class="nested-sub-items">
                                            <li v-for="nestedItem in subItem[subItem.type]" :key="nestedItem.title">
                                                <h3 @click.stop="toggleNestedExpand(nestedItem)">{{ nestedItem.title }}
                                                </h3>
                                                <p v-if="nestedItem.dates">{{ nestedItem.dates }}</p>
                                                <p :class="['trend', nestedItem.trend]">{{ nestedItem.trend }}</p>
                                            </li>
                                        </ul>
                                    </transition>
                                </div>
                            </div>
                        </div>
                    </transition>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
import VerticalSlider from './VerticalSlider.vue';
import subperiodsIcon from '@/assets/subperiodsIcon.svg';
import crisisIcon from '@/assets/crisisIcon.svg';
import transitionIcon from '@/assets/transitionIcon.svg';
import schoolsIcon from '@/assets/schoolsIcon.svg';

export default {
    name: 'ArtTimeline',
    components: {
        VerticalSlider
    },
    props: {
        data: Object,
    },
    data() {
        return {
            activeTag: null,
            activeEpoch: null,
            activeEpochIndex: null,
            sliderPosition: 100, // Новая переменная для хранения позиции слайдера
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
        toggleNestedExpand(item) {
            item.expanded = !item.expanded;
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
        getActiveSubItems(epoch) {
            if (this.activeTag && epoch[this.activeTag]) {
                return epoch[this.activeTag];
            }
            return epoch.subItems;
        },
        getIconPath(fileName) {
            switch (fileName) {
                case 'subperiodsIcon.svg':
                    return subperiodsIcon;
                case 'crisisIcon.svg':
                    return crisisIcon;
                case 'transitionIcon.svg':
                    return transitionIcon;
                case 'schoolsIcon.svg':
                    return schoolsIcon;
                default:
                    return '';
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
        }
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
