<template>
  <div class="title" style="padding-top: 50px;">
    <h1>SIGN IN</h1>
    <br />
    <div class="container">
      <el-form
        @keyup.enter="signItin"
        status-icon
        label-width="70px"
        style="width: 85%; algin: center"
      >
        <el-form-item
          label=""
          style="text-align: left; font-weight: bold"
          prop="email"
        >
          Email
          <ValidationProvider name="email" rules="required|email">
            <div slot-scope="{ errors }">
              <el-input
                type="email"
                v-model="signin.email"
                autocomplete="off"
              ></el-input>
             <p style="color: red; font-size: 10px; margin-top:-10px; margin-bottom: -10px;">{{ errors[0] }}</p>
            </div>
          </ValidationProvider>
        </el-form-item>
        <el-form-item
          label=""
          style="text-align: left; font-weight: bold; margin-top: 20px; padding-top: 5px;"
          prop="pass"
        >
          Password
          <ValidationProvider name="password" rules="required">
            <div slot-scope="{ errors }">
              <el-input
                type="password"
                v-model="signin.password"
                autocomplete="off"
              ></el-input>
              <p style="color: red; font-size: 10px; margin-top:-10px; margin-bottom: -10px;">{{ errors[0] }}</p>
            </div>
          </ValidationProvider>
        </el-form-item>
        <div class="btnSign">
          <!-- <router-link to="/homepage"> -->
          <el-button type="primary" class="btnSignIn" required @click="signIn"
            >Sign In</el-button
          >
          <!-- </router-link> -->
          <router-link to="/signup">
            <el-button
              type="primary"
              class="btnSignUp"
              @click="resetForm('ruleForm')"
              >Sign Up Now</el-button
            >
            <!-- @click="resetForm('ruleForm')" -->
          </router-link>
        </div>
      </el-form>
    </div>
    <div class="linker">Copyright @2019</div>
  </div>
</template>
<script>
import axios from "axios";
export default {
  data() {
    return {
      signin: {
        email: "",
        password: "",
      },
      errored: false,
    };
  },
  methods: {
    async signIn() {
      console.log(this.signin);
      await axios
        .post("https://mockup-api.herokuapp.com/auth/signin", this.signin)
        .then((response) => {
          let newToken = response.data.auth_token;
          window.token = newToken;
          let email = response.data.email;
          // let email = response.da
          localStorage.setItem("token", newToken);
          localStorage.setItem("email", email);
          // localStorage.setItem("email", JSON.stringify(user));
          window.axios.defaults.params = { auth_token: newToken };
          // Event.$emit("signin", user);
          this.errored = false;
        })
        .catch((error) => {
          console.log(error);
          this.errored = true;
        });
      if (!this.errored) this.$router.push({ path: "/homepage" });
    },

    resetForm(formName) {
      this.$refs[formName].resetFields();
    },
  },
};
</script>
<style scoped>
.container {
  background: white;
  padding: 30px 0 50px 0;
  width: 400px;
  margin: auto;
  height: 500px;
}
.btnSign {
  text-align: center;
  text-decoration: none;
  padding-left: 60px;
  padding-right: 10px;
}
.btnSignIn {
  margin-left: 10px;
  margin-top: 40px;
  border-top-right-radius: 10px;
  border-bottom-left-radius: 10px;
  border-top-left-radius: 10px;
  border-bottom-right-radius: 10px;
  border: none #ffff;
  background: rgb(25, 235, 130);
  width: 100%;
  font-size: 20px;
  padding-top: 10px;
  padding-bottom: 10px;
}
.btnSignUp {
  margin-top: 12px;
  margin-left: 10px;
  border-top-right-radius: 10px;
  border-bottom-left-radius: 10px;
  border-top-left-radius: 10px;
  border-bottom-right-radius: 10px;
  border: none #ffff;
  width: 100%;
  font-size: 20px;
  padding-top: 10px;
  padding-bottom: 10px;
}
</style>
