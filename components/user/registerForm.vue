<template>
  <div class="main">
    <el-form :model="ruleForm" :rules="rules" ref="ruleForm" class="registerForm">
      <el-form-item prop="username">
        <el-input v-model="ruleForm.username" placeholder="请输入用户名/手机"></el-input>
      </el-form-item>

      <el-form-item prop="captcha">
        <el-row type="flex">
            <el-input v-model="ruleForm.captcha" placeholder="请输入验证码"></el-input>
            <el-button>发送验证码</el-button>
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

      <el-button type="primary" style="width: 100%">注册</el-button>
    </el-form>
  </div>
</template>

<script>
export default {
    data:function () {
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
                    {required: true, message: '请输入用户名/密码', trigger: 'blur'}
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
                    {required: true, message: '请输入再次密码', trigger: 'blur'}
                ]
            }
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
