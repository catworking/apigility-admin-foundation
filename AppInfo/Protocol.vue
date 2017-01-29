<template>
  <div class="wrap">

    <ItemContainer ref="Protocol"
                   resourceUri="appinfo/protocol"
                   v-bind:resourceId="protocolId"
                   resourceName="protocol"
                   v-on:fetched="onFetched">
      <div slot="dataView">

        <el-form ref="form" :model="protocolData" label-width="80px">

          <el-form-item label="标题">
            <el-input v-model="protocolData.title"></el-input>
          </el-form-item>

          <el-form-item label="内容">
            <quill-editor ref="myTextEditor"
                          v-model="protocolData.content"
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
        protocolData: {
          title: null,
          content: null
        },
        editorOption: {
        }
      }
    },
    props: ['protocolId'],
    methods: {
      onFetched: function (data) {
        console.log(data)
        this.protocolData = data
      },
      onSubmit (event) {
        console.log(event, this.protocolData)

        // 不能为空
        if (!this.protocolData.title) {
          this.$message('请完整填写表单')
          return false
        }

        if (this.protocolId) this.$refs.Protocol.patchData(this.protocolData)
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
        this.protocolData.content = html
      }
    },
    components: {
      ItemContainer
    }
  }
</script>
