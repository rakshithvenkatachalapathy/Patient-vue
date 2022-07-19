<template>
  <body>
    <nav class="navbar navbar-dark bg-dark">
      <!-- <span class="navbar-brand mb-0 h1">Navbar</span> -->
      <button class="text-dark" v-on:click="loadHome()">Home</button>
    </nav>
    <div class="container">
      <h3>Employee Database</h3>
      <form>
        <div class="input-group" v-show="showTable">
          <div class="col-md-12">
            <h2>Please use the search box to search by Employee name!</h2>
          </div>
          <div class="w-50 p-3">
            <label for="fname">First Name</label>
            <input
              type="search"
              class="form-control rounded"
              placeholder="First Name"
              aria-label="Search"
              aria-describedby="search-addon"
              ref="groupId1"
              v-model="firstName"
            />
            <!-- </div>
          <div class="w-50 p-3"> -->
            <label for="fname">Last Name</label>
            <input
              type="search"
              class="form-control rounded"
              placeholder="Last Name"
              aria-label="Search"
              aria-describedby="search-addon"
              ref="groupId2"
              v-model="lastName"
            />
            <!-- </div>
          <div class="w-50 p-3"> -->
            <button
              type="button"
              class="btn btn-outline-primary"
              v-on:click="searchText()"
            >
              Search
            </button>
            <button
              type="button"
              class="btn btn-outline-primary"
              v-on:click="searchText()"
            >
              Update
            </button>
          </div>
        </div>
      </form>
      <div>
        <h3 v-show="showNotFound" class="text-danger">
          No employees found with the name provided
        </h3>
      </div>
      <div v-show="showTable">
        <h2>Employees in the database:</h2>
        <table class="table">
          <thead>
            <tr>
              <th scope="col">ID</th>
              <th scope="col">First Name</th>
              <th scope="col">Last Name</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="user in users" v-bind:key="user.id">
              <th scope="row">{{ user.id }}</th>
              <td>{{ user.firstName }}</td>
              <td>{{ user.lastName }}</td>
            </tr>
          </tbody>
        </table>
      </div>

      <div v-show="!showTable">
        <h2>Search Results are:</h2>
        <table class="table">
          <thead>
            <tr>
              <th scope="col">ID</th>
              <th scope="col">First Name</th>
              <th scope="col">Last Name</th>
              <th scope="col">Address</th>
              <th scope="col">Date of joining</th>
              <th scope="col">Contact Numbers</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="user in users" v-bind:key="user.id">
              <th scope="row">{{ user.id }}</th>
              <td>{{ user.firstName }}</td>
              <td>{{ user.lastName }}</td>
              <td>{{ user.address }}</td>
              <td>{{ user.dateOfJoining }}</td>
              <td>
                {{
                  user.numbers
                    .map((item) => item.type + ":" + item.phoneNumber)
                    .join(", ")
                }}
              </td>
            </tr>
          </tbody>
        </table>
      </div>
    </div>
  </body>
</template>
<script>
import axios from "axios";

export default {
  name: "Users",

  data() {
    return {
      users: null,
      showTable: true,
      showNotFound: false,
    };
  },
  created: function () {
    this.loadHome();
  },
  methods: {
    loadHome() {
      console.log("inside return");
      axios.get("http://localhost:8080/api/employees").then((res) => {
        this.users = res.data;
        this.showTable = true;
        this.showNotFound = false;
      });
    },
    searchText() {
      axios
        .get(
          "http://localhost:8080/api/employees/" +
            this.$refs.groupId1.value +
            "/" +
            this.$refs.groupId2.value
        )
        .then((res) => {
          this.users = res.data;
          if (this.users.length === 0) {
            axios.get("http://localhost:8080/api/employees").then((res) => {
              this.users = res.data;
            });
            this.showTable = true;
            this.showNotFound = true;
          } else {
            this.showNotFound = false;
            this.showTable = false;
          }
        });
    },
  },
};
</script>
<style>
h3 {
  margin-bottom: 5%;
}
div {
  padding: 0.1%;
}
button {
  margin-top: 1%;
  padding: 0.5%;
}
input {
  margin-bottom: 1%;
}
nav {
  margin-top: 1%;
}
</style>