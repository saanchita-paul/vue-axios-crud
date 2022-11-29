<template>
  <div>
    <button @click="getUsers">Get users</button> <br />
  </div>
    <div class="users" v-for="user in users" :key="user.id">
      <h2>{{ user.title }}</h2>
      <!-- <p>{{ user.email }}</p> -->
    </div>
  <div class="user-add">
    <h2>Add user</h2>
    <input type="text" v-model="title" /> <br />
    <!-- <input type="text" v-model="email" /> <br /> -->
    <button @click="storeUser">Store</button>
  </div>
  <div class="user-update">
    <h2>Update user</h2>
    <input type="text" v-model="user.title" /> <br />
    <!-- <input type="text" v-model="user.email" /> <br /> -->
    <button @click="updateUser">Update</button>
  </div>
  <div class="users" v-for="user in users" :key="user.id">
    <h2>{{ user.title }}</h2>
    <!-- <p>{{ user.email }}</p> -->

    <button @click="editUser(user)">Edit</button>
    <button @click="deleteUser(user.id)">Delete</button>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "App",
  data() {
    return {
      users: [],
      user: {},
      // email: '',
      title: '',
    };
  },
  methods: {
    async getUsers() {
      try {
        const users = await axios.get(
          "http://127.0.0.1:8000/api/list"
        );

        this.users = users.data;
        console.log(this.users);
      } catch (e) {
        console.log(e);
      }
    },
    async storeUser() {
      try {
        const user = await axios.post(
          "http://127.0.0.1:8000/api/create", 
          {
            title: this.title,
            // email : this.email
          }
        );

        console.log(user)
      } catch(e) {
        console.log(e);
      }
    },
    async updateUser() {
      try {
        const user = await axios.put(
          "http://127.0.0.1:8000/api/update/" + this.user.id,
          {
            title: this.user.title,
            // email: this.user.email,
          }
        );

        console.log(user.data);
        alert("User updated!");
      } catch (e) {
        console.log(e);
      }
    },
    async editUser(user) {
      this.user.title = user.title;
      // this.user.email = user.email;
      this.user.id = user.id;
    },
    async deleteUser(id) {
      let x = window.confirm("You want to delete the user?");

      if (x) {
        const user = await axios.delete(
          "http://127.0.0.1:8000/api/destroy/" + id
        );

        console.log(user);
        alert("User deleted!");
      }
    },
  },
};
</script>