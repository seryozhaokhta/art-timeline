<!-- components/AppHeader.vue -->

<template>
    <header class="app-header">
        <img src="@/assets/logo.svg" alt="Логотип" class="logo">
        <button class="burger-button" @click="toggleMenu">
            <span :class="{ 'active': isMenuOpen }"></span>
            <span :class="{ 'active': isMenuOpen }"></span>
            <span :class="{ 'active': isMenuOpen }"></span>
        </button>
        <nav class="menu" v-if="isMenuOpen">
            <ul>
                <li><a href="#">Пункт 1</a></li>
                <li><a href="#">Пункт 2</a></li>
                <li><a href="#">Пункт 3</a></li>
            </ul>
        </nav>
    </header>
</template>

<script>
export default {
    name: 'AppHeader',
    data() {
        return {
            isMenuOpen: false
        };
    },
    methods: {
        toggleMenu() {
            this.isMenuOpen = !this.isMenuOpen;
        }
    }
};
</script>

<style scoped>
.app-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    padding: 1rem;
    background-color: #f8f8f8;
    position: relative;
}

.logo {
    height: 3rem;
}

.burger-button {
    background: none;
    border: none;
    cursor: pointer;
    padding: 0.5rem;
}

.burger-button span {
    display: block;
    width: 2rem;
    height: 0.2rem;
    background-color: #333;
    margin: 0.3rem 0;
    transition: 0.3s ease-in-out;
}

.burger-button span.active:nth-child(1) {
    transform: translateY(8px) rotate(45deg);
}

.burger-button span.active:nth-child(2) {
    opacity: 0;
}

.burger-button span.active:nth-child(3) {
    transform: translateY(-8px) rotate(-45deg);
}

.menu {
    position: absolute;
    top: 100%;
    left: 0;
    width: 100%;
    background-color: #fff;
    box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
    z-index: 1;
}

.menu ul {
    list-style: none;
    padding: 0;
    margin: 0;
}

.menu li {
    padding: 1rem 1.5rem;
    border-bottom: 1px solid #eee;
}

.menu a {
    text-decoration: none;
    color: #333;
}
</style>