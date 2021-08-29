<template>
  <div>
    <el-container>
      <el-header style="height: 50px;">
        <Header></Header>
      </el-header>
      <el-container style="height: calc(100vh - 50px);">
        <el-aside style="width: 250px; background-color: antiquewhite">
          <Aside></Aside>
        </el-aside>
        <el-main>
          <Main></Main>
        </el-main>
      </el-container>
<!--      <div style="display: flex;">-->
<!--        -->
<!--        <router-view style="flex: 1; background-color: #cccccc"></router-view>-->
<!--      </div>-->
    </el-container>
  </div>
</template>

<script>
import Header from '@/components/Header';
import Aside from "./Aside";
import Main from "./Main";
export default {
  name: 'Home',
  components: {Aside, Header, Main},
  data () {
    return {
      msg: 'Welcome to Your Vue.js App'
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>

</style>
