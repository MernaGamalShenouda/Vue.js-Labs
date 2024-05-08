<template>
  <div class="container">
    <!-- Form section -->
    <div class="mb-4">
      <h1 class="text-center mt-5">Users Form</h1>
      <form @submit.prevent="submitForm" class="mt-4">
        <div class="mb-3 row justify-content-center">
          <div class="col-md-4">
            <label for="name" class="col-form-label text-end">Name:</label>
            <input type="text" id="name" v-model="name" class="form-control" required>
          </div>
          <div class="col-md-4">
            <label for="age" class="col-form-label text-end">Age:</label>
            <input type="number" id="age" v-model.number="age" class="form-control" required>
          </div>
        </div>
        <div class="mb-3 row justify-content-center">
          <div class="col-md-4">
            <label for="role" class="col-form-label text-end">Role:</label>
            <select id="role" v-model="role" class="form-select" required>
              <option value="admin">Admin</option>
              <option value="user">User</option>
            </select>
          </div>
        </div>
        <div class="text-center">
          <button type="submit" class="btn btn-primary">Submit</button>
        </div>
      </form>
    </div>

    <!-- Buttons section -->
    <div class="d-flex justify-content-center mb-4">
      <button @click="flag = 'UsersList'" class="btn btn-primary me-2">Users</button>
      <button @click="flag = 'AdminsList'" class="btn btn-primary">Admins</button>
    </div>

    <!-- Lists section -->
    <div>
      <UsersList v-if="flag === 'UsersList'" :users="users" @delete="deleteUser" />
      <AdminsList v-else-if="flag === 'AdminsList'" :admins="admins" @delete="deleteAdmin"/>
    </div>
  </div>
</template>

<script>
import UsersList from "./UsersList"
import AdminsList from "./AdminsList"

export default {
  name: "HomePage",
  data() {
    return {
      name: '',
      age: null,
      role: 'user',
      flag: 'UsersList',
      users: [],
      admins: []
    };
  },
  components: {
    UsersList,
    AdminsList
  },
  methods: {
    submitForm() {
      console.log("submitUsers:", this.users);

      if (this.role === 'admin') {
        this.admins.push({ name: this.name, age: this.age });
      } else {
        this.users.push({ name: this.name, age: this.age });
      }

      this.name = '';
      this.age = null;
      this.role = 'user';
    },

    deleteUser(index) {
      this.users.splice(index, 1);
      console.log("DeleteUsers:", this.users);
    },

    deleteAdmin(index) {
      this.admins.splice(index, 1);
      console.log("DeleteAdmins:", this.admins);
    }
  }
};
</script>

<style scoped>
</style>
