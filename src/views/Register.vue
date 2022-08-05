<template>
  <div class="wrapper">
    <div class="mainBox">
      <div style="margin: 20px 0; text-align: center; font-size: 24px"><b>注册</b></div>
      <el-form :model="user" :rules="rules" ref="userForm">
        <el-form-item prop="username">
          <el-input
            size="medium"
            class="inputBox"
            prefix-icon="el-icon-user"
            placeholder="请输入用户名"
            v-model="user.username"
          ></el-input>
        </el-form-item>

        <el-form-item prop="password">
          <el-input
            size="medium"
            class="inputBox"
            prefix-icon="el-icon-lock"
            placeholder="请输入密码"
            show-password
            v-model="user.password"
          ></el-input>
        </el-form-item>

        <el-form-item prop="confirmPassword">
          <el-input
            size="medium"
            class="inputBox"
            prefix-icon="el-icon-lock"
            placeholder="请确认密码"
            show-password
            v-model="user.confirmPassword"
          ></el-input>
        </el-form-item>

        <el-form-item prop="email">
          <el-input
            size="medium"
            class="inputBox"
            prefix-icon="el-icon-message"
            placeholder="请输入邮箱"
            v-model="user.email"
          ></el-input>
        </el-form-item>

        <el-form-item prop="phone">
          <el-input
            size="medium"
            class="inputBox"
            prefix-icon="el-icon-phone"
            placeholder="请输入手机号"
            v-model="user.phone"
          ></el-input>
        </el-form-item>

        <el-form-item prop="address">
          <el-input
            size="medium"
            class="inputBox"
            prefix-icon="el-icon-lock"
            placeholder="请输入地址"
            v-model="user.address"
          ></el-input>
        </el-form-item>

        <el-form-item style="margin: 25px 0 0 0; text-align: right">
          <el-button type="primary" size="small" autocomplete="off" @click="register">确认</el-button>
          <el-button type="warning" size="small" autocomplete="off" @click="$router.push('/login')">返回登录</el-button>
        </el-form-item>
      </el-form>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Register',
  data() {
    return {
      user: {},
      rules: {
        username: [
          { required: true, message: '请输入用户名', trigger: 'blur' },
          { min: 1, max: 20, message: '长度在 1 到 20 个字符', trigger: 'blur' },
        ],
        nickname: [
          { required: true, message: '请输入昵称', trigger: 'blur' },
          { min: 1, max: 10, message: '长度在 1 到 5 个字符', trigger: 'blur' },
        ],
        password: [
          { required: true, message: '请输入密码', trigger: 'blur' },
          { min: 1, max: 20, message: '长度在 1 到 20 个字符', trigger: 'blur' },
        ],
        confirmPassword: [
          { required: true, message: '请再次输入密码', trigger: 'blur' },
          { min: 1, max: 20, message: '长度在 1 到 20 个字符', trigger: 'blur' },
        ],
      },
    };
  },
  methods: {
    register() {
      this.$refs['userForm'].validate((valid) => {
        if (valid) {
          if (this.user.password != this.user.confirmPassword) {
            this.$message.error('两次输入密码不一致');
            return false;
          }
          this.request.post('/manage/user/register', this.user).then((res) => {
            if (res.code == '200') {
              this.$router.push('/login');
              this.$message.success('注册成功');
            } else {
              this.$message.error(res.msg);
            }
          });
        } else {
          return false;
        }
      });
    },
  },
};
</script>

<style>
.wrapper {
  height: 100vh;
  background-image: linear-gradient(to bottom right, #fc466b, #3f5efb);
  overflow: hidden;
}

.mainBox {
  margin: 200px auto;
  background-color: #fff;
  width: 350px;
  padding: 20px;
  border-radius: 10px;
}

.inputBox {
  margin: 10px, 0;
}
</style>