<template>

  <!--背景图-->
  <div class="note" :style="note">
    <!--login框，表单+tab标签页的组合-->
    <div class="loginFrame">
      <!--表单组件放在外面，标签栏在里面-->
      <el-form class="demo-ruleForm login-container" autoComplete="on" :model="loginForm" :rules="loginRules" ref="loginForm" label-position="left">

        <div class="formGroup">
          <el-form-item label="账号" prop="username">
            <el-input name="username" type="text" v-model="loginForm.username" autoComplete="on" placeholder="username" />
          </el-form-item>
          <el-form-item label="密码" prop="password" class="form-inline">
            <el-input name="password" :type="pwdType" @keyup.enter.native="handleLogin" v-model="loginForm.password" autoComplete="on" placeholder="password"></el-input>
          </el-form-item>
        </div>
        <div class="remFor">
          <a href="'https://blog.csdn.net/Vanadis_outlook/article/details/72823024.html'" class="forget">忘记密码？</a>
        </div>

        <div class="formButton">
          <el-form-item style="width:100%;">
            <el-button type="primary" style="width:100%;" :loading="loading" @click.native.prevent="handleLogin">登录</el-button>
          </el-form-item>
        </div>
      </el-form>
    </div>
  </div>
</template>

<script>
import {
  isvalidUsername
} from '@/utils/validate'

export default {
  name: 'login',
  data() {
    const validateUsername = (rule, value, callback) => {
      if (!isvalidUsername(value)) {
        callback(new Error('请输入正确的用户名'))
      } else {
        callback()
      }
    }
    const validatePass = (rule, value, callback) => {
      if (value.length < 5) {
        callback(new Error('密码不能小于5位'))
      } else {
        callback()
      }
    }
    return {
      loginForm: {
        username: 'yukong',
        password: 'abc123'
      },
      loginRules: {
        username: [{
          required: true,
          trigger: 'blur',
          validator: validateUsername
        }],
        password: [{
          required: true,
          trigger: 'blur',
          validator: validatePass
        }]
      },
      loading: false,
      pwdType: 'password',
      note: {
        position: 'absolute',
        top: '0px',
        left: '0px',
        width: '100%',
        height: '100%',
        backgroundImage: 'url(' + require('../../assets/bg.png') + ')',
        backgroundSize: '100% 100%',
        backgroundRepeat: 'no-repeat'

      }
    }
  },
  methods: {
    showPwd() {
      if (this.pwdType === 'password') {
        this.pwdType = ''
      } else {
        this.pwdType = 'password'
      }
    },
    handleLogin() {
      this.$refs.loginForm.validate(valid => {
        if (valid) {
          this.loading = true
          this.$store.dispatch('Login', this.loginForm).then(() => {
            this.loading = false
            this.$router.push({
              path: '/'
            })
          }).catch(() => {
            this.loading = false
          })
        } else {
          console.log('error submit!!')
          return false
        }
      })
    }
  }
}

</script>

<style>
.login-container {
  -webkit-border-radius: 5px;
  border-radius: 15px;
  -moz-border-radius: 5px;
  background-clip: padding-box;
  margin: 180px auto;
  width: 350px;
  padding: 35px 35px 15px 35px;
  background: rgba(255, 255, 255, 0.7);
  border: 1px solid #eaeaea;
  box-shadow: 0 0 25px #cac6c6;
  position: absolute;
  right: 50px;
}
label {
  width: 70px;
  text-align: left;
}

.form-control {
  width: 270px;
  flex: 1;
  -webkit-flex: 1;
  -ms-flex: 1;
}
.remember {
  width: 250px;
  text-align: left;
}
.forget {
  width: 500px;
  text-align: right;
  font-size: 14px;
  font-family: PingFang SC;
}
.remFor {
  margin-bottom: 10px;
  padding-bottom: 10px;
}
.tabsUser {
  display: inline-block;
  margin-left: 0px;
  margin-right: 0px;
  text-align: center;
  font-size: 25px;
}
</style>