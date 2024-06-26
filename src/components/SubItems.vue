<!-- components/SubItems.vue -->

<template>
    <div class="sub-items-content">
        <div v-for="subItem in subItems" :key="subItem.title" class="sub-item">
            <h3 @click.stop="toggleNestedExpand(subItem)">
                {{ subItem.title }}
                <span v-if="subItem.subperiodsIcon" class="icon subperiods-icon" :class="{ rotated: subItem.expanded }">
                    <img :src="getIconPath('subperiodsIcon.svg')" alt="Subperiods Icon">
                </span>
                <span v-if="subItem.crisisIcon" class="icon crisis-icon" :class="{ rotated: subItem.expanded }">
                    <img :src="getIconPath('crisisIcon.svg')" alt="Crisis Icon">
                </span>
                <span v-if="subItem.transitionIcon" class="icon transition-icon" :class="{ rotated: subItem.expanded }">
                    <img :src="getIconPath('transitionIcon.svg')" alt="Transition Icon">
                </span>
                <span v-if="subItem.schoolsIcon" class="icon schools-icon" :class="{ rotated: subItem.expanded }">
                    <img :src="getIconPath('schoolsIcon.svg')" alt="Schools Icon">
                </span>
            </h3>
            <p v-if="subItem.dates">{{ subItem.dates }}</p>
            <p :class="['trend', subItem.trend]">{{ subItem.trend }}</p>
            <transition name="expand">
                <ul v-if="subItem.expandable && subItem.expanded" class="nested-sub-items">
                    <li v-for="nestedItem in subItem[subItem.type]" :key="nestedItem.title">
                        <h3 @click.stop="toggleNestedExpand(nestedItem)">{{ nestedItem.title }}</h3>
                        <p v-if="nestedItem.dates">{{ nestedItem.dates }}</p>
                        <p :class="['trend', nestedItem.trend]">{{ nestedItem.trend }}</p>
                    </li>
                </ul>
            </transition>
        </div>
    </div>
</template>

<script>
export default {
    name: 'SubItems',
    props: {
        subItems: Array,
    },
    methods: {
        toggleNestedExpand(item) {
            item.expanded = !item.expanded;
            this.$emit('toggleNestedExpand', item);
        },
        getIconPath(fileName) {
            switch (fileName) {
                case 'subperiodsIcon.svg':
                    return require('@/assets/subperiodsIcon.svg');
                case 'crisisIcon.svg':
                    return require('@/assets/crisisIcon.svg');
                case 'transitionIcon.svg':
                    return require('@/assets/transitionIcon.svg');
                case 'schoolsIcon.svg':
                    return require('@/assets/schoolsIcon.svg');
                default:
                    return '';
            }
        },
    },
};
</script>

<style scoped>
.sub-items {
    overflow: hidden;
    transition: max-height 0.3s ease;
    max-height: 0;
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
    margin-top: -10px;
}

.nested-sub-items li {
    margin-bottom: -10px;
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

</style>
