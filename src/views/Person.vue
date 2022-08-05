<template>
  <div>
    <el-form :model="formUser" :label-width="formLabelWidth" size="small">
      <el-form-item label="用户名">
        <el-input v-model="formUser.username" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="密码">
        <el-input v-model="formUser.passwrod" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="昵称">
        <el-input v-model="formUser.nickname" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="邮箱">
        <el-input v-model="formUser.email" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="手机">
        <el-input v-model="formUser.phone" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="地址">
        <el-input v-model="formUser.address" autocomplete="off"></el-input>
      </el-form-item>
    </el-form>
    <div slot="footer" class="dialog-footer">
      <el-button @click="dialogFormVisible = false">取 消</el-button>
      <el-button type="primary" @click="save">确 定</el-button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'Person',
  data() {
    return {
      formUser: {},
      user: localStorage.getItem('user') ? JSON.parse(localStorage.getItem('user')) : {},
    };
  },
  created() {
    try {
      this.request.get('/manage/user/username/' + this.user.username).then((res) => {
        switch (res.code) {
          case '200':
            this.formUser = res.data;
            break;
          case '400':
            localStorage.removeItem('user');
            this.$router.push('/login');
            this.$message.error('用户不存在，已返回登录界面');
            break;
          default:
            break;
        }
      });
    } catch (e) {
      this.$router.push('/login');
      this.$message.error('请求失败，已返回登录界面');
    }
  },
  methods: {},
};
</script>

<style>
</style>