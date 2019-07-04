<template>
    <div class="container">
        <div class="main">
            <div class="pay-title">
                支付总金额 <span class="pay-price">￥ {{data.price}}</span>
            </div>
            <div class="pay-main">
                <h4>微信支付</h4>
                <el-row type="flex" 
                justify="space-between" 
                align="middle"
                class="pay-qrcode">
                    <div class="qrcode">
                        <!-- 二维码 -->
                        <canvas id="qrcode-stage"></canvas>
                        <p>请使用微信扫一扫</p>
                        <p>扫描二维码支付</p>
                    </div>
                    <div class="pay-example">
                        <img src="http://157.122.54.189:9093/images/wx-sweep2.jpg" alt="">
                    </div>
                </el-row>
            </div>
        </div>
    </div>
</template>

<script>
import QRCode from "qrcode";
import { async } from 'q';

export default {
    data: function () {
      return {
          data:{},
          timerid: null
      }  
    },
    methods:{
        //查询是否已支付
        isPay(){
            let {id, orderNo, price} = this.data;
            let {token} = this.$store.state.user.userInfo;
            return this.$axios({
                url: '/airorders/checkpay',
                method: 'POST',
                data:{
                    id,
                    nonce_str: price,
                    out_trade_no: orderNo
                },
                headers:{
                    Authorization: `Bearer ${token}` 
                }
            }).then( res => {
                if(res.data.statusTxt === "订单未支付"){
                    return false;
                }else{
                    return true;
                }
            })
        }

    },

    //组件销毁后清除定时器
    destroyed(){
        clearInterval(this.timerid);
    },

    mounted(){
        // 这个处理方法是有缺陷的，不100%准确
        // userInfo在页面加载完才赋值
        setTimeout( v => {
            let {token} = this.$store.state.user.userInfo;

            this.$axios({
                url: `/airorders/${this.$route.query.id}`,
                method: 'GET',
                headers:{
                    Authorization: `Bearer ${token}` 
                }
            }).then( res => {
                this.data = res.data;
                const stage = document.querySelector("#qrcode-stage");
                QRCode.toCanvas(stage,this.data.payInfo.code_url,{
                    width: 200
                })

                //同时发起验证是否支付
                this.timerid = setInterval( async () => {
                    const pay = await this.isPay();
                    if(pay){
                        this.$message.success("订单支付成功");
                        clearInterval(this.timerid);
                    }
                },3000)

            } )
        },150)
    }
}
</script>

<style scoped lang="less">
.container{
    background:#f5f5f5;
    padding: 30px 0;

    .main{
        width:1000px;
        margin:0 auto;

        .pay-title{
            text-align: right;
            span{
                font-size:28px;
                color:orangered;
            }
        }

        .pay-main{
            background:#fff;
            margin-top:10px;
            border-top: 5px orange solid;
            padding:30px;

            h4{
                font-size: 28px;
                font-weight: normal;
                margin-bottom: 10px;
            }

            .pay-qrcode{
                padding:0 80px;
            }

            .qrcode{
                border:1px #ddd solid;
                padding:15px;
                height: fit-content;

                #qrcode-stage{
                    width:200px;
                    height:200px;
                    margin-bottom: 10px;
                }

                p{
                    line-height: 2;
                    text-align: center;
                }
            }
        }
    }
}
</style>