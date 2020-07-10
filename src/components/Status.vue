<template>
  <div class="container">
    <div class="row cm-list" :key="key">
      <div class="cm" v-for="cm in cmList" :key="cm.id">
        {{ cm.comment }}
        <hr />
        {{ new Date(x.time * 1000) }}
      </div>
    </div>
    <div class="row">
      <div class="col-4 my-4" v-for="x in statusList" :hidden="!x">
        <div class="card" v-if="x" :key="x.id">
          <div class="card-header">
            {{ findUser(x.user) }}
          </div>
          <div class="card-body">
            {{ x.status }}
            <hr />
            {{ new Date(x.time * 1000) }}
          </div>
          <div class="card-footer" v-if="x.user === user">
            <span class="btn btn-warning" @click="edit(x)">
              Edit
            </span>
            <span class="btn btn-danger" @click="deleteStatus(x)">
              Delete
            </span>
          </div>
          <div class="card-footer" v-if="x.user === user">
            <span class="btn btn-primary" @click="comment(x)">
              Comment
            </span>
            <span class="btn btn-success" @click="loadCm(x)">
              Load Comment
            </span>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Cookies from "js-cookie";

export default {
  name: "Status.vue",
  data: function() {
    return {
      userList: null,
      statusList: [],
      user: Cookies.get("id"),
      cmList: [],
      key:0
    };
  },
  mounted() {
    const vm = this;
    const url = window.apiBase;
    axios.get(`${url}/userlist`).then(function(response) {
      vm.userList = response.data;
    });
    axios.get(`${url}/statusList`).then(function(response) {
      vm.statusList = response.data;
    });
    console.log(this.statusList);
  },
  methods: {
    findUser: function(id) {
      for (let x in this.userList) {
        if (this.userList[x].id === id) {
          return this.userList[x].name;
        }
      }
      return "Unknown";
    },
    edit: function(x) {
      const { id, status } = x;
      var newStatus = prompt("Edit", status);
      if (newStatus != null) {
        axios
          .put(`${window.apiBase}/status`, null, {
            params: {
              status: newStatus,
              id: id
            }
          })
          .then(function(response) {
            window.location.reload();
          });
      }
    },
    comment: function(x) {
      const { id } = x;
      var comment = prompt("New Comment", comment);
      if (comment != null) {
        console.log({
          statusId: id,
          comment: comment
        });
        axios
          .post(`${window.apiBase}/comment`, null, {
            params: {
              statusId: id,
              comment: comment
            }
          })
          .then(function(response) {
            window.location.reload();
          });
      }
    },
    deleteStatus: function(x) {
      const { id, status } = x;

      axios
        .delete(`${window.apiBase}/status`, {
          params: {
            id: id
          }
        })
        .then(function(response) {
          window.location.reload();
        });
    },
    loadCm: function(x) {
      const { id } = x;
      axios
        .get(`${window.apiBase}/comment`, {
          params: {
            status: id
          }
        })
        .then(function(response) {
          this.cmList = response.data;
          this.key = Math.random()
        });

      console.log(this.cmList);
    }
  }
};
</script>

<style scoped></style>
