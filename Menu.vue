<template>
  <div>
    <el-menu default-active="2" class="el-menu-vertical-demo" @open="handleOpen" @close="handleClose" theme="dark">

      <router-link to="/workbench/dashboard">
        <el-menu-item index="1"><i class="el-icon-information"></i>数据统计</el-menu-item>
      </router-link>

      <router-link to="/workbench/social/person-list">
        <el-menu-item index="4"><i class="el-icon-document"></i>用户管理</el-menu-item>
      </router-link>

      <router-link to="/workbench/vip/contract-list">
        <el-menu-item index="5"><i class="el-icon-date"></i>年费管理</el-menu-item>
      </router-link>

      <el-submenu index="6">
        <template slot="title"><i class="el-icon-picture"></i>广告管理</template>
        <el-menu-item-group>
          <template slot="title">广告图</template>
          <router-link to="/workbench/ad/banner-list">
            <el-menu-item index="6-1">广告图列表</el-menu-item>
          </router-link>
          <router-link to="/workbench/ad/banner">
            <el-menu-item index="6-2">新增广告图</el-menu-item>
          </router-link>
        </el-menu-item-group>

        <el-menu-item-group>
          <template slot="title">广告位置</template>
          <router-link to="/workbench/ad/position-list">
            <el-menu-item index="6-3">位置列表</el-menu-item>
          </router-link>
        </el-menu-item-group>
      </el-submenu>

      <el-submenu index="7">
        <template slot="title"><i class="el-icon-message"></i>系统消息</template>
        <router-link to="/workbench/message/message-list">
          <el-menu-item index="7-1">消息列表</el-menu-item>
        </router-link>
        <router-link to="/workbench/message/message">
          <el-menu-item index="7-2">新增消息</el-menu-item>
        </router-link>
      </el-submenu>

      <router-link to="/workbench/feedback/feedback-list">
        <el-menu-item index="8"><i class="el-icon-edit"></i>反馈管理</el-menu-item>
      </router-link>

      <el-submenu index="9">
        <template slot="title"><i class="el-icon-setting"></i>应用信息</template>

        <router-link to="/workbench/app-info/protocol-list">
          <el-menu-item index="9-1">协议管理</el-menu-item>
        </router-link>

        <router-link to="/workbench/app-info/about-list">
          <el-menu-item index="9-3">简介管理</el-menu-item>
        </router-link>
      </el-submenu>

      </router-link>

    </el-menu>
  </div>
</template>
<style>

</style>
<script>
  export default {
    methods: {
      handleOpen (key, keyPath) {
        console.log(key, keyPath)
      },
      handleClose (key, keyPath) {
        console.log(key, keyPath)
      }
    }
  }
</script>
