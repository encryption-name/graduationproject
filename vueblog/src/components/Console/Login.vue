<template>
  <div class="login-wrap">
    <el-form :rules="rules" :model="loginForm" ref="loginForm" class="login-container" label-position="left" label-width="0px" v-loading="loading">
      <h3 class="login_title">后台管理系统</h3>
      <el-form-item prop="username">
        <el-input type="text" v-model="loginForm.username" auto-complete="off" placeholder="账号"></el-input>
      </el-form-item>
      <el-form-item prop="password">
        <el-input type="password" v-model="loginForm.password" auto-complete="off" placeholder="密码"></el-input>
      </el-form-item>
      <el-checkbox class="login_remember" v-model="checked" label-position="left">记住密码</el-checkbox>
      <el-form-item style="width: 100%">
        <el-button type="primary" @click.native.prevent="submitClick" style="width: 100%">登录</el-button>
      </el-form-item>
      
    </el-form>
  </div>
</template>
<script>
 import {postRequest} from '../../utils/api'
  import {putRequest} from '../../utils/api'
  import {deleteRequest} from '../../utils/api'
  import {getRequest} from '../../utils/api'
  export default{
    data(){ 
      return {
        rules: {
          username: [{required: true, message: '请输入用户名', trigger: 'blur'}],
          password: [{required: true, message: '请输入密码', trigger: 'blur'}]
        },
        checked: true,
        loginForm: {
          username: 'admin',
          password: 'admin'
        },
        loading: false
      }
    },
    methods: {
      submitClick: function () {
        this.loading = true;
        postRequest('/api/login', {
          account: this.loginForm.username,
          password: this.loginForm.password
        }).then(resp=> {
          this.loading = false;
            // console.log("resp",resp);
          if (resp.data.code == 1000) {
            localStorage.setItem('ms_username',this.loginForm.username);
            localStorage.setItem('ms_role',resp.data.data.role);
            this.$router.push('/articleList');
        } else {
            this.$message.error(resp.data.message) ;
        }
        }
        );
      }
    }
  }
</script>
<style>
.login-container {
  border-radius: 15px;
  background-clip: padding-box;
  margin: 180px auto;
  width: 350px;
  padding: 35px 35px 15px 35px;
  background: #fff;
  border: 1px solid #eaeaea;
  box-shadow: 0 0 25px #cac6c6;
}
.login-wrap {
  position: relative;
  width: 100%;
  height: 100%;
}

.login_title {
  margin: 0px auto 40px auto;
  text-align: center;
  color: #505458;
}

.login_remember {
  margin: 0px 0px 35px 0px;
  text-align: left;
}
</style>
