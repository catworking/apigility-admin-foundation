<template>
  <div id="app">
    <transition
      mode="out-in"
      enter-active-class="fadeIn"
      leave-active-class="fadeOut"
      appear>
      <router-view class="animated"></router-view>
    </transition>
  </div>
</template>

<script>
export default {
  name: 'app'
}
</script>

<style>
  html,body, div#root, div#app {
    width:100%;
    height: 100%;
    background: #324057;
  }
  a { color: white; text-decoration: none;}
</style>
