<template>
  <div id="app">
    <div v-if="!login">
      <Login/>
    </div>
    <div v-else>

    </div>
    <!--    <img src="./assets/logo.png">-->
    <!--    <HelloWorld/>-->
  </div>
</template>

<script>
    import Login from './components/Login'
    import Cookies from 'js-cookie'

    export default {
        name: 'App',
        components: {
            Login
        },
        data: function () {
            return {
                login: false,
                id: null
            }
        },
        mounted() {
            // Check Login
            const id = Cookies.get("id")
            if (id) {
                this.login = true
                this.id = id
            }
        }
    }
</script>

<style>
  #app {
    font-family: 'Avenir', Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
