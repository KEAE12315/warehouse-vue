<template>
  <el-menu
    :default-openeds="['1', '3']"
    style="height: 100%; overflow-x: hidden"
    background-color="rgb(48,65,86)"
    text-color="#ffff"
    active-text-color="#ffd04b"
    :collapse-transition="false"
    :collapse="isCollapse"
    class="el-el-menu-vertical-demo"
    router
  >
    <div style="height: 60px; line-height: 60px; text-align: center; display: flex; justify-content: center; align-items: center">
      <img src="../assets/logo.png" alt="" style="width: 20px; height: 20px" />
      <b style="color: white; margin-left: 10px" v-show="!isCollapse">后台管理系统</b>
    </div>
    <el-menu-item index="/">
      <template slot="title">
        <i class="el-icon-house"></i>
        <span slot="title">主页</span>
      </template>
    </el-menu-item>
    <el-submenu index="2">
      <template slot="title">
        <i class="el-icon-menu"></i>
        <span slot="title">系统管理</span>
      </template>
      <el-menu-item index="/manage/user">用户管理</el-menu-item>
    </el-submenu>
  </el-menu>
</template>

<script>
export default {
  name: 'Aside',
  props: {
    isCollapse: Boolean,
  },
};
</script>

<style>
</style>