<template>
  <div class="login-form container">
    <div class="alert alert-danger fade" role="alert" :class="{'show':alert}">
      {{alertMsg}}
    </div>
    <div>
      <h2 class="text-center">{{loginBtn ? "Login" : "Register"}}</h2>
      <div class="form-group">
        <input v-model="username" type="text" class="form-control" placeholder="Username" required="required">
      </div>
      <div class="form-group">
        <input v-model="pwd" type="password" class="form-control" placeholder="Password" required="required">
      </div>
      <div class="form-group" v-if="!loginBtn">
        <input v-model="name" type="text" class="form-control" placeholder="Name" required="required">
      </div>
      <div class="form-group">
        <button @click="btnFunc" type="submit" class="btn btn-primary btn-block">{{loginBtn ? "Login" : "Register"}}
        </button>
      </div>
      <div class="clearfix">
        <label class="float-left form-check-label"><input type="checkbox"> Remember me</label>
      </div>
    </div>
    <p class="text-center"><a href="#" v-if="loginBtn" @click="loginBtn = !loginBtn">Create an Account</a></p>
  </div>
</template>

<script>
    import axios from 'axios'
    import Cookies from 'js-cookie'


    export default {
        name: "Login",
        data: function () {
            return {
                loginBtn: true,
                username: '',
                pwd: '',
                name: '',
                alert: false,
                alertMsg: ''
            }
        },
        methods: {
            btnFunc: function () {
                const vm = this
                const url = window.apiBase;
                const {username, pwd, name} = this
                console.log({url, username, pwd})
                if (this.loginBtn) {
                    // Do login
                    axios.get(`${url}/login`, {
                        params: {
                            user: username,
                            password: pwd
                        }
                    })
                        .then(function (response) {
                            vm.alert = false
                            console.log('response', response);
                            const {data} = response
                            const {id} = data
                            Cookies.set("id", id)
                            window.location.reload()

                        })
                        .catch(function (error) {
                            if (error && error.response) {
                                vm.alert = true

                                switch (error.response.status) {
                                    case 400:
                                        vm.alertMsg = "Wrong Username / Password";
                                        break
                                    default:
                                        vm.alertMsg = "Server Error"

                                }
                                console.log('error', error.response.status);
                            }

                        })
                        .then(function () {
                            // always executed
                        });

                } else {
                    axios.post(`${url}/user`, null, {
                        params: {
                            user: username,
                            password: pwd,
                            name: name
                        }
                    })
                        .then(function (response) {
                            vm.alert = false
                            console.log('response', response);
                            window.location.reload()
                        })
                        .catch(function (error) {
                            if (error) {
                                vm.alert = true
                                vm.alertMsg = "Please try other data"
                            }
                        })
                        .then(function () {
                            // always executed
                        });

                }
            }
        }
    }
</script>

<style scoped>

</style>
