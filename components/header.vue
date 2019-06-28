<template>
    <div class="header">
        <div class="main">

            <el-row type='flex' justify='space-between'>

                <!-- logo -->
                <el-row class="logo">
                    <nuxt-link to="/">
                        <img src="http://157.122.54.189:9093/images/logo.jpg" alt="">
                    </nuxt-link>
                </el-row>

                <!-- 导航栏 -->
                <el-row class="nav">
                    <nuxt-link to="/">首页</nuxt-link>
                    <nuxt-link to="/post">旅游攻略</nuxt-link>
                    <nuxt-link to="/hotel">酒店</nuxt-link>
                    <nuxt-link to="/air">国内机票</nuxt-link>
                </el-row>

                <!-- 登录注册与用户信息 -->
                <el-row class="login-register" v-if="!$store.state.user.userInfo.token">
                    <el-dropdown>
                        <span class="el-dropdown-link">
                            <i class="el-icon-bell"></i>
                            消息
                            <i class="el-icon-caret-bottom el-icon--right"></i>
                        </span>
                        <el-dropdown-menu slot="dropdown">
                            <el-dropdown-item>消息</el-dropdown-item>
                        </el-dropdown-menu>
                    </el-dropdown>
                    <nuxt-link to='/user/login'>登录 / 注册</nuxt-link>
                </el-row>

                <el-row v-else class="user">
                    <el-dropdown>
                        <span class="el-dropdown-link">
                            <i class="el-icon-bell"></i>
                            消息
                            <i class="el-icon-caret-bottom el-icon--right"></i>
                        </span>
                        <el-dropdown-menu slot="dropdown">
                            <el-dropdown-item>消息</el-dropdown-item>
                        </el-dropdown-menu>
                    </el-dropdown>
                    <el-dropdown>
                        <span class="el-dropdown-link">
                           <el-row type="flex" align="middle" class="userinfo">
                                <img :src="$axios.defaults.baseURL + $store.state.user.userInfo.user.defaultAvatar" alt="">
                                <i class="user-name">{{$store.state.user.userInfo.user.nickname}}</i>
                                <i class="el-icon-caret-bottom el-icon--right"></i>
                            </el-row>
                        </span>
                        <el-dropdown-menu slot="dropdown">
                            <el-dropdown-item>个人中心</el-dropdown-item>
                            <el-dropdown-item>
                                <div @click="handleLogout">退出</div>
                            </el-dropdown-item>
                        </el-dropdown-menu>
                    </el-dropdown>


                    
                </el-row>

            </el-row>

        </div>
    </div>
</template>

<script>
export default {
    // mounted(){
    //     console.log(this.$store.state.user);
    // }
    methods:{
        handleLogout:function () {
            this.$store.commit('user/clearUserInfo');
        }
    }
}
</script>

<style lang='less' scoped>
//排版样式
.header{
    height: 60px;
    border-bottom: 1px solid #ddd;
    box-sizing: border-box;
    .main{
        width: 1000px;
        margin: 0 auto;
        .logo{
            width: 156px;
            height: 43px;
            margin-top: 9px;
            margin-right: 10px;
            a{
                img{
                    display: block;
                    width: 100%;
                }
            }
        }
        .nav{
            flex: 1;
            height: 60px;
            line-height: 60px;
            display: flex;
            a{
                padding: 0 20px;
                display: block;
                box-sizing: border-box;
                &:hover{
                    color: #409eff;
                    border-bottom: 5px solid #409eff;
                }
            }
        }
        .login-register{
            display: flex;
            justify-content: center;
            align-items: center;
            a{
                font-size: 14px;
                color: #666;
                margin-left: 10px;
                &:hover{
                    color: #409eff;
                    text-decoration: underline;
                }
            }
        }
        .user{
            display: flex;
            justify-content: center;
            align-items: center;
            .userinfo{
                &:hover img{
                    border: 2px solid #409eff;
                }
               margin-left: 10px; 
               img{
                   width: 36px;
                   height: 36px;
                   border-radius: 50%;
                   box-sizing: border-box;
                   border: 2px solid #fff;
               }
            }
        }
    }
}

//对应路径对应的样式
.nuxt-link-exact-active{
    background-color: #409eff;
    color: #fff !important;
}

//组件样式
.el-dropdown-link {
    cursor: pointer;
    color: inherit;
  }
  .el-icon-arrow-down {
    font-size: 12px;
  }
</style>
