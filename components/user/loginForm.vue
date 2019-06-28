<template>
  <div class="main">
    <el-row class="content-login">
      <el-form :rules="rules" :model="form" ref="form">
        <el-form-item prop="username">
          <el-input type="text" v-model="form.username" placeholder="请输入用户名/手机"></el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input type="password" v-model="form.password" placeholder="请输入密码"></el-input>
        </el-form-item>
        <div class="forget">
          <a href="#">忘记密码</a>
        </div>
        <el-button 
        type="primary" 
        style="width: 100%"
        @click="handleLogin">
            登录
        </el-button>
      </el-form>
    </el-row>
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      form: {
        username: "",
        password: ""
      },
      rules: {
        username: [
          { required: true, message: "请输入用户名/手机", trigger: "blur" }
        ],
        password: [{ required: true, message: "请输入密码", trigger: "blur" }]
      }
    };
  },
  methods:{
      handleLogin: function () {
          this.$refs.form.validate( valid => {
              if(valid){
                  //调用actions
                  this.$store.dispatch('user/login',this.form).then( v => {
                      this.$message.success('登录成功，正在跳转....');
                      setTimeout( () => {
                          this.$router.push("/");
                      },1000)
                  } )
              }
          } )
      }
  }
};
</script>

<style scoped lang='less'>
.main {
  .content-login {
    padding: 25px;
    width: 400px;
    .forget {
      text-align: right;
      margin-bottom: 10px;
      a {
        color: #409eff;
        font-size: 12px;
      }
    }
  }
}
</style>
