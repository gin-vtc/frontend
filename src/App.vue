<template>
  <div id="app">
    <div class="row">
      <div class="col">123</div>
      <div class="col">234</div>
    </div>
    <!--    <img src="./assets/logo.png">-->
    <!--    <HelloWorld/>-->
  </div>
</template>

<script>
    import HelloWorld from './components/HelloWorld'
    import Cookies from 'js-cookie'

    export default {
        name: 'App',
        components: {
            HelloWorld
        },
        data: {
            login: false,
            id: null
        },
        mounted() {
            // Check Login
            const id = cookies.get("id")
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
