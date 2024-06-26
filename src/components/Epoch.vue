<!-- components/Epoch.vue -->

<template>
    <div class="epoch">
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
                    <SubItems :subItems="getActiveSubItems(epoch)" @toggleNestedExpand="toggleNestedExpand" />
                </div>
            </transition>
        </div>
    </div>
</template>

<script>
import SubItems from './SubItems.vue';

export default {
    name: 'ArtEpoch',
    props: {
        epoch: Object,
        index: Number,
        activeTag: String,
    },
    components: {
        SubItems,
    },
    methods: {
        toggleExpand(epoch, index) {
            this.$emit('toggleExpand', epoch, index);
        },
        changePeriodization(tag, epoch) {
            this.$emit('changePeriodization', tag, epoch);
        },
        getActiveSubItems(epoch) {
            if (this.activeTag && epoch[this.activeTag]) {
                return epoch[this.activeTag];
            }
            return epoch.subItems;
        },
    },
};
</script>

<style scoped>
.epoch {
    width: 100%;
    margin-bottom: 5px;
    display: flex;
}

.epoch-content {
    background-color: transparent;
    padding: 5px 5px;
    border-bottom: 1px solid grey;
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

.active-epoch .sub-items {
    max-height: var(--sub-items-max-height);
}
</style>
