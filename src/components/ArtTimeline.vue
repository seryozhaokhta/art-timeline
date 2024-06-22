<!-- components/ArtTimeline.vue -->

<template>
    <div class="timeline" @click.self="closeAllEpochs">
        <VerticalSlider @positionChange="handlePositionChange" class="slider" />
        <div class="epochs-container">
            <div v-for="(epoch, index) in data['Western-European-art-periodization']" :key="epoch.id" class="epoch">
                <div class="epoch-line">
                    <div class="circle"></div>
                    <div class="line"></div>
                </div>
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
                                            <img :src="require(`@/assets/${subItem.subperiodsIcon.replace('/src/assets/', '')}`)"
                                                alt="Subperiods Icon">
                                        </span>
                                        <span v-if="subItem.crisisIcon" class="icon crisis-icon"
                                            :class="{ rotated: subItem.expanded }">
                                            <img :src="require(`@/assets/${subItem.crisisIcon.replace('/src/assets/', '')}`)"
                                                alt="Crisis Icon">
                                        </span>
                                        <span v-if="subItem.transitionIcon" class="icon transition-icon"
                                            :class="{ rotated: subItem.expanded }">
                                            <img :src="require(`@/assets/${subItem.transitionIcon.replace('/src/assets/', '')}`)"
                                                alt="Transition Icon">
                                        </span>
                                        <span v-if="subItem.schoolsIcon" class="icon schools-icon"
                                            :class="{ rotated: subItem.expanded }">
                                            <img :src="require(`@/assets/${subItem.schoolsIcon.replace('/src/assets/', '')}`)"
                                                alt="Schools Icon">
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
        };
    },
    created() {
        this.initializeExpansionStates();
    },
    methods: {
        handlePositionChange(position) {
            const totalEpochs = this.data['Western-European-art-periodization'].length;
            const index = Math.floor(position * totalEpochs / 100);
            this.activeEpochIndex = index;
            this.activeTag = null; // Reset active tag when the slider changes
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
            epoch.expanded = !epoch.expanded;
            if (!epoch.expanded) {
                this.activeEpochIndex = null;
                this.activeTag = null;
            } else {
                this.activeEpochIndex = index;
            }
        },
        toggleNestedExpand(item) {
            item.expanded = !item.expanded;
        },
        changePeriodization(tag, epoch) {
            this.activeTag = tag;
            // Проверка на существование тегов в эпохе
            if (epoch.tags && epoch.tags.includes(tag)) {
                // Если теги существуют и содержат выбранный тег, то раскрываем эпоху
                epoch.expanded = true;
                // Проверка на существование subItems перед итерацией
                if (epoch.subItems && Array.isArray(epoch.subItems)) {
                    epoch.subItems.forEach(subItem => {
                        // Раскрытие всех subItems
                        subItem.expanded = true;
                    });
                }
            }

            // Дополнительная проверка для случая, когда разделение по конкретному тегу
            // (например, у эпохи "Возрождение" могут быть разные subItems для разных стран)
            // Необходимо проверить, действительно ли существуют данные по этому тегу в эпохе
            if (epoch[this.activeTag] && Array.isArray(epoch[this.activeTag])) {
                epoch[this.activeTag].forEach(tagSpecificSubItem => {
                    // Раскрытие всех соответствующих subItems
                    tagSpecificSubItem.expanded = true;
                });
            }
        },
        getActiveSubItems(epoch) {
            if (this.activeTag && epoch[this.activeTag]) {
                return epoch[this.activeTag];
            }
            return epoch.subItems;
        }
    }
};
</script>

<style>
.timeline {
    display: flex;
    align-items: flex-start;
    width: 80%;
    margin: 50px auto;
    padding: 30px;
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
    margin-bottom: 30px;
    display: flex;
}

.epoch-line {
    width: 20px;
    display: flex;
    flex-direction: column;
    align-items: left;
    margin-right: 10px;
}

.circle {
    width: 12px;
    height: 12px;
    border-radius: 50%;
    background-color: #ddd;
    margin: auto 0;
    transform: translateX(-50%);
    margin-bottom: 5px;
}

.line {
    width: 1px;
    flex-grow: 1;
    background-color: #ddd;
}

.epoch-content {
    background-color: #fff;
    padding: 20px 25px;
    border-radius: 8px;
    height: auto;
    box-shadow: 0 2px 5px rgba(0, 0, 0, 0.05);
}

.epoch-header {
    text-align: left;
    cursor: pointer;
    transition: background-color 0.3s ease;
}

.epoch-header:hover {
    background-color: transparent;
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
    margin-bottom: 20px;
}

.sub-item h3 {
    margin: 0;
    cursor: pointer;
    display: flex;
    align-items: center;
}

.sub-item .icon {
    margin-left: 10px;
    transition: transform 0.3s ease;
}

.rotated {
    transform: rotate(180deg);
}

.nested-sub-items {
    margin-top: 10px;
}

.nested-sub-items li {
    margin-bottom: 10px;
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
</style>
