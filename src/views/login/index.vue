<template>
  <div class='container' ref="box">
    <el-card class="my-card">
      <img src="../../assets/imgs/logo_index.png" alt="">
    <el-form :model="loginForm" :rules="loginRules" ref="hehe">
        <el-form-item prop="mobile">
          <el-input v-model="loginForm.mobile" placeholder="请输入手机号"></el-input>
        </el-form-item>
        <el-form-item prop="code">
          <el-input v-model="loginForm.code" placeholder="请输入验证码" style="width:238px;margin-right:10px"></el-input>
          <el-button>发送验证码</el-button>
        </el-form-item>
        <el-form-item>
          <el-checkbox :value="true">我已阅读并同意用户协议和隐私条款</el-checkbox>
        </el-form-item>
        <el-form-item>
           <el-button type="primary" style="width:100%" @click="login()">登 录</el-button>
        </el-form-item>
      </el-form>
    </el-card>

  </div>
</template>

<script>
import store from '../../store/store'
export default {
  mounted () {
    const haha = this.$refs.box
    const hehe = this.$refs.hehe.validate
    console.log(haha)
    console.log(hehe)
  },
  data () {
    const checkMobilk = (rule, value, callback) => {
      if (!/^1[3-9]\d{9}$/.test(value)) {
        return callback(new Error('手机号格式不正确'))
      }
      callback()
    }
    return {
      loginForm: {
        mobile: '18888888888',
        code: '246810'
      },
      loginRules: {
        mobile: [
          { required: true, message: '请输入正确的手机号', trigger: 'blur' },
          { validator: checkMobilk, trigger: 'change' }
        ],
        code: [
          { required: true, message: '请输入正确的验证码', trigger: 'blur' },
          { len: 6, message: '验证码长度为6位', trigger: 'blur' }
        ]
      }
    }
  },
  methods: {
    login () {
      this.$refs.hehe.validate((valid) => {
        if (valid) {
          this.$http.post('authorizations', this.loginForm)
            .then(res => {
              store.setUser(res.data.data)
              this.$router.push('/')
            })
            .catch(() => {
              this.$message.error('弄错了')
            })
        }
      })
    }
  }

}
</script>

<style scoped lang='less'>
.container{
  background: url(../../assets/imgs/login_bg.jpg) no-repeat center / cover;
  width: 100%;
  height: 100%;
  position: absolute;
  left: 0;
  top: 0;
  .my-card{
    width: 400px;
    height: 350px;
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%,-50%);
    img{
      width: 200px;
      display: block;
      margin: 0 auto 15px;
    }
  }
}
</style>
