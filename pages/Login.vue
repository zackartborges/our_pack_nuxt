<template>
  <div class="login">
    <v-form v-on:submit.prevent="submit()">
      <v-container>
      <h1 class="login">Login</h1>
      <v-col>
      <ul>
        <li class="text-danger" v-for="error in errors" v-bind:key="error">
          {{ error }}
        </li>
      </ul>
        <v-text-field
        v-model="email"
        label="Email"
        outlined
        shaped></v-text-field>
      </v-col>
      <v-col>
        <v-text-field
        v-model="password"
        label="Password"
        outlined
        shaped></v-text-field>      
      </v-col>
      <input type="submit" class="btn btn-primary" value="Submit" />
      <!-- Possible correct Button, will test when more is completed -->
      <!-- <v-btn
        class="mr-4"
        type="submit"
        :disabled="invalid"
      >
        submit
      </v-btn> -->
      </v-container>
    </v-form>
  </div>
</template>
<style></style>
<script>
import axios from "axios";
export default {
  data: function () {
    return {
      email: "",
      password: "",
      errors: [],
    };
  },
  methods: {
    submit: function () {
      var params = {
        email: this.email,
        password: this.password,
      };
      axios
        .post("/api/sessions", params)
        .then((response) => {
          axios.defaults.headers.common["Authorization"] = "Bearer " + response.data.jwt;
          localStorage.setItem("jwt", response.data.jwt);
          localStorage.setItem("user_id", response.data.user_id);
          this.$router.push("/");
        })
        .catch((error) => {
          console.log(error.response);
          this.errors = ["Invalid email or password."];
          this.email = "";
          this.password = "";
        });
    },
  },
};
</script>
