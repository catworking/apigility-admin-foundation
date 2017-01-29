<template>
  <div class="toolbar">
    <div class="title">「{{appName}}」管理后台</div>
    <div class="commands"><span>{{loginAdministratorName}}</span><span><router-link to="/login">退出登录</router-link></span></div>
  </div>
</template>
<style scoped>
  div.toolbar {
    height: 2.5rem;
    line-height: 2.5rem;
    background:#000;
    color:#fff;
    padding: 0 1rem;
  }
  div.title {
    float: left;
  }
  div.commands {
    float: right;
  }
  div.commands span {
    display: inline-block;
    padding-left: 1rem;
  }
</style>
<script>
  export default {
    data () {
      return {}
    },
    computed: {
      appName () {
        return this.$store.state.config.AppName
      },
      loginAdministratorName () {
        if (this.$store.state.login.administrator) return this.$store.state.login.administrator.name
        else return ''
      }
    }
  }
</script>
