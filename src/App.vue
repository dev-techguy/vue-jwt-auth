<template>
    <div id="app">
        <img alt="Vue logo" src="./assets/logo.png" style="width: 100px;">
        <Auth msg="Welcome to Your Vue.js App"/>
    </div>
</template>

<script>
    import Auth from './components/Auth.vue'

    export default {
        name: 'App',
        components: {
            Auth
        }
    }
</script>

<style>
    #app {
        font-family: Avenir, Helvetica, Arial, sans-serif;
        -webkit-font-smoothing: antialiased;
        -moz-osx-font-smoothing: grayscale;
        text-align: center;
        color: #2c3e50;
        margin-top: 60px;
    }

    @import '~bootstrap/dist/css/bootstrap.css';
</style>
