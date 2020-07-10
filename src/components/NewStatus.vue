<template>
  <div>
    <input type="text" v-model="status" placeholder="status" />
    <button @click="send">Go</button>
  </div>
</template>

<script>
import axios from "axios";
import Cookies from "js-cookie";

export default {
  name: "Status.vue",
  data: function() {
    return {
      status: null
    };
  },
  mounted() {
    const vm = this;
    const url = window.apiBase;
    axios.get(`${url}/userlist`).then(function(response) {
      vm.userList = response.data;
    });
    console.log(this.userLiseiwt);
  },
  methods: {
    send: function() {
      const vm = this;
      console.log(vm.status)
      if (vm.status) {
        axios
          .post(`${window.apiBase}/status`, null, {
            params: {
              status: vm.status,
              id: Cookies.get("id")
            }
          })
          .then(function(response) {
              window.location.reload()
          });
      }
    }
  }
};
</script>

<style scoped></style>
