<template>

</template>

<script>
  import axios from 'axios'
    export default {
        name: "Status.vue",
        data: function () {
            return {
                userList: null
            }
        },
        mounted(){
            const vm = this
            const url = window.apiBase;
            axios.get(`${url}/userlist`)
                .then(function (response) {
                   vm.userList = response.data
                })
            console.log(this.userList)
        }
    }
</script>

<style scoped>

</style>
