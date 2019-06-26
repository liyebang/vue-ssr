<template>
    <div class='footer'>
        <div class="main">
            <el-row class="tips-info" type="flex" justify='space-around'>
                <el-row class="tips">
                    <h5>闲云旅游旅游网</h5>
                    <p>上亿旅行者共同打造的"旅行神器"</p>
                    <p><span style="color: orange">60,000</span> 多个全球旅游目的地</p>
                    <p><span style="color: orange">600,000</span> 个细分目的地新玩法</p>
                    <p><span style="color: orange">760,000,000</span> 次攻略下载</p>
                    <p><span style="color: orange">38,000</span> 家旅游产品供应商</p>
                </el-row>

                <el-row class="tips">
                    <h5>关于我们</h5>
                    <p>隐私政策 商标声明</p>
                    <p>服务协议 游记协议</p>
                    <p>商城平台服务协议</p>
                    <p>网络信息侵权通知指引</p>
                    <p>闲云旅游旅游网服务监督员</p>
                    <p>网站地图加入闲云旅游</p>
                </el-row>

                <el-row class="tips">
                    <h5>旅行服务</h5>
                    <p>旅游攻略 酒店预订</p>
                    <p>旅游特价 国际租车</p>
                    <p>旅游问答 旅游保险</p>
                    <p>旅游指南 订火车票</p>
                    <p>旅游资讯 APP下载</p>
                </el-row>

                <el-row class="code">
                    <img src="http://157.122.54.189:9093/images/1556522965.png" alt="">
                    <p>关注我们</p>
                </el-row>
            </el-row>
            <p class="brand">京ICP备08001421号 京公网安备110108007702 Copyright © 2016-2019 博学谷 All Rights Reserved</p>
        </div>
    </div>
</template>

<script>
export default {

}
</script>

<style lang='less' scoped>
.footer{
    height: 329px;
    background-color: #333;
    .main{
        width: 1000px;
        margin: 0 auto;
        .tips-info{
            padding: 30px 0;
            border-bottom: 1px solid #ccc;
            .tips{
                h5{
                    color: #ccc;
                    font-weight: 400;
                    font-size: 16px;
                    margin-bottom: 10px;
                }
                p{
                    color: #ccc;
                    font-size: 12px;
                    line-height: 21px;
                }
            }
            .code{
                text-align: center;
                img{
                    width: 140px;
                    height: 140px;
                }
                p{
                    font-size: 12px;
                    color: #ccc;
                }
            }
        }
        .brand{
            color: #ccc;
            font-size: 12px;
            height: 108px;
            line-height: 108px;
            text-align: center;
        }
    }
}
</style>
