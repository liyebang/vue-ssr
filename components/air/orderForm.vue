<template>
  <div class="main">
    <div class="air-column">
      <h2>乘机人</h2>
      <el-form class="member-info">
        <div class="member-info-item" v-for="(item, index) in user" :key="index">
          <el-form-item label="乘机人类型">
            <el-input placeholder="姓名" class="input-with-select" v-model="item.username">
              <el-select slot="prepend" value="1" placeholder="请选择">
                <el-option label="成人" value="1"></el-option>
              </el-select>
            </el-input>
          </el-form-item>

          <el-form-item label="证件类型">
            <el-input placeholder="证件号码" class="input-with-select" v-model="item.id">
              <el-select slot="prepend" value="1" placeholder="请选择">
                <el-option label="身份证" value="1" :checked="true"></el-option>
              </el-select>
            </el-input>
          </el-form-item>

          <span class="delete-user" @click="handleDeleteUser(index)">-</span>
        </div>
      </el-form>

      <el-button class="add-member" type="primary" @click="handleAddUsers">添加乘机人</el-button>
    </div>

    <div class="air-column">
      <h2>保险</h2>
      <div>
        <div class="insurance-item" v-for="(item, index) in insuranceslist" :key="index">
          <el-checkbox
            :label="`${item.type}：￥${item.price}/份×1  最高赔付${item.compensation}`"
            border
            @change="handleInsurance(item.id)"
          ></el-checkbox>
        </div>
      </div>
    </div>

    <div class="air-column">
      <h2>联系人</h2>
      <div class="contact">
        <el-form label-width="60px">
          <el-form-item label="姓名">
            <el-input v-model="contactName"></el-input>
          </el-form-item>

          <el-form-item label="手机">
            <el-input placeholder="请输入内容" v-model="contactPhone">
              <template slot="append">
                <el-button @click="handleSendCaptcha">发送验证码</el-button>
              </template>
            </el-input>
          </el-form-item>

          <el-form-item label="验证码">
            <el-input v-model="captcha"></el-input>
          </el-form-item>
        </el-form>
        <el-button type="warning" class="submit" @click="handleSubmit">提交订单</el-button>
      </div>
    </div>

    <!-- 调用一下total，让其执行 -->
    <input type="hidden" :value="total" />
  </div>
</template>

<script>
export default {
  data: function() {
    return {
      user: [{ username: "", id: "" }],
      insurances: [], //所选择的保险id
      contactName: "", //联系人名字
      contactPhone: "", //联系人电话
      invoice: false, //是否需要发票
      captcha: "", //验证码
      insuranceslist: [], //后台请求回来的保险列表
      orderinfo: {
          seat_infos:{}
      } //后台请求回来的订单详细信息
    };
  },
  methods: {
    // 添加乘机人
    handleAddUsers() {
      this.user.push({
        username: "",
        id: ""
      });
    },

    // 移除乘机人
    handleDeleteUser(index) {
      this.user.splice(index, 1);
    },

    //处理选择的保险
    handleInsurance(id) {
      let index = this.insurances.indexOf(id);
      if (index === -1) {
        this.insurances.push(id);
      } else {
        this.insurances.splice(index, 1);
      }
    },

    // 发送手机验证码
    handleSendCaptcha() {
      //发动验证码
      this.$axios({
        url: "/captchas",
        method: "POST",
        data: { tel: this.contactPhone }
      }).then(res => {
        //模拟后台发送短信验证码
        let { code } = res.data;
        alert("验证码为：" + code);
      });
    },

    // 提交订单
    handleSubmit() {
      let data = {
        users: this.user,
        insurances: this.insurances,
        contactName: this.contactName,
        contactPhone: this.contactPhone,
        invoice: this.invoice,
        seat_xid: this.$route.query.seat_xid,
        air: this.$route.query.id,
        captcha: this.captcha
      };

      //获取token
      let { token } = this.$store.state.user.userInfo;

      this.$axios({
        url: "/airorders",
        method: "POST",
        data,
        //设置请求头
        headers: {
          Authorization: `Bearer ${token}`
        }
      }).then(res => {
        // console.log(res);
        this.$message.success("正在生成订单，请稍后...");

        //订单id
        let { id } = res.data.data;

        this.$router.push({
          path: "/air/pay",
          query: {
            id
          }
        });
      });
    }
  },
  mounted() {
    //请求机票相关信息
    this.$axios({
      url: `/airs/${this.$route.query.id}`,
      method: "GET",
      params: { seat_xid: this.$route.query.seat_xid }
    }).then(res => {
      let { insurances, seat_infos } = res.data;
      //保险
      this.insuranceslist = insurances;
      this.orderinfo = res.data;
      //把订单详情传回父组件
      this.$emit("setOrderinfo", res.data);
    });
  },
  computed: {
    //计算总价
    total() {
    
      let total = 0;

      //加上机票单价
      total += this.orderinfo.seat_infos.org_settle_price;

      //加上燃油费
      total += this.orderinfo.airport_tax_audlet;

      //加上保险费
      total += this.insurances.length * 30;

      //再乘上人数
      total *= this.user.length;

      //把总计传递给父组件
      this.$emit('setTotal',total);

      return total;
    }
  }
};
</script>

<style scoped lang="less">
.air-column {
  border-bottom: 1px #ddd dashed;
  padding-bottom: 20px;
  margin-bottom: 20px;
}

.air-column h2 {
  margin-bottom: 20px;
  font-size: 22px;
  font-weight: normal;
}

/deep/ .el-select .el-input {
  width: 130px;
}

.input-with-select {
  width: 590px;
}

.input-with-select /deep/ .el-input-group__prepend {
  background-color: #fff;
}
.member-info /deep/ .el-form-item {
  margin-bottom: 0;
}

.member-info-item {
  border-bottom: 1px #eee dashed;
  padding-bottom: 20px;
  position: relative;

  &:first-child {
    .delete-user {
      display: none;
    }
  }
}

.add-member {
  margin-top: 20px;
}

.delete-user {
  display: block;
  background: #ddd;
  width: 16px;
  height: 16px;
  font-size: 14px;
  text-align: center;
  line-height: 16px;
  color: #fff;
  cursor: pointer;
  border-radius: 50px;
  position: absolute;
  right: -30px;
  top: 50%;
}

.insurance {
  > div {
    margin-top: 10px;
  }
}

.insurance-item {
  margin-bottom: 20px;
}

.contact {
  /deep/ .el-input {
    width: 50%;
  }
}

.submit {
  margin: 50px auto;
  display: block;
  width: 250px;
  height: 50px;
}
</style>