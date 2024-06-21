<!-- components/AppFooter.vue -->

<template>
    <footer class="app-footer">
        <div class="container">
            <p class="copyright">© {{ new Date().getFullYear() }} </p>
        </div>
    </footer>
</template>

<style scoped>
.app-footer {
    background-color: #f8f8f8;
    color: #333;
    padding: 1rem 0;
}

.container {
    max-width: 960px;
    margin: 0 auto;
    text-align: center;
}

.copyright {
    font-size: 0.9rem;
}
</style>