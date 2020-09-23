<template>
  <div CLASS="login_container">
    <div CLASS="login_box">
      <!--头像开始-->
      <div class="avatar_box">
          <img src="../assets/logo.png" alt="">
      </div>
      <!--头像结束-->
      <!--登录开始-->
      <!--
      参数说明：
      :model实现数据绑定
      -->
      <el-form ref="loginFormRef" :model="loginForm" :rules="loginFormRules"  label-width="0px" class="login_form">
        <!--用户名结构开始-->
        <el-form-item prop="username">
          <el-input v-model="loginForm.username" prefix-icon="iconfont icon-user"></el-input>
        </el-form-item>
        <!--用户名结构结束-->
        <!--用户密码结构开始-->
        <el-form-item prop="password">
          <el-input v-model="loginForm.password" prefix-icon="iconfont icon-3702mima" type="password"></el-input>
        </el-form-item>
        <!--用户密码结构结束-->

        <!--按钮结构开始-->
        <el-form-item class="btns">
          <el-button type="primary" @click="login">登录</el-button>
          <el-button type="info" @click="resetLoginForm">重置</el-button>
        </el-form-item>
        <!--按钮结构结束-->
      </el-form>
      <!--登录结束-->
    </div>
  </div>
</template>

<script>
export default {
  // 行为区
  data () {
    return {
      // 登录表单数据数据绑定开始
      loginForm: {
        // 绑定数据属性username
        username: 'admin',
        // 绑定数据数据属性password
        password: '123456'
      },
      // 登录表单数据数据绑定结束
      // 表单验证规则对象开始
      loginFormRules: {
        // 验证用户名是否合法
        username: [
          { required: true, message: '请输入登录名称', trigger: 'blur' },
          { min: 3, max: 5, message: '长度在3到5个字符', trigger: 'blur' }
        ],
        // 验证密码是否合法
        password: [
          { required: true, message: '请输入登录密码', trigger: 'blur' },
          { min: 6, max: 15, message: '长度在6到15个字符', trigger: 'blur' }
        ]
      }
      // 表单验证规则对象结束
    }
  },
  methods: {
    // 点击重重按钮，重置登录菜单
    resetLoginForm () {
      this.$refs.loginFormRef.resetFields()
    },
    login: function () {
      this.$refs.loginFormRef.validate(async valid => {
        // console.log(valid)
        if (!valid) return
        const { data: res } = await this.$http.post('login', this.loginForm)
        // console.log(res)
        // if (res.meta.status !== 200) return console.log('登录失败')
        if (res.meta.status !== 200) return this.$message.error('登录失败')
        // console.log('登录成功')
        this.$message.success('登录成功')
        // 登录成功保存session
        window.sessionStorage.setItem('token', res.data.token)
        // 登录成功后跳转到后台首页
        this.$router.push('/home')
      })
    }
  }
}
</script>
<!--scoped主要防止组件的样式冲突-->
<style lang="less" scoped>
  .login_container {
    background-color: #2b4b6b;
    height: 100%;
  }
  .login_box {
    width: 450px;
    height: 300px;
    background-color: #fff;
    border-radius: 3px;
  //  设置绝对定位
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%,-50%);
    .avatar_box{
      height: 130px;
      width: 130px;
      border: 1px solid #eee;
      border-radius: 50%;
      padding: 10px;
      box-shadow: 0 0 10px #ddd;
      position: absolute;
      left: 50%;
      transform: translate(-50%,-50%);
      background-color: #fff;

      img{
        width: 100%;
        height: 100%;
        border-radius: 50%;
        background-color: #eee;
      }
    }
    /*表单样式开始*/
    .login_form{
      position: absolute;
      bottom: 0;
      width: 100%;
      padding: 0 25px;
      box-sizing: border-box;
    }
    /*表单样式结束*/
    /*按钮样式开始*/
    .btns{
      display: flex;
      justify-content: flex-end;
    }
    /*按钮样式结束*/
  }
</style>
