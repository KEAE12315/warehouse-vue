<template>
  <el-container style="height: 100vh">
    <el-aside
      :width="sideWidth + 'px'"
      style="background-color: rgb(238, 241, 246); height=100%; box-shadow:2px 0 6px rgb(0 21 41)"
    >
      <Aside :isCollapse="isCollapse"></Aside>
    </el-aside>

    <el-container>
      <el-header style="border-bottom: 1px solid #dcdcdc; justify-content: space-between">
        <Header :collapseBtnClass="collapseBtnClass" :collapse="collapse"></Header>
      </el-header>

      <el-main> <router-view /> </el-main>
    </el-container>
  </el-container>
</template>

<script>
// @ is an alias to /src
import Aside from '@/components/Aside.vue';
import Header from '@/components/Header.vue';

export default {
  name: 'HomeView',
  data() {
    return {
      // setting of sidebar
      collapseBtnClass: 'el-icon-s-fold',
      isCollapse: false,
      sideWidth: 200,
    };
  },

  components: {
    Aside,
    Header,
  },
  methods: {
    collapse() {
      this.isCollapse = !this.isCollapse;
      if (this.isCollapse) {
        this.sideWidth = 64;
        this.collapseBtnClass = 'el-icon-s-unfold';
      } else {
        this.sideWidth = 200;
        this.collapseBtnClass = 'el-icon-s-fold';
      }
    },
  },
  created() {
    // console.log(this.$route);
  },
};
</script>
