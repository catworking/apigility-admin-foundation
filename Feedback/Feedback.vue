<template>
  <div class="wrap">

    <ItemContainer resourceUri="feedback/feedback"
                   v-bind:resourceId="feedbackId"
                   resourceName="feedback"
                   v-on:fetched="onFetched">
      <div slot="dataView">

        <div v-if="feedbackData">
          <h4>文本内容：</h4>
          <div v-if="feedbackData.article.content">{{feedbackData.article.content}}</div>
          <div v-else>反馈者没有填写文本内容</div>

          <div v-if="feedbackData.attached_object_type">
            <h4>反馈目标类型：{{feedbackData.attached_object_type | attachedObjectTypeName}}</h4>
          </div>

          <div v-if="feedbackData.attached_object_id">
            <h4>反馈目标：
              <el-button
                type="text"
                @click="showUser(personName, personUserId)">
                {{personName}}
              </el-button>
            </h4>
            <div style="width: 100%; overflow: hidden;" v-if="userView.visible">
              <User v-if="userView.userId" v-bind:user-id="userView.userId"></User>
            </div>
          </div>

          <div v-if="feedbackData.create_time">
            <br>
            <h4>反馈时间：{{feedbackData.create_time | timeFormatter}}</h4>
          </div>

        </div>

        <div v-else>没有数据</div>
      </div>
    </ItemContainer>

    <div style="display: none">
      <ItemContainer v-if="personalResourceId"
                     title="社交人"
                     ref="Person"
                     resourceUri="social/person"
                     v-bind:resourceId="personalResourceId"
                     resourceName="person"
                     v-on:fetched="onPersonFetched">
      </ItemContainer>
    </div>

  </div>
</template>

<style scoped>
</style>

<script>
  import ItemContainer from '../ItemContainer.vue'
  import moment from 'moment'
  import User from '../User/User.vue'

  export default {
    data () {
      return {
        feedbackData: null,
        personData: null,
        userView: {
          visible: false,
          title: null,
          userId: null
        }
      }
    },
    props: ['feedbackId'],
    computed: {
      personalResourceId () {
        if (this.feedbackData && this.feedbackData.attached_object_id) return this.feedbackData.attached_object_id
        else return null
      },
      personName () {
        if (this.personData) return this.personData.user.nickname
        else return ''
      },
      personUserId () {
        if (this.personData) return this.personData.user.id
        else return null
      }
    },
    methods: {
      onFetched: function (data) {
        console.log(data)
        this.feedbackData = data
      },
      onPersonFetched (data) {
        this.personData = data
      },
      showUser: function (title, userId) {
        this.userView.title = title
        this.userView.userId = userId
        this.userView.visible = true
      }
    },
    filters: {
      attachedObjectTypeName (value) {
        switch (value) {
          case 'person':
            return '社交用户'

          default:
            return '示知类型'
        }
      },
      timeFormatter (value) {
        return moment.unix(value).format('YYYY-MM-DD HH:mm:ss')
      }
    },
    components: {
      ItemContainer, User
    }
  }
</script>
