<template>
  <div class="card">
    <div class="card-header">
      Users
      <router-link class="btn btn-primary" style="float: right;" to="/users/add">Add User</router-link>
    </div>
    <div class="card-body">
      <div class="table-responsive">
        <table class="table" v-if="users && users.length">
          <thead>
            <tr>
              <th>User Name</th>
              <th>Email</th>
              <th></th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="user in users" :key="user.id">
              <td>
                <router-link :to="'/users/'+ user.id">{{ user.userName }}</router-link>
              </td>
              <td>{{ user.email }}</td>
              <td>
                <router-link class="btn btn-primary" :to="'/users/edit/'+ user.id">Edit</router-link>&nbsp;
                <button
                  type="button"
                  class="btn btn-primary btn-danger"
                  @click="deleteUser(user)"
                >Delete</button>
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
    <div v-if="errorMessage" class="alert alert-danger">Error: {{ errorMessage }}</div>
    <b-modal id="modal-delete" title="Delete User" @ok="deleteConfirmed">
      <p class="my-4">
        Are you sure you want to delete
        <strong>{{ selectedUser.userName }}</strong>
      </p>
    </b-modal>
  </div>
</template>

<script>
import axios from "./axios";

export default {
  data() {
    return {
      users: [],
      selectedUser: {},
      errorMessage: "",
    };
  },
  computed: {},
  methods: {
    loadUsers() {
      axios.get("").then((rs) => {
        this.users = rs.data;
      });
    },
    deleteUser(user) {
      this.selectedUser = user;
      this.$bvModal.show("modal-delete");
    },
    deleteConfirmed() {
      axios.delete(this.selectedUser.id).then((rs) => {
        this.loadUsers();
      });
    },
  },
  components: {},
  filters: {
    lowercase: function (value) {
      return value.toLowerCase();
    },
    formatedDateTime: function (value) {
      if (!value) return value;
      var date = new Date(value);
      return date.toLocaleDateString() + " " + date.toLocaleTimeString();
    },
  },
  created() {
    this.loadUsers();
  },
};
</script>

<style scoped>
thead {
  color: #337ab7;
}
</style>