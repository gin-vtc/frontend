<template>
  <div id="app">
    <div v-if="!login">
      <Login />
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

        <div class="col" @click="">
          <div class="btn btn-danger">Logout</div>
        </div>
      </div>
      <hr />
      <div v-if="newStatusBlock">
        <NewStatus />
      </div>
      <div v-if="changePwdBlock">
        Change Pwd
      </div>
      <Status />
    </div>
    <!--    <img src="./assets/logo.png">-->
    <!--    <HelloWorld/>-->
  </div>
</template>

<script>
import Login from "./components/Login";
import Status from "./components/Status";
import NewStatus from "./components/NewStatus";
import Cookies from "js-cookie";

export default {
  name: "App",
  components: {
    Login,
    Status,
    NewStatus
  },
  data: function() {
    return {
      login: false,
      id: null,
      newStatusBlock: false,
      changePwdBlock: false
    };
  },
  mounted() {
    // Check Login
    const id = Cookies.get("id");
    if (id) {
      this.login = true;
      this.id = id;
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
