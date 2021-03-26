// CreatePost.vue

<template>
  <div>
    <h1>ADMIN LOGIN</h1>
    <form @submit.prevent="login">
      <div class="container justify-content-center">
        <div class="form-group">
          <label>Email:</label>
          <input type="text" class="form-control" v-model="data.email" />
        </div>

        <div class="form-group">
          <label>Password:</label>
          <input type="password" class="form-control" v-model="data.password" />
        </div>

        <br />
        <div class="form-group">
          <button class="btn btn-primary" type="submit">LOGIN</button>
        </div>
      </div>
    </form>
  </div>
</template>

<script>
export default {
  data() {
    return {
      data: {}
    };
  },
  methods: {
    login() {
      let uri = "api/auth/login";
      this.axios
        .post(uri, this.data)
        .then(response => {
          if (response.data.status) {
            //check if status of api is true
            localStorage.setItem("token", response.data.access_token); //store token
            this.$router.push({ name: "create" }); //redirect to home page
          } else {
            alert(response.data.message); //status not true alert the user 
            localStorage.removeItem("token"); //clear any token if available
          }
        })
        .catch(error => {
          alert("System error or invalid data sup-plied");//api error occurs alert user
          localStorage.removeItem("token");
        });
    }
  }
};
</script>