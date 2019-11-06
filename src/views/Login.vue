<template>
  <div class="login-container">
    <div class="login-form">
      <el-card header="请先登录" class="login-card">
        <el-form @submit.native.prevent="login">
          <el-form-item label="用户名">
            <el-input v-model="model.username"></el-input>
          </el-form-item>
          <el-form-item label="密码">
            <el-input v-model="model.password"  type="password"></el-input>
          </el-form-item>
          <el-form-item>
            <el-button type="primary" native-type="submit" style="width:100%">登录</el-button>
          </el-form-item>
        </el-form>
      </el-card>
    </div>
  </div>
</template>

<script>
export default {
  name: "login",
  data() {
    return {
      model: {}
    };
  },
  methods: {
    async login() {
      const res = await this.$http.post("login", this.model);
      // sessionStorage.token = res.data.token
      localStorage.token = res.data.token;
      this.$router.push("/");
      this.$message({
        type: "success",
        message: "登录成功"
      });
    }
  }
};
</script>

<style scoped >
.login-container {
  min-height: 100%;
  width: 100%;
  background-color: #2d3a4b;
  overflow: hidden;
}
.login-form {
  position: relative;
  width: 500px;
  max-width: 100%;
  padding: 160px 35px 0;
  margin: 0 auto;
  overflow: hidden;
}
</style>
