<template>
  <div class="login-container" :style="note1">
    <div class="login-form">
      <div class="login-img" :style="note"></div>
      <el-form  :model="loginForm" :rules="loginRules" ref="loginForm" v-loading='loading'>
        <el-form-item prop="username">
          <el-input v-model="loginForm.username" size="medium" placeholder="用户名">
            <i slot="suffix" class="el-input__icon">
              <svg-icon icon-class='user' style="font-size:22px;color:#666;margin-top:8px"></svg-icon>
            </i>
          </el-input>
        </el-form-item>
        <el-form-item prop="password">
          <el-input v-model="loginForm.password" size="medium" placeholder="密码" :type="pwdType" @keyup.native.enter="handleLogin">
            <i slot="suffix" class="el-input__icon" @click="showPwd">
              <svg-icon :icon-class='pwdType === "password" ? "password":"logineye"' class="password-login"></svg-icon>
            </i>
          </el-input>
        </el-form-item>
        <p class="forget-pwd">
          <span @click="$router.push({path:'/forgetpwd/phone'})">忘记密码了？</span>
        </p>
        <el-form-item>
          <el-button type="primary" class="login-button" size="medium" @click="handleLogin">登录</el-button>
        </el-form-item>
      </el-form>
    </div>
    <div class="login-qrcode">
        <div class="qrcode-block">
          <div id="and_qrcode" ref="and_qrcode"></div>
          <div class="phone-btn">
            <svg-icon icon-class='Android'></svg-icon>
            <span>iphone</span>
          </div>
        </div>
        <div class="qrcode-block">
          <div id="app_qrcode" ref="app_qrcode"></div>
          <div class="phone-btn">
            <svg-icon icon-class='Iphone'></svg-icon>
            <span>iphone</span>
          </div>
        </div>
    </div>
    <div class="copy">© 2017-2020 电子政务办 & 神航星云 联合出品</div>
  </div>
</template>

<script>
import { isvalidUsername } from "@/utils/validate";
import QRCode from "qrcodejs2";

export default {
  name: "login",
  components: { QRCode },
  data() {
    const validateUsername = (rule, value, callback) => {
      if (!value) {
        callback(new Error("请输入用户名"));
      } else {
        callback();
      }
    };
    const validatePass = (rule, value, callback) => {
      if (value.length < 6) {
        callback(new Error("密码不能小于6位"));
      } else {
        callback();
      }
    };
    return {
      loginForm: {
        username: "",
        password: ""
      },
      note: {
        backgroundImage: "url(" + require("@/assets/imgs/logo2.png") + ")"
      },
      note1: {
        backgroundImage: "url(" + require("@/assets/imgs/bodybg.jpg") + ")"
      },
      loginRules: {
        username: [
          { required: true, trigger: "blur", validator: validateUsername }
        ],
        password: [{ required: true, trigger: "blur", validator: validatePass }]
      },
      loading: false,
      pwdType: "password"
    };
  },
  created() {
    this.$nextTick(() => {
      this.app_qrcode();
      this.and_qrcode();
    });
    
  },
  methods: {
    app_qrcode() {
      let qrcode = new QRCode("app_qrcode", {
        width: 100, // 设置宽度
        height: 100, // 设置高度
        text: "hello world"
      });
    },
    and_qrcode() {
      let qrcode = new QRCode("and_qrcode", {
        width: 100, // 设置宽度
        height: 100, // 设置高度
        text: "hello world"
      });
    },
    showPwd() {
      if (this.pwdType === "password") {
        this.pwdType = "";
      } else {
        this.pwdType = "password";
      }
    },
    handleLogin() {
      this.$refs.loginForm.validate(valid => {
        if (valid) {
          this.loading = true;
          setTimeout(res => {
            this.loading = false;
            this.$message({
              type: "success",
              message: "登陆成功！"
            });
            this.$router.push({ path: "/message/index" });
          }, 1000);
        } else {
          return false;
        }
      });
    }
  }
};
</script>

<style rel="stylesheet/scss" lang="scss">
.password-login {
  font-size: 16px;
  color: #666;
  margin-right: 2px;
  cursor: pointer;
  &:hover {
    color: #409eff;
  }
}
.login-container {
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-repeat: no-repeat;
  background-size: 100% 100%;
  background-image: linear-gradient(to bottom, #001f79, #1e74c7, #80ddff);
  .login-form {
    width: 360px;
    height: 320px;
    background-color: #fff;
    margin: 0 auto;
    background: #fff;
    border: 1px solid rgba(255, 255, 255, 0.3);
    box-shadow: 0 3px 0 rgba(12, 12, 12, 0.03);
    border-radius: 3px;
    padding: 30px;
    margin-top: 10%;
    .login-img {
      height: 59px;
      background-position: center top;
      background-repeat: no-repeat;
      margin-bottom: 10px;
    }
    .login-button {
      width: 100%;
    }
    .forget-pwd {
      font-size: 13px;
      color: #337ab7;
      margin-top: 0px;
      margin-bottom: 12px;
      cursor: pointer;
      &:hover {
        color: #37aeeb;
      }
    }
  }
  .login-qrcode {
    width: 360px;
    margin: 0 auto;
    display: flex;
    margin-top: 40px;
    .qrcode-block {
      text-align: center;
      flex: 1;
      #app_qrcode,
      #and_qrcode {
        img {
          background-color: #fff;
          padding: 2px;
          margin: 0 auto;
        }
      }
      .phone-btn {
        color: #fff;
        border-color: #fff;
        height: 40px;
        line-height: 40px;
        border-radius: 5px;
        padding-left: 14px;
        padding-right: 17px;
        margin-top: 20px;
        display: inline-block;
        border: 1px solid #e7eaec;
        font-size: 14px;
        cursor: pointer;
      }
    }
  }
  .copy {
    position: absolute;
    left: 0;
    right: 0;
    bottom: 12px;
    text-align: center;
    font-size: 14px;
    line-height: 24px;
    color: #fff;
  }
}
</style>
