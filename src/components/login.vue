<!--
  顾客登录输入框组件
  by：汪明杰
-->

<template>
  <el-container style="height: 100%;">
    <el-header style="height: auto;">
      <!--顾客身份-->
      <el-image
          :src="customerIcon"
          style="width: 10%;position:absolute;top:20%;cursor: pointer;"
          @click="changeToCustomer(1)"
          @mouseover="changeToCustomer(2)"
          @mouseout="changeToCustomer(3)"
      ></el-image>
      <p
          style="font-size: xx-large;
            margin-top: 10px;
            text-align: center;">
        登录</p>
      <!--房东身份-->
      <el-image
          :src="hostIcon"
          style="width: 10%;position:absolute;top:20%;right: 10%;cursor: pointer;"
          @click="changeToHost(1)"
          @mouseover="changeToHost(2)"
          @mouseout="changeToHost(3)"
      ></el-image>
      <p>登录“归宿”，体验专属于你的精彩世界！</p>
    </el-header>
    <el-form style="margin-top: 20px;margin-left: 40px; margin-right: 40px;height: 100%;">
      <el-row style="height: 100%;">
        <el-form-item>
          <el-input v-model="phonenumber" placeholder="手机号"></el-input>
        </el-form-item>
        <el-form-item>
          <el-input
              v-model="password"
              placeholder="请输入密码"
              show-password
          ></el-input>
        </el-form-item>
        <!--验证码-->
        <el-form-item style="height: 100%;margin-bottom: 0;">
          <el-col :span="11">
            <el-input
                v-model="verifycode"
                placeholder="验证码"
                maxlength=4
            >
            </el-input>
          </el-col>
          <el-col :span="11" style="margin-left: 30px;height: 100%;">
            <el-image
                :src="codeimg"
                @click="updateVerifyCode"
                style="width: 140px;height:50px;cursor: pointer;">
            </el-image>
          </el-col>

        </el-form-item>
        <el-form-item style="margin-bottom: 10px;">
          <el-col :span="11" style="margin-left: 20px;">
            <el-checkbox
                label="记住我"
                name="type"
                v-model="rememberMe"
            ></el-checkbox>
          </el-col>
          <el-col :span="11" style="width: auto;margin-left: 40px;">
            <el-button
                type="text"
                @click="forgetPassword"
            >
              忘记密码？
            </el-button>
          </el-col>
        </el-form-item>

      </el-row>
    </el-form>
  </el-container>
</template>

<script>
import {getVerifyCode} from '@/api/public'
import {mapMutations} from 'vuex';

export default {
  name: 'LoginName',//这个LoginName最好和引入的vue的LoginName相同
  data() {
    return {
      phonenumber: '',
      password: '',
      verifycode: '',
      codeimg: '',
      trueVerifycode: '',//正确的验证码
      customerIcon: "https://oliver-img.oss-cn-shanghai.aliyuncs.com/img/customerIconSelected.jpg",
      hostIcon: "https://oliver-img.oss-cn-shanghai.aliyuncs.com/img/hostIconSelected.jpg",
      rememberMe: false,
    }
  },
  created() {
    /*
    页面生成时更新
    */

    //判断是否有"记住我"信息
    let rememberState = localStorage.getItem('rememberPhone');

    if (rememberState === null || rememberState === '') {
      console.log('上次操作没有选择记住我')
    } else {
      console.log('上次操作选择了"记住我"，已自动读取')
      this.phonenumber = localStorage.getItem('rememberPhone');
      console.log('电话为:', this.phonenumber, localStorage.getItem('rememberPhone'));
      this.password = localStorage.getItem('rememberPassword');
      this.rememberMe = true;
    }
    this.updateVerifyCode();
  },
  methods: {
    submitForm() {
      // this.$parent.$parent.$parent.$parent.$parent.dialogTableVisible=false;
      if (this.verifycode === this.trueVerifycode) {
        this.$emit("logins")
        this.$router.go(0);
      } else {
        this.verifycode = ""
        this.updateVerifyCode()
        this.$message({
          message: "验证码错误，请重新输入",
          type: 'warning'
        });
      }
    },

    updateVerifyCode() {
      /*
      更新验证码
      */
      getVerifyCode().then(response => {
        console.log('verifycode:', response);
        this.codeimg = response.data.codeImg;
        this.trueVerifycode = response.data.token;
      })
    },

    forgetPassword() {
      /*
      忘记密码
      */
      console.log('忘记密码按钮被触发')
      let that = this
      this.$router.push({
        path: '/forgetPassword', query: {
          isCustomer: that.customerLogin
        }
      });
      //关闭登录界面
      this.$emit('closeLogin');
    }

  }
}
</script>

<style scoped>

</style>
