<template>
  <div class="login">
    <div class="centerBar">
      <h2 style="margin:0;padding:0">用户登录</h2>
      <el-form
        status-icon
        label-width="100px"
        class="demo-ruleForm"
        label-position="top"
        :model="useObj"
        :rules="rules"
        ref="ruleForm"
      >
        <el-form-item label="用户名" prop="username">
          <el-input type="text" v-model="useObj.username" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="password">
          <el-input type="password" v-model="useObj.password" autocomplete="off"></el-input>
        </el-form-item>
        <el-form-item>
          <el-button type="primary" style="width:100%" @click.prevent="loginData('ruleForm')">登录</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  data () {
    return {
      useObj: {
        username: '',
        password: ''
      },
      rules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 5, max: 10, message: '长度在 5 到 10 之间', trigger: 'blur' }
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 5, max: 10, message: '长度在 5 到 10 中间', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    open2 () {
      this.$message({
        message: '恭喜你，登录成功!',
        type: 'success'
      })
    },
    open4 () {
      this.$message.error('密码错误!')
    },
    open5 () {
      this.$message.error('用户名不存在!')
    },
    open6 () {
      this.$message.error('请输入用户名或密码!')
    },
    loginData (formName) {
      this.$refs[formName].validate(valid => {
        if (valid) {
          this.$http({
            method: 'post',
            url: 'http://localhost:8888/api/private/v1/login',
            data: this.useObj
          })
            .then(res => {
              const { data, meta } = res.data
              if (meta.status === 200) {
                window.localStorage.setItem('token', data.token)
                this.open2()
                setTimeout(() => {
                  this.$router.push('/home')
                }, 1500)
              } else if (meta.msg === '密码错误') {
                this.open4()
              } else {
                this.open5()
              }
            })
            .catch(err => {
              console.log(err)
            })
        } else {
          this.open6()
        }
      })
    }
  }
}
</script>

<style>
.login {
  width: 100%;
  height: 100%;
  background-color: #ccc;
  position: relative;
}
.centerBar {
  background-color: #fff;
  width: 400px;
  height: 300px;
  border-radius: 10px;
  position: absolute;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  padding: 40px;
}
</style>
