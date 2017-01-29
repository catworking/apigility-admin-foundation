<template>
  <div>
    <ListContainer title="广告图"
                   resource-uri="ad/banner"
                   resource-name="banner"
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
            prop="title"
            label="标题">
          </el-table-column>

          <el-table-column
            label="链接">
            <template scope="scope">
              <a :href="scope.row.link" :title="scope.row.title" style="color: black">{{scope.row.link}}</a>
            </template>
          </el-table-column>

          <el-table-column
            label="图片">
            <template scope="scope">
              <div style="padding: .5rem 0;">
                <a :href="scope.row.link" :title="scope.row.title" style="color: black"><img :src="scope.row.image" style="display: block; width: 100%"></a>
              </div>
            </template>
          </el-table-column>

          <el-table-column
            label="操作">
            <template scope="scope">
              <el-button-group>
                <el-button type="primary"
                           size="small"
                           icon="edit"
                           @click="showBanner(scope.row.title, scope.row.id)">
                  编辑
                </el-button>
                <el-button type="danger"
                           size="small"
                           icon="delete">删除</el-button>
              </el-button-group>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </ListContainer>

    <el-dialog v-bind:title="bannerView.title" v-model="bannerView.visible" size="small">
      <div style="width: 100%; overflow: hidden;" v-if="bannerView.visible">
        <Banner v-bind:banner-id="bannerView.bannerId"></Banner>
      </div>
    </el-dialog>
  </div>
</template>
<style>

</style>
<script>
  import ListContainer from '../ListContainer.vue'
  import Banner from './Banner.vue'

  export default {
    data () {
      return {
        items: [],
        bannerView: {
          visible: false,
          title: null,
          bannerId: null
        }
      }
    },
    methods: {
      onFetched: function (data) {
        this.items = data
      },
      showBanner: function (title, bannerId) {
        this.bannerView.title = title
        this.bannerView.bannerId = bannerId
        this.bannerView.visible = true
      }
    },
    components: {
      ListContainer, Banner
    }
  }
</script>
