<template>
    <Header />
    <main>
        <router-view />
    </main>
    <footer class="py-3 bg-light text-center mt-auto">
        <div class="container">
            <p class="text-muted">Copyright © 2024, COMP3385 Web Dev Superstars ✨</p>
        </div>
    </footer>
</template>

<script setup>
import Header from './components/Header.vue';
</script>

<style>
#app {
    display: flex;
    flex-direction: column;
    min-height: 100vh;
}

main {
    flex: 1;
}
</style>
