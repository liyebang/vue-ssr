<template>
    <div class="main">
        <!-- 轮播图 -->
        <Carousel/>

        <!-- 登录注册Tab栏 -->
        <el-row class="tab-tool">
            <el-row class="button" type="flex">
                <div
                v-for="(item, index) in ['登录', '注册']"
                :key="index"
                style="flex: 1"
                @click="handleClick(index)"
                :class="{'button-login': true, 'active': active===index}"
                >
                    {{item}}
                </div>
            </el-row>
            <el-row class="content">
                <!-- <el-row class="content-login" v-if="active===0">
                    <el-form 
                    :rules="rules"
                    :model="form"
                    ref="form"
                    >
                        <el-form-item  prop="username">
                            <el-input 
                            type="text" 
                            v-model="form.username" 
                            placeholder="请输入用户名/手机">
                            </el-input>
                        </el-form-item>
                        <el-form-item  prop="password">
                            <el-input 
                            type="password" 
                            v-model="form.password" 
                            placeholder="请输入密码">
                            </el-input>
                        </el-form-item>
                        <div class="forget"><a href="#">忘记密码</a></div>
                        <el-button type="primary" style="width: 100%">登录</el-button>
                    </el-form>
                </el-row> -->
                <!-- <el-row class="content-register" v-else></el-row> -->
                <LoginForm v-if="active===0"/>
                <Register v-else/>
            </el-row>
        </el-row>

    </div>
</template>

<script>
//引入轮播图组件
import Carousel from '@/components/carousel.vue';
//引入登录表单
import LoginForm from '@/components/user/loginForm.vue';
//引入注册表单
import Register from '@/components/user/registerForm.vue';

export default {
    data:function () {
        return {
            active: 0,
            // form:{
            //     username:'',
            //     password:''
            // },
            // rules:{
            //     username:[
            //         { required: true, message: '请输入用户名/手机', trigger: 'blur' }
            //     ],
            //     password:[
            //         { required: true, message: '请输入密码', trigger: 'blur' }
            //     ]
            // }
        }
    },
    // mounted(){
    //     console.log(this.$store.state.user.userInfo.token);
    // },
    components:{
        Carousel,
        LoginForm,
        Register
    },
    methods:{
        //tab栏切换的方法
        handleClick:function (i) {
            this.active = i;
        }
    }
}
</script>

<style scoped lang='less'>
.main{
    position: relative;
    min-width: 1000px;
    .tab-tool{
        width: 400px;
        position: absolute;
        z-index: 999;
        top: 50%;
        left: 50%;
        transform: translateX(-50%) translateY(-50%);
        background-color: #fff;
        .button{
            height: 50px;
            line-height: 50px;
            text-align: center;
            color: #666;
            background: #eee;
            cursor: pointer;
            .button-login{}
            .button-register{}
            .active{
                color: orange;
                border-top: 2px solid orange;
                background-color: #fff;
                font-weight: 700;
            }
        }
        .content{
            // padding: 25px;
            // .content-login{
            //     .forget{
            //         text-align: right;
            //         margin-bottom: 10px;
            //         a{
            //             color: #409eff;
            //             font-size: 12px;
            //         }
            //     }
            // }
            .content-register{}
        }
    }
}

</style>
