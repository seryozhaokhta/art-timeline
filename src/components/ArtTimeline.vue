<!-- components/ArtTimeline.vue -->

<template>
    <div class="timeline" @click.self="closeAllEpochs">
        <div class="epochs-container">
            <div v-for="epoch in data['Western-European-art-periodization']" :key="epoch.id" class="epoch">
                <div class="epoch-line">
                    <div class="circle"></div>
                    <div class="line"></div>
                </div>
                <div class="epoch-content">
                    <div class="epoch-header" @click="toggleExpand(epoch)">
                        <h2>{{ epoch.epoch }}</h2>
                    </div>
                    <transition name="expand">
                        <div v-show="epoch.expandable && epoch.expanded" class="sub-items">
                            <div v-if="epoch.tags" class="tags">
                                <span v-for="tag in epoch.tags" :key="tag" class="tag"
                                    @click.stop="changePeriodization(tag, epoch)">
                                    {{ tag }}
                                </span>
                            </div>
                            <div class="sub-items-content">
                                <div v-for="subItem in getActiveSubItems(epoch)" :key="subItem.title" class="sub-item">
                                    <h3 @click="toggleNestedExpand(subItem)">
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
                                                <h3 @click="toggleNestedExpand(nestedItem)">{{ nestedItem.title }}</h3>
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
export default {
    name: "ArtTimeline",
    props: {
        data: Object,
    },
    data() {
        return {
            activeTag: null,
            activeEpoch: null,
        };
    },
    created() {
        this.initializeExpansionStates();
    },
    mounted() {
        this.setTransitionHeight();
    },
    updated() {
        this.setTransitionHeight();
    },
    methods: {
        initializeExpansionStates() {
            this.data['Western-European-art-periodization'].forEach(epoch => {
                if (typeof epoch.expanded === 'undefined') {
                    this.$set(epoch, 'expanded', false);
                }
                if (epoch.subItems) {
                    epoch.subItems.forEach(subItem => {
                        if (typeof subItem.expanded === 'undefined') {
                            this.$set(subItem, 'expanded', false);
                        }
                        if (subItem[this.activeTag]) {
                            subItem[this.activeTag].forEach(nestedItem => {
                                if (typeof nestedItem.expanded === 'undefined') {
                                    this.$set(nestedItem, 'expanded', false);
                                }
                            });
                        }
                    });
                }
            });
        },
        setTransitionHeight() {
            this.$nextTick(() => {
                document.querySelectorAll('.sub-items').forEach(el => {
                    if (el.scrollHeight > 0) {
                        el.style.setProperty('--sub-items-max-height', `${el.scrollHeight}px`);
                    }
                });
            });
        },
        toggleExpand(item) {
            this.closeAllEpochs();
            item.expanded = !item.expanded;
        },
        closeAllEpochs() {
            this.data['Western-European-art-periodization'].forEach(epoch => {
                epoch.expanded = false;
            });
        },
        toggleNestedExpand(item) {
            item.expanded = !item.expanded;
        },
        changePeriodization(tag, epoch) {
            this.activeTag = tag;
            this.activeEpoch = epoch.id;
        },
        getActiveSubItems(epoch) {
            return this.activeTag && this.activeEpoch === epoch.id ? epoch[this.activeTag] : epoch.subItems;
        }
    },
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

.epoch-header h2 {
    font-size: 1.8rem;
    font-weight: bold;
    color: #333;
    margin: 0;
}

.expand-enter-active,
.expand-leave-active {
    transition: max-height 0.5s ease, opacity 0.5s ease;
}

.expand-enter,
.expand-leave-to

/* .expand-leave-active in <2.1.8 */
    {
    max-height: 0;
    opacity: 0;
    overflow: hidden;
}

.sub-items {
    text-align: left;
    margin-top: 20px;
    max-height: var(--sub-items-max-height);
    overflow: hidden;
}

.tags {
    margin-bottom: 10px;
}

.tag {
    display: inline-block;
    padding: 5px 10px;
    margin-right: 5px;
    border-radius: 5px;
    background-color: #f2f2f2;
    cursor: pointer;
}

.tag:hover {
    background-color: #ddd;
}

.sub-items-content .sub-item {
    border-radius: 6px;
    margin-bottom: 10px;
}

.sub-items-content .sub-item h3 {
    font-size: 1.1rem;
    font-weight: 600;
    margin: 0 0 5px 0;
}

.sub-items-content .sub-item .icon {
    display: inline-flex;
    align-items: center;
    margin-left: 5px;
    transition: transform 0.3s ease;
}

.sub-items-content .sub-item .icon img {
    width: 16px;
    height: auto;
    filter: invert(1);
}

.sub-items-content .sub-item .icon.rotated {
    transform: rotate(90deg);
}

.sub-items-content .sub-item p {
    margin: 0;
    font-size: 0.9rem;
    color: #555;
}

.sub-items-content .sub-item p.dates {
    color: #888;
}

.sub-items-content .sub-item .trend {
    font-weight: bold;
}

.sub-items-content .sub-item .trend.rise {
    color: green;
}

.sub-items-content .sub-item .trend.decline {
    color: red;
}

.nested-sub-items {
    list-style: none;
    padding: 0;
}

.nested-sub-items li {
    margin-left: 10px;
}

.nested-sub-items li h3 {
    cursor: pointer;
}
</style>
