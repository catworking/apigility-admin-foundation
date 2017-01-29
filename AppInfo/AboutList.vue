<template>
  <div>
    <ListContainer title="简介"
                   resource-uri="appinfo/about"
                   resource-name="about"
                   v-on:fetched="onFetched">
      <div slot="items">
        <el-table
          :data="items"
          stripe
          style="width: 100%">
          <el-table-column
            prop="id"
            label="ID" width="60">
          </el-table-column>
          <el-table-column
            prop="app_name"
            label="应用名称" width="120">
          </el-table-column>

          <el-table-column
            prop="enterprise_name"
            label="企业名称">
          </el-table-column>

          <el-table-column
            prop="customer_service_tel"
            label="客服电话">
          </el-table-column>

          <el-table-column
            label="操作">
            <template scope="scope">
              <el-button type="primary"
                         size="small"
                         icon="edit"
                         @click="showAbout(scope.row.app_name, scope.row.id)">
                编辑
              </el-button>
            </template>
          </el-table-column>

        </el-table>
      </div>
    </ListContainer>

    <el-dialog v-bind:title="aboutView.title" v-model="aboutView.visible" size="full">
      <div style="width: 100%; overflow: hidden;" v-if="aboutView.visible">
        <About v-bind:about-id="aboutView.aboutId"></About>
      </div>
    </el-dialog>
  </div>
</template>
<style>

</style>
<script>
  import ListContainer from '../ListContainer.vue'
  import About from './About.vue'

  export default {
    data () {
      return {
        items: [],
        aboutView: {
          visible: false,
          title: null,
          aboutId: null
        }
      }
    },
    methods: {
      onFetched: function (data) {
        this.items = data
      },
      showAbout: function (title, aboutId) {
        this.aboutView.title = title
        this.aboutView.aboutId = aboutId
        this.aboutView.visible = true
      }
    },
    components: {
      ListContainer, About
    }
  }
</script>
