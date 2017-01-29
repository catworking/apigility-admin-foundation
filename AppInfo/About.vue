<template>
  <div class="wrap">

    <ItemContainer ref="About"
                   resourceUri="appinfo/about"
                   v-bind:resourceId="aboutId"
                   resourceName="about"
                   v-on:fetched="onFetched">
      <div slot="dataView">

        <el-form ref="form" :model="aboutData" label-width="80px">

          <el-form-item label="应用名称">
            <el-input v-model="aboutData.app_name"></el-input>
          </el-form-item>

          <el-form-item label="客服电话">
            <el-input v-model="aboutData.customer_service_tel"></el-input>
          </el-form-item>

          <el-form-item label="反馈邮箱">
            <el-input v-model="aboutData.feedback_email"></el-input>
          </el-form-item>

          <el-form-item label="企业名称">
            <el-input v-model="aboutData.enterprise_name"></el-input>
          </el-form-item>

          <el-form-item label="企业地址">
            <el-input v-model="aboutData.enterprise_address"></el-input>
          </el-form-item>

          <el-form-item label="内容">
            <quill-editor ref="myTextEditor"
                          v-model="aboutData.content"
                          :config="editorOption"
                          @blur="onEditorBlur($event)"
                          @focus="onEditorFocus($event)"
                          @ready="onEditorReady($event)">
            </quill-editor>
          </el-form-item>

          <el-form-item>
            <el-button type="primary" @click="onSubmit">保存</el-button>
          </el-form-item>
        </el-form>

      </div>
    </ItemContainer>
  </div>

  </div>
</template>

<style scoped>
</style>

<script>
  import ItemContainer from '../ItemContainer.vue'

  export default {
    data () {
      return {
        aboutData: {
          app_name: null,
          customer_service_tel: null,
          feedback_email: null,
          enterprise_name: null,
          enterprise_address: null,
          content: null
        },
        editorOption: {
        }
      }
    },
    props: ['aboutId'],
    methods: {
      onFetched: function (data) {
        console.log(data)
        this.aboutData = data
      },
      onSubmit (event) {
        console.log(event, this.aboutData)

        // 不能为空
        if (!this.aboutData.app_name) {
          this.$message('请完整填写表单')
          return false
        }

        if (this.aboutId) this.$refs.About.patchData(this.aboutData)
      },
      onEditorBlur (editor) {
        console.log('editor blur!', editor)
      },
      onEditorFocus (editor) {
        console.log('editor focus!', editor)
      },
      onEditorReady (editor) {
        console.log('editor ready!', editor)
      },
      onEditorChange ({ editor, html, text }) {
        // console.log('editor change!', editor, html, text)
        this.aboutData.content = html
      }
    },
    components: {
      ItemContainer
    }
  }
</script>
