<template>
  <div id="app" class="fade" :class="{'show':health}">
    <div v-if="!login">
      <Login/>
    </div>
    <div v-else>
      <div class="row">
        <div class="col">
          <div
            class="btn btn-primary"
            @click="newStatusBlock = !newStatusBlock"
          >
            New Status
          </div>
        </div>

        <div class="col">
          <div
            class="btn btn-warning"
            @click="changePwdBlock = !changePwdBlock"
          >
            Change Password
          </div>
        </div>

        <div class="col" @click="logout">
          <div class="btn btn-danger">Logout</div>
        </div>
      </div>
      <hr/>
      <div v-if="newStatusBlock">
        <NewStatus/>
      </div>
      <div v-if="changePwdBlock">
        <ChangePWD />
      </div>
      <Status/>
    </div>
    <!--    <img src="./assets/logo.png">-->
    <!--    <HelloWorld/>-->
  </div>
</template>

<script>
    import Login from "./components/Login";
    import Status from "./components/Status";
    import NewStatus from "./components/NewStatus";
    import ChangePWD from "./components/ChangePWD";
    import Cookies from "js-cookie";
    import axios from 'axios'

    export default {
        name: "App",
        components: {
            Login,
            Status,
            NewStatus,
            ChangePWD
        },
        data: function () {
            return {
                login: false,
                id: null,
                newStatusBlock: false,
                changePwdBlock: false,
                health:false
            };
        },
        mounted() {
            // Check Login
            const vm = this
            const id = Cookies.get("id");
            if (id) {
                this.login = true;
                this.id = id;
            }

            axios.get(window.apiBase+'/').then(r=>{
                vm.health = true
            }).catch(function (error) {
               window.alert("API Error")
            })
        },
        methods: {
            logout: () => {
                document.cookie.split(";").forEach(function (c) {
                    document.cookie = c.replace(/^ +/, "").replace(/=.*/, "=;expires=" + new Date().toUTCString() + ";path=/");
                });
                window.location.reload()
            }
        }
    };
</script>

<style>
  #app {
    font-family: "Avenir", Helvetica, Arial, sans-serif;
    -webkit-font-smoothing: antialiased;
    -moz-osx-font-smoothing: grayscale;
    text-align: center;
    color: #2c3e50;
    margin-top: 60px;
  }
</style>
