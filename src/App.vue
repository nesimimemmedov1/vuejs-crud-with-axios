<template>
  <div class="container-xl">
    <div class="table-responsive">
      <div class="table-wrapper">
        <table class="table table-success table-striped table-hover">
          <thead>
            <tr>
              <th>Index</th>
              <th>Name</th>
              <th>Username</th>
              <th>Email</th>
              <th>Phone</th>
              <th>Actions</th>
            </tr>
          </thead>
          <tbody>
            <tr v-for="(user, index) in users" :key="user.id">
              <td>{{ index + 1 }}</td>
              <td>{{ user.name }}</td>
              <td>{{ user.username }}</td>
              <td>{{ user.email }}</td>
              <td>{{ user.phone }}</td>
              <td class="actions">
                <a
                  @click="editUser(user)"
                  href="#"
                  class="edit"
                  data-toggle="modal"
                  type="button"
                  data-bs-toggle="modal"
                  data-bs-target="#staticBackdrop"
                  ><i class="fa-solid fa-pen"></i
                ></a>
                <a
                  @click="deleteUser(user)"
                  href="#"
                  class="delete"
                  data-toggle="modal"
                  ><i class="fa-solid fa-trash"></i
                ></a>
              </td>
            </tr>
          </tbody>
        </table>
        <div class="add-user">
          <div class="form-group">
            <label for="name">Name</label>
            <input v-model="userData.name" type="text" id="name" name="name" />
          </div>
          <div class="form-group">
            <label for="name">Username</label>
            <input
              v-model="userData.username"
              type="text"
              id="username"
              name="username"
            />
          </div>
          <div class="form-group">
            <label for="email">Name</label>
            <input
              v-model="userData.email"
              type="email"
              id="email"
              name="email"
            />
          </div>
          <div class="form-group">
            <label for="name">Phone</label>
            <input v-model="userData.phone" id="phone" name="phone" />
          </div>
          <button @click="addUser" class="btn btn-success">
            <i class="fa fa-plus" aria-hidden="true"></i>
          </button>
        </div>
      </div>
    </div>
    <div
      class="modal fade"
      id="staticBackdrop"
      data-bs-backdrop="static"
      data-bs-keyboard="false"
      tabindex="-1"
      aria-labelledby="staticBackdropLabel"
      aria-hidden="true"
    >
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <h5 class="modal-title" id="staticBackdropLabel">Update user</h5>
            <button
              type="button"
              class="btn-close"
              data-bs-dismiss="modal"
              aria-label="Close"
            ></button>
          </div>
          <div class="modal-body">
            <div class="row">
              <div class="col-lg-12">
                <div class="form-group">
                  <label for="name">Name</label>
                  <input
                    v-model="updateData.name"
                    type="text"
                    id="name"
                    name="name"
                  />
                </div>
              </div>
              <div class="col-lg-12">
                <div class="form-group">
                  <label for="name">Username</label>
                  <input
                    v-model="updateData.username"
                    type="text"
                    id="username"
                    name="username"
                  />
                </div>
              </div>
              <div class="col-lg-12">
                <div class="form-group">
                  <label for="email">Name</label>
                  <input
                    v-model="updateData.email"
                    type="email"
                    id="email"
                    name="email"
                  />
                </div>
              </div>
              <div class="col-lg-12">
                <div class="form-group">
                  <label for="name">Phone</label>
                  <input v-model="updateData.phone" id="phone" name="phone" />
                </div>
              </div>
            </div>
          </div>
          <div class="modal-footer">
            <button @click="updateUser" class="btn btn-warning">
              Update
            </button>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
