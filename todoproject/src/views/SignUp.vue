<template>
  <div class="title" style="padding-top: 50px">
    <h1>SIGN UP</h1>
    <br />
    <div class="container">
      <el-form
        status-icon
        label-width="70px"
        class="demo-ruleForm"
        style="width: 85%; algin: center"
      >
        <el-form-item
          label=""
          style="text-align: left; font-weight: bold;"
          prop="email"
        >
          Email
          <ValidationProvider name="email" rules="required|email">
            <div slot-scope="{ errors }">
              <el-input
                type="email"
                v-model="register.email"
                autocomplete="off"
              ></el-input>
              <p style="color: red; font-size: 10px; margin-top:-10px; margin-bottom: -10px;">{{ errors[0] }}</p>
            </div>
          </ValidationProvider>
        </el-form-item>
        <el-form-item
          label=""
          style="text-align: left; font-weight: bold; margin-top: -20px; padding-top:5px;"
          prop="pass"
        >
          Password
          <ValidationProvider name="password" rules="required">
            <div slot-scope="{ errors }">
              <el-input
                type="password"
                v-model="register.password"
                autocomplete="off"
              ></el-input>
              <p style="color: red; font-size: 10px;margin-top:-10px; margin-bottom: -10px;">{{ errors[0] }}</p>
            </div>
          </ValidationProvider>
        </el-form-item>
        <el-form-item
          label=""
          style="text-align: left; font-weight: bold; margin-top: -20px; padding-top:5px;"
          prop="password_confirmation"
        >
          Confirm Password
          <ValidationProvider name="password" rules="required">
            <div slot-scope="{ errors }">
              <el-input
                type="password"
                v-model="register.password_confirmation"
                autocomplete="off"
              ></el-input>
              <p style="color: red; font-size: 10px; margin-top:-10px; margin-bottom: -10px;">{{ errors[0] }}</p>
            </div>
          </ValidationProvider>
        </el-form-item>
        <div class="btnSign">
          <el-button
            type="primary"
            class="btnSignIn"
            @click="registerIt"
            required
            >Sign In</el-button
          >
          <router-link to="/signin">
            <el-button type="primary" class="btnSignUp">Sign Up</el-button>
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
      register: {
        email: "",
        password: "",
        password_confirmation: "",
      },
    };
  },
  methods: {
    async registerIt() {
      await axios
        .post("https://mockup-api.herokuapp.com/auth/signup", this.register)
        .then((response) => {
          let token = response.data.auth_token;
          var email = response.email;
          //console.log(response);
          localStorage.setItem("token", token);
          localStorage.setItem("email", email);
        });
      //console.log(localStorage.getItem('token'));
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
  padding-right: 5px;
}
.btnSignIn {
  margin-left: 10px;
  margin-top: 30px;
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
  margin-left: 8px;
}
.btnSignUp {
  margin-top: 12px;
  border-top-right-radius: 10px;
  border-bottom-left-radius: 10px;
  border-top-left-radius: 10px;
  border-bottom-right-radius: 10px;
  border: none #ffff;
  width: 100%;
  font-size: 20px;
  padding-top: 10px;
  padding-bottom: 10px;
  margin-left: 8px;
}
</style>
