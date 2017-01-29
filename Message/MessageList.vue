<template>
  <div>
    <ListContainer title="消息"
                   resource-uri="message/message"
                   resource-name="message"
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
            label="消息内容">
            <template scope="scope">
              {{scope.row.text}}
              <el-button
                type="text"
                @click="showMessage('消息详情', scope.row.id)">
                查看详情
              </el-button>
            </template>
          </el-table-column>
          <el-table-column
            prop="create_time"
            label="创建时间"
            :formatter="timeFormatter">
          </el-table-column>
          <el-table-column
            label="创建人">
            <template scope="scope">
              <el-button
                type="text"
                @click="showUser(scope.row.user.nickname, scope.row.user.id)">
                {{scope.row.user.nickname}}
              </el-button>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </ListContainer>

    <el-dialog v-bind:title="userView.title" v-model="userView.visible" size="small">
      <div style="width: 100%; overflow: hidden;">
        <User v-if="userView.visible" v-bind:user-id="userView.userId"></User>
      </div>
    </el-dialog>

    <el-dialog v-bind:title="messageView.title" v-model="messageView.visible" size="small">
      <div style="width: 100%; overflow: hidden;">
        <Message v-if="messageView.visible" v-bind:message-id="messageView.messageId"></Message>
      </div>
    </el-dialog>
  </div>
</template>
<style>

</style>
<script>
  import ListContainer from '../ListContainer.vue'
  import moment from 'moment'
  import User from '../User/User.vue'
  import Message from './Message.vue'

  export default {
    data () {
      return {
        items: [],
        userView: {
          visible: false,
          title: null,
          userId: null
        },
        messageView: {
          visible: false,
          title: null,
          messageId: null
        }
      }
    },
    methods: {
      onFetched: function (data) {
        this.items = data
      },
      timeFormatter (row, column) {
        return moment.unix(row.create_time).format('YYYY-MM-DD HH:mm:ss')
      },
      showUser: function (title, userId) {
        this.userView.title = title
        this.userView.userId = userId
        this.userView.visible = true
      },
      showMessage: function (title, messageId) {
        this.messageView.title = title
        this.messageView.messageId = messageId
        this.messageView.visible = true
      }
    },
    components: {
      ListContainer, User, Message
    }
  }
</script>
