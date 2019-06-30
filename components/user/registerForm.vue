<template>
  <div class="main">
    <el-form :model="ruleForm" :rules="rules" ref="ruleForm" class="registerForm">
      <el-form-item prop="username">
        <el-input v-model="ruleForm.username" placeholder="请输入用户名/手机"></el-input>
      </el-form-item>

      <el-form-item prop="captcha">
        <el-row type="flex">
            <el-input v-model="ruleForm.captcha" placeholder="请输入验证码"></el-input>
            <template>
              <el-button @click="handleSendCaptchas">发送验证码</el-button>
            </template>
        </el-row>
      </el-form-item>

      <el-form-item prop="nickname">
        <el-input v-model="ruleForm.nickname" placeholder="请输入你的名字"></el-input>
      </el-form-item>

      <el-form-item prop="password">
        <el-input v-model="ruleForm.password" type="password" placeholder="请输入密码"></el-input>
      </el-form-item>

      <el-form-item prop="passwordAgain">
        <el-input v-model="ruleForm.passwordAgain" type="password" placeholder="请再次输入密码"></el-input>
      </el-form-item>

      <el-button type="primary" style="width: 100%" @click="handleRegister">注册</el-button>
    </el-form>
  </div>
</template>

<script>
export default {
    data:function () {
          //自定义表单验证规则
          var validatePass = (rule, value, callback) => {
          if (value === '') {
            callback(new Error('请再次输入密码'));
          } else if (value !== this.ruleForm.password) {
            callback(new Error('两次输入密码不一致!'));
          } else {
            callback();
          }
        };
        return {
            ruleForm:{
                username: '',
                captcha: '',
                nickname: '',
                password: '',
                passwordAgain: ''
            },
            rules:{
                username:[
                    {required: true, message: '请输入用户名手机', trigger: 'blur'}
                ],
                captcha:[
                    {required: true, message: '请输入验证码', trigger: 'blur'}
                ],
                nickname:[
                    {required: true, message: '请输入你的名字', trigger: 'blur'}
                ],
                password:[
                    {required: true, message: '请输入密码', trigger: 'blur'}
                ],
                passwordAgain:[
                    // {required: true, message: '请输入再次密码', trigger: 'blur'},
                    //validator类似一个验证器
                    { validator: validatePass, trigger: 'blur' }
                ]
            }
        }
    },
    methods:{
      //发动验证码
      handleSendCaptchas(){
        this.$axios({
          url: '/captchas',
          method: 'POST',
          data:{ tel: this.ruleForm.username }
        }).then( res => {
          console.log(res);
          //模拟后台发送短信验证码
          let { code } = res.data;
          alert('验证码为：' + code);
        } )
      },
      //注册
      handleRegister(){
        this.$refs.ruleForm.validate( vaild => {
          if(vaild){
            const {passwordAgain, ...props} = this.ruleForm;
            this.$axios({
              url: '/accounts/register',
              method: 'POST',
              data: props
            }).then( res => {
              // console.log(res);
              //把用户数据给store
              this.$store.commit("user/setUserInfo", res.data);
              //完成注册后跳转
              this.$message.success("注册成功，正在登录...");
              setTimeout( () => {
                this.$router.push('/');
              },1500 )
            } )
          }
        } )
      }
    }
}
</script>

<style scoped lang='less'>
.main{

    .registerForm{
        width: 400px;
        padding: 25px;
        box-sizing: border-box;
    }
}
</style>
