<template>
  <div>
    <ItemContainer ref="Message"
                   resourceUri="message/message"
                   v-bind:resourceId="messageId"
                   resourceName="message"
                   v-on:fetched="onFetched"
                   v-on:posted="onPosted">
      <div slot="dataView">
        <el-form ref="form" :model="MessageData" label-width="80px">

          <el-form-item label="消息内容">
            <el-input
              type="textarea"
              :autosize="{ minRows: 4, maxRows: 8}"
              placeholder="请输入内容"
              v-model="MessageData.text">
            </el-input>
          </el-form-item>

          <el-form-item v-if="!messageId">
            <el-button type="primary" @click="onSubmit">保存</el-button>
          </el-form-item>

          <el-form-item v-if="messageId" label="已发送">
            {{sentCount}} 个目标
          </el-form-item>

          <el-form-item v-if="messageId">
            <el-checkbox v-model="sendToAllUser" checked>向所有用户广播消息(重复广播时，只会向未发送过的用户发送)</el-checkbox>
          </el-form-item>

          <el-form-item v-if="messageId">
            <el-button type="primary" @click="onSend">发送</el-button>
          </el-form-item>

        </el-form>

        <div style="display: none">
          <ListContainer ref="Send"
                         v-if="sendDataVIf"
                         title="发送列表"
                         resource-uri="message/send"
                         resource-name="send"
                         :params="sendDataParams"
                         v-on:fetched="onSendFetched"></ListContainer>
          <ItemContainer ref="SendMessage"
                         resourceUri="message/send"
                         resourceName="send"
                         v-on:posted="onSendMessagePosted"></ItemContainer>
        </div>

      </div>
    </ItemContainer>
  </div>
</template>
<style>

</style>
<script>
  import ItemContainer from '../ItemContainer.vue'
  import ListContainer from '../ListContainer.vue'

  export default {
    data () {
      return {
        MessageData: {
          text: null
        },
        sendData: null,
        sendDataVIf: true,
        sendDataTotal: null,
        sendToAllUser: true
      }
    },
    props: ['messageId'],
    computed: {
      sentCount () {
        if (this.sendData && this.messageId) return parseInt(this.sendDataTotal)
        else return 0
      },
      sendDataParams () {
        return {
          message_id: this.messageId
        }
      }
    },
    methods: {
      onFetched: function (data) {
        console.log(data)
        this.MessageData = data
      },
      onPosted: function (data) {
        console.log(data)
        this.MessageData = data
        this.messageId = this.MessageData.id
      },
      onSubmit (event) {
        console.log(event, this.MessageData)
        this.sendDataVIf = false
        if (this.messageId) this.$refs.Message.patchData(this.MessageData)
        else this.$refs.Message.postData(this.MessageData)
      },
      onSendFetched (data) {
        this.sendData = data
        this.sendDataTotal = this.$refs.Send.total_items
        this.sendDataVIf = true
      },
      onSend (event) {
        console.log(event)
        if (this.sendToAllUser) {
          this.sendDataVIf = false
          this.$refs.SendMessage.postData({
            message_id: this.messageId,
            user_filters: {}
          })
        }
      },
      onSendMessagePosted (data) {
        console.log(data)
        // 刷新组件
        this.sendDataVIf = true
      }
    },
    components: {
      ItemContainer, ListContainer
    }
  }
</script>
