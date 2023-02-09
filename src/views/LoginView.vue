<!-- eslint-disable import/no-extraneous-dependencies -->
<!-- eslint-disable vuejs-accessibility/no-autofocus -->
<!-- eslint-disable vuejs-accessibility/label-has-for -->
<!-- eslint-disable max-len -->
<template>
  <div class="container">
    <div class="container-page">
      <div class="row justify-content-center">
        <h1 class="h3 mb-3 font-weight-normal">
          請先登入
        </h1>
        <div class="col-8">
          <form id="form" class="form-signin" @submit.prevent="login">
            <div class="form-floating mb-3">
              <input type="email" class="form-control" id="username" v-model="submit.username"
                placeholder="name@example.com" required autofocus>
              <label for="username">Email address</label>
            </div>
            <div class="form-floating">
              <input type="password" class="form-control" id="password" v-model="submit.password" placeholder="Password"
                required>
              <label for="password">Password</label>
            </div>
            <button class="btn btn-lg btn-primary w-100 mt-3" type="submit">
              登入
            </button>
          </form>
        </div>
      </div>
      <p class="mt-5 mb-3 text-muted">
        &copy; 2021~∞ - 六角學院
      </p>
    </div>

  </div>

</template>
<script type="module">
// eslint-disable-next-line import/no-extraneous-dependencies
import axios from 'axios';

export default {
  name: 'LoginView',
  data() {
    return {
      submit: {
        username: '',
        password: '',
      },
      url: 'https://vue3-course-api.hexschool.io/v2', // 請加入站點
      path: 'sesame-store',
    };
  },
  methods: {
    login() {
      console.log(this.submit);
      axios.post(`${this.url}/admin/signin`, this.submit)
        .then((res) => {
          console.log(res);
          const { token, expired } = res.data;
          document.cookie = `hexToken=${token}; expires=${new Date(expired)}; `;
          this.$router.push({ path: '/home', replace: true });
        })
        .catch((err) => {
          this.alert(err.data.message);
        });
    },
  },
};

</script>

<style>
.form-signin {
  width: 100%;
  max-width: 330px;
  padding: 15px;
  margin: auto;
}

.container {
  height: 100vh;
  align-items: center;
  display: flex;
  justify-content: center;
}
</style>
