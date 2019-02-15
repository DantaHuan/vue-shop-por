<template>
  <div id="login-container">
    <div id="login-box">
      <div id="logo-box">
        <img src="../assets/logo.png" alt>
      </div>
      <div>
        <el-form :rules="loginFormRules" ref="loginFormRef" :model="loginForm">
          <el-form-item prop="username">
            <el-input v-model="loginForm.username">
              <i slot="prefix" class="iconfont icon-user"></i>
            </el-input>
          </el-form-item>
          <el-form-item prop="password">
            <el-input type="password" v-model="loginForm.password">
              <i slot="prefix" class="iconfont icon-3702mima"></i>
            </el-input>
          </el-form-item>
          <el-row>
            <el-col :offset="15">
              <el-button type="primary" @click="login">登录</el-button>
              <el-button type="info" @click="reseForm">重置</el-button>
            </el-col>
          </el-row>
        </el-form>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      // 登录时的表单验证所需要的数据
      loginForm: {
        username: '',
        password: ''
      },
      // 给各个表单域定义校验规则
      loginFormRules: {
        username: [
          // required:非空、 message：错误提示、trigger：触发校验机制
          { required: true, message: '请输入正确的用户名称', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入正确的密码', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    // 用户登录系统
    login() {
      // 对登录的form表单进行整体验证
      // this.$refs.loginFormRef.validate(function(valid){})
      this.$refs.loginFormRef.validate(async valid => {
        // console.log(valid) true/false 校验成功或失败
        if (valid === true) {
          // 用户真实性的验证
          // axios带着用户信息去到后端数据库校验
          const { data: res } = await this.$http.post('/login', this.loginForm)
          // console.log(res)

          // 判断用户名或密码 真实性校验失败
          if (res.meta.status !== 200) {
            return this.$message.error('用户名或密码不存在')
          }
          // 通过浏览器的sessionStorage记录服务器返回的token信息
          window.sessionStorage.setItem('token', res.data.token)
          // 校验成功 界面重定向到后台首页（/home）
          this.$router.push('/home')
        }
      })
    },
    reseForm() {
      this.$refs.loginFormRef.resetFields()
    }
  }
}
</script>

<style lang="less" scoped>
#login-container {
  background-color: #2b4b6b;
  height: 100%;
  overflow: hidden;
  #login-box {
    width: 450px;
    height: 304px;
    background-color: #fff;
    border-radius: 5px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    .el-form {
      position: absolute;
      bottom: 0px;
      width: 100%;
      padding: 20px;
      box-sizing: border-box;
    }
    #logo-box {
      width: 130px;
      height: 130px;
      border: 1px solid #eee;
      border-radius: 50%;
      padding: 8px;
      background-color: #fff;
      box-shadow: 0 0 10px #eee;
      position: absolute;
      left: 50%;
      transform: translate(-50%, -50%);
      img {
        width: 130px;
        height: 130px;
        border-radius: 50%;
        background-color: #eee;
      }
    }
  }
}
</style>
