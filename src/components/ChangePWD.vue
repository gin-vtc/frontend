<template>
  <div class="container">

    <div class="row">
      <div class="col-12">

        <div class="card my-4 fade show">
          <div class="card-header">
            Change Password
          </div>
          <div class="card-body">

            <div class="form-group">
              <input v-model="username" type="text" class="form-control" placeholder="Current Username" required="required">
            </div>

            <div class="form-group">
              <input v-model="oldPwd" type="password" class="form-control" placeholder="Old Password"
                     required="required">
            </div>

            <div class="form-group">
              <input v-model="newPwd" type="password" class="form-control" placeholder="New Password"
                     required="required">
            </div>

            <div class="form-group">
              <input v-model="name" type="text" class="form-control" placeholder="New Name" required="required">
            </div>

            <div class="form-group">
              <button @click="btnFunc" type="submit" class="btn btn-primary btn-block">
                Change
              </button>
            </div>
          </div>

        </div>
      </div>
    </div>
  </div>
</template>

<script>
    import axios from 'axios'

    export default {
        name: "ChangePWD",
        data: () => ({
            username: '',
            oldPwd: '',
            newPwd: '',
            name: ''
        }),
        methods: {
            btnFunc: function () {
                const {username, oldPwd, newPwd, name} = this
                // console.log({username,oldPwd,newPwd,name})
                const paramsObj = {user: username, password: oldPwd, newPwd};
                if (name !== "") {
                    paramsObj.newName = name
                }
                axios.put(window.apiBase + '/user',null, {params: paramsObj}).then(r => {
                    window.alert("done")
                    document.cookie.split(";").forEach(function (c) {
                        document.cookie = c.replace(/^ +/, "").replace(/=.*/, "=;expires=" + new Date().toUTCString() + ";path=/");
                    });
                    window.location.reload()
                }).catch(e=>{
                    alert("Wrong Credential")
                })
            }
        }
    }
</script>

<style scoped>

</style>
