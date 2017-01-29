<template>
  <div class="wrap">

    <ItemContainer ref="Banner"
                   resourceUri="ad/banner"
                   v-bind:resourceId="bannerId"
                   resourceName="banner"
                   v-on:fetched="onFetched">
      <div slot="dataView">

        <el-form ref="form" :model="bannerData" label-width="80px">

          <el-form-item label="名称">
            <el-input v-model="bannerData.title"></el-input>
          </el-form-item>

          <el-form-item label="链接">
            <el-input v-model="bannerData.link"></el-input>
          </el-form-item>

          <el-form-item label="图片">
            <el-upload
              name="data"
              :data="{type:1}"
              :action="uploadUrl"
              type="drag"
              accept="image/*"
              :thumbnail-mode="true"
              :on-success="uploadSuccess"
              :on-preview="handlePreview"
              :on-remove="handleRemove"
              :default-file-list="fileList">
              <i class="el-icon-upload"></i>
              <div class="el-dragger__text">将文件拖到此处，或<em>点击上传</em></div>
              <div class="el-upload__tip" slot="tip">只能上传jpg/png文件，且不超过500kb</div>
            </el-upload>
          </el-form-item>

          <el-form-item label="广告位置">
            <el-checkbox-group v-model="bannerDataPositions">
              <el-checkbox v-for="item in positionListData"
                           :label="item.id">{{item.name}}
              </el-checkbox>
            </el-checkbox-group>
          </el-form-item>

          <el-form-item>
            <el-button type="primary" @click="onSubmit">保存</el-button>
          </el-form-item>
        </el-form>

      </div>
    </ItemContainer>

    <div style="display: none">
      <ListContainer ref="PositionList"
                     title="广告位置"
                     resource-uri="ad/position"
                     resource-name="position"
                     v-on:fetched="onPositionListFetched">
      </ListContainer>
    </div>

  </div>
</template>

<style scoped>
</style>

<script>
  import ItemContainer from '../ItemContainer.vue'
  import ListContainer from '../ListContainer.vue'

  export default {
    data () {
      return {
        bannerData: {
          title: null,
          link: null,
          image: null,
          positions: []
        },
        positionListData: null
      }
    },
    computed: {
      uploadUrl () {
        return this.$store.state.config.ApiHostUrl + '/object-storage/file'
      },
      fileList () {
        if (this.bannerData.link) return [{name: this.bannerData.title, url: this.bannerData.image}]
        else return []
      },
      bannerDataPositions: {
        get () {
          let values = []
          for (let index in this.bannerData.positions) {
            values.push(this.bannerData.positions[index].id)
          }

          return values
        },
        set (values) {
          let newData = []

          for (let index in values) {
            for (let dataIndex in this.positionListData) {
              if (this.positionListData[dataIndex].id === values[index]) {
                newData.push(this.positionListData[dataIndex])
              }
            }
          }

          this.bannerData.positions = newData
        }
      }
    },
    props: ['bannerId'],
    methods: {
      onFetched: function (data) {
        console.log(data)
        this.bannerData = data
      },
      onPositionListFetched: function (data) {
        console.log(data)
        this.positionListData = data
      },
      onPosted: function (data) {
        console.log(data)
        this.bannerData = data
        this.bannerData = this.bannerData.id
      },
      onSubmit (event) {
        console.log(event, this.bannerData)

        // 不能为空
        if (!this.bannerData.title || !this.bannerData.link || !this.bannerData.image || this.bannerData.positions.length === 0) {
          this.$message('请完整填写表单')
          return false
        }

        if (this.bannerId) this.$refs.Banner.patchData(this.bannerData)
        else this.$refs.Banner.postData(this.bannerData)
      },
      uploadSuccess (response, file, fileList) {
        this.bannerData.image = response.uri
      }
    },
    components: {
      ItemContainer, ListContainer
    }
  }
</script>

