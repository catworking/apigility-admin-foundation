<template>
    <div id="layout">
      <div class="toolbar_wrap"><catwork-toolbar></catwork-toolbar></div>
      <div class="mainRow_wrap">
        <el-row class="mainRow">
          <el-col :xs="{span:0}" :sm="{span:6}" :md="{span:5}" :lg="{span:4}">
            <div>
              <catwork-menu></catwork-menu>
            </div>
          </el-col>
          <el-col :xs="{span:24}" :sm="{span:18}" :md="{span:19}" :lg="{span:20}">
            <div class="mainContent">
              <transition
                mode="out-in"
                enter-active-class="fadeIn"
                leave-active-class="fadeOut"
                appear>
                  <router-view class="animated"></router-view>
              </transition>
            </div>
          </el-col>
        </el-row>
      </div>
    </div>
</template>
<style scoped>
  div#layout, div.mainRow, div.mainRow_wrap, div.el-col, div.mainContent {
    min-height: 100%;
  }
  div.mainRow_wrap {
    padding-top: 2.5rem;
  }
  div.mainContent {
    background-color : #FFFFFF;
    padding: 24px;
  }
  div.toolbar_wrap {
    position: fixed;
    z-index: 999;
    width: 100%;
  }
</style>
<script>
  import CatworkMenu from './Menu.vue'
  import CatworkToolbar from './Toolbar.vue'
  export default {
    name: 'app',
    components: {
      CatworkMenu,
      CatworkToolbar
    },
    created: function () {
      if (!this.$store.state.login.status) {
        this.$router.push('/login')
      }
    }
  }
</script>