export default {
  name: "app",
  data() {
    return {
      users: [],
      userData: {
        name: null,
        username: null,
        email: null,
        phone: null
      },
      updateData: {
        id: null,
        name: null,
        username: null,
        email: null,
        phone: null
      }
    };
  },
  created() {
    axios.get("https://jsonplaceholder.typicode.com/users").then(response => {
      let userLength = response.data.slice(0, 5);
      this.users = userLength || [];
    });
  },
  methods: {
    deleteUser(user) {
      axios
        .delete(`https://jsonplaceholder.typicode.com/users/${user.id}`)
        .then(response => {
          this.users = this.users.filter(i => i.id !== user.id);
        });
    },
    addUser() {
      axios
        .post("https://jsonplaceholder.typicode.com/users", this.userData)
        .then(response => {
          this.users.push(this.userData);
          console.log(response);
          this.userData = {};
        });
    },
    editUser(user) {
      this.updateData = {
        id: user.id,
        name: user.name,
        username: user.username,
        email: user.email,
        phone: user.phone
      };
      this.deleteUser(user);
    },
    updateUser() {
      axios
        .put(
          `https://jsonplaceholder.typicode.com/users/${this.updateData.id}`,
          this.updateData
        )
        .then(response => {
          let updatedData = {
            id: response.data.id,
            name: response.data.name,
            username: response.data.username,
            email: response.data.email,
            phone: response.data.phone
          };
          this.users.push(updatedData);
        });
    }
  }
};
</script>

<style lang="scss">
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500&family=Poppins:wght@200;300;400&display=swap");
body {
  height: 100vh;
  position: relative;
  overflow: hidden;
  font-family: "Montserrat", sans-serif;
  font-family: "Poppins", sans-serif;
  font-size: 16px;
  font-weight: 500;
  background: #4da0b0;
  background: -webkit-linear-gradient(to right, #d39d38, #4da0b0);
  background: linear-gradient(to right, #d39d38, #4da0b0);
}
input,
button {
  box-shadow: none;
  border: none;
  outline: none;
  &:focus {
    box-shadow: none !important;
  }
}

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
  margin: 0;
}


.modal-footer {
  button {
    color: white;
  }
}

.table-responsive {
  margin: 30px 0;
}
.table-wrapper {
  border-radius: 3px;
  min-width: 1000px;
  box-shadow: 0 1px 1px rgba(0, 0, 0, 0.05);
}
.add-user {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.add-user button {
  padding: 5px 30px;
}

.form-group {
  display: flex;
  flex-direction: column;
  align-items: flex-start;
}

.form-group input {
  background: transparent;
  border-bottom: 1px solid #40544b;
  width: 100%;
  position: relative;
  transition: 0.5s;
}

.modal-dialog .form-group input {
  &:focus {
    border-bottom: 1px solid #68b3ff !important;
  }
}

.table-title {
  padding-bottom: 15px;
  background: #435d7d;
  color: #fff;
  padding: 16px 30px;
  min-width: 100%;
  margin: -20px -25px 10px;
  border-radius: 3px 3px 0 0;
}
.table-title h2 {
  margin: 5px 0 0;
  font-size: 24px;
}
.table-title .btn-group {
  float: right;
}
.table-title .btn {
  color: #fff;
  float: right;
  font-size: 13px;
  border: none;
  min-width: 50px;
  border-radius: 2px;
  border: none;
  outline: none !important;
  margin-left: 10px;
}
.table-title .btn i {
  float: left;
  font-size: 21px;
  margin-right: 5px;
}
.table-title .btn span {
  float: left;
  margin-top: 2px;
}
table.table tr th,
table.table tr td {
  border-color: #e9e9e9;
  padding: 12px 15px;
  vertical-align: middle;
  text-align: center;
}
table.table tr th:first-child {
  width: 60px;
}
table.table tr th:last-child {
  width: 100px;
}
table.table-striped tbody tr:nth-of-type(odd) {
  background-color: #fcfcfc;
}
table.table-striped.table-hover tbody tr:hover {
  background: #f5f5f5;
}
table.table th i {
  font-size: 13px;
  margin: 0 5px;
  cursor: pointer;
}
table.table td:last-child i {
  opacity: 0.9;
  font-size: 22px;
  margin: 0 5px;
}
table.table td a {
  font-weight: bold;
  color: #566787;
  display: inline-block;
  text-decoration: none;
  outline: none !important;
}
table.table td a:hover {
  color: #2196f3;
}
table.table td a.edit {
  color: white;
  background-color: #fbc02d;
}
table.table td a {
  padding: 5px 24px;
  border-radius: 3px;
}
table.table td a.delete {
  color: white;
  background-color: #d32f2f;
}
table.table td i {
  font-size: 17px !important;
}
table.table .avatar {
  border-radius: 50%;
  vertical-align: middle;
  margin-right: 10px;
}
table .actions {
  width: 18%;
}
</style>
