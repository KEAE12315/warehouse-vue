<template>
  <div style="font-size: 12px; line-height: 60px; display: flex">
    <div style="flex: 1; font-size: 20px">
      <span :class="collapseBtnClass" style="cursor: pointer" @click="collapse"></span>

      <el-breadcrumb separator="/" style="display: inline-block; margin-left: 20px; font-size: 15px">
        <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
        <el-breadcrumb-item>用户管理</el-breadcrumb-item>
      </el-breadcrumb>
    </div>

    <el-dropdown style="cursor: pointer">
      <span>{{ nickname }}</span>
      <i class="el-icon-arrow-down" style="margin-left: 5px; font-size: 5px"></i>
      <el-dropdown-menu slot="dropdown">
        <el-dropdown-item>个人信息</el-dropdown-item>
        <el-dropdown-item><span @click="logout">退出账号</span></el-dropdown-item>
      </el-dropdown-menu>
    </el-dropdown>
  </div>
</template>

<script>
export default {
  name: 'Header',
  data() {
    return {
      user: localStorage.getItem('user') ? JSON.parse(localStorage.getItem('user')) : {},
    };
  },
  computed: {
    nickname() {
      if (this.user.nickname) {
        return this.user.nickname;
      } else {
        return '<默认昵称>';
      }
    },
  },
  props: {
    collapseBtnClass: String,
    collapse: Function,
  },
  methods: {
    logout() {
      this.$router.push('/login');
      localStorage.removeItem('user');
      this.$message.success('退出成功');
    },
  },
};
</script>

<style>
a {
  text-decoration: none;
}
.router-link-active {
  text-decoration: none;
}
</style>