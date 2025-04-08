<template>
    <nav class="navbar navbar-expand-lg navbar-dark bg-dark">
        <div class="container-fluid">
            <router-link class="navbar-brand" to="/">COMP3385</router-link>

            <button class="navbar-toggler" type="button" data-bs-toggle="collapse" data-bs-target="#navbarNav">
                <span class="navbar-toggler-icon"></span>
            </button>

            <div class="collapse navbar-collapse" id="navbarNav">
                <ul class="navbar-nav me-auto">
                    <li class="nav-item">
                        <router-link class="nav-link" to="/">Home</router-link>
                    </li>

                    <li v-if="isLoggedIn" class="nav-item">
                        <router-link class="nav-link" to="/movies">Movies</router-link>
                    </li>

                    <li v-if="isLoggedIn" class="nav-item">
                        <router-link class="nav-link" to="/movies/create">Add Movie</router-link>
                    </li>
                </ul>

                <ul class="navbar-nav">
                    <li v-if="isLoggedIn" class="nav-item">
                        <a href="#" class="nav-link" @click.prevent="logout">
                            <span v-if="loggingOut" class="spinner-border spinner-border-sm me-1" role="status" aria-hidden="true"></span>
                            Logout
                        </a>
                    </li>

                    <li v-else class="nav-item">
                        <router-link class="nav-link" to="/login">Login</router-link>
                    </li>
                </ul>
            </div>
        </div>
    </nav>
</template>

<script setup>
import { ref, computed } from 'vue';
import { useRouter } from 'vue-router';

const router = useRouter();
const loggingOut = ref(false);

const isLoggedIn = computed(() => {
    return localStorage.getItem('jwt_token') !== null;
});

const logout = () => {
    loggingOut.value = true;

    const token = localStorage.getItem('jwt_token');

    if (!token) {
        localStorage.removeItem('jwt_token');
        router.push('/login');
        return;
    }

    fetch('/api/v1/logout', {
        method: 'POST',
        headers: {
            'Accept': 'application/json',
            'Authorization': `Bearer ${token}`
        }
    })
        .then(response => {
            if (!response.ok && response.status !== 401) {
                throw new Error('Logout failed');
            }
            return response.json();
        })
        .then(() => {
            localStorage.removeItem('jwt_token');

            router.push('/login');
        })
        .catch(error => {
            console.error('Logout error:', error);


            localStorage.removeItem('jwt_token');
            router.push('/login');
        })
        .finally(() => {
            loggingOut.value = false;
        });
};
</script>
