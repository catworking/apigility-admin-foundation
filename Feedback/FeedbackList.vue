<template>
  <div>
    <ListContainer title="用户反馈"
                   resource-uri="feedback/feedback"
                   resource-name="feedback"
                   :params="filterParams"
                   v-on:fetched="onFetched"
                   ref="FeedbackList">
      <div slot="filters">
        <div class="select">
          <el-select v-model="filters.type" @change="onFilterChange" placeholder="类型筛选">
            <el-option
              v-for="item in selectOpts.typeOpts"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>
          <el-select v-model="filters.status" @change="onFilterChange" placeholder="状态筛选">
            <el-option
              v-for="item in selectOpts.statusOpts"
              :label="item.label"
              :value="item.value">
            </el-option>
          </el-select>
        </div>
      </div>
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
            label="反馈内容">
            <template scope="scope">
              {{scope.row.article.content}}
              <el-button
                type="text"
                @click="showFeedback(scope.row.user.nickname + '的反馈', scope.row.id)">
                查看详情
              </el-button>
            </template>
          </el-table-column>
          <el-table-column
            prop="create_time"
            label="时间"
            :formatter="timeFormatter">
          </el-table-column>
          <el-table-column
            label="反馈人">
            <template scope="scope">
              <el-button
                type="text"
                @click="showUser(scope.row.user.nickname, scope.row.user.id)">
                {{scope.row.user.nickname}}
              </el-button>
            </template>
          </el-table-column>
          <el-table-column
            prop="create_time"
            label="类型">
            <template scope="scope">
              <el-tag type="primary" v-if="scope.row.type === 'complaint'">投诉</el-tag>
              <el-tag type="success" v-if="scope.row.type === 'accusation'">举报</el-tag>
            </template>
          </el-table-column>
          <el-table-column
            prop="status"
            label="状态">
            <template scope="scope">
              <el-select v-model="scope.row.status" @change="changeRowStatus(scope.row.id, scope.row.status)" placeholder="未知状态" size="mini">
                <el-option
                  v-for="item in selectOpts.statusOptsClean"
                  :label="item.label"
                  :value="item.value">
                </el-option>
              </el-select>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </ListContainer>

    <el-dialog v-bind:title="userView.title" v-model="userView.visible" size="small">
      <div style="width: 100%; overflow: hidden;" v-if="userView.visible">
        <User v-bind:user-id="userView.userId"></User>
      </div>
    </el-dialog>

    <el-dialog v-bind:title="feedbackView.title" v-model="feedbackView.visible" size="small">
      <div style="width: 100%; overflow: hidden;" v-if="feedbackView.visible">
        <Feedback v-bind:feedback-id="feedbackView.feedbackId"></Feedback>
      </div>
    </el-dialog>

    <div style="display: none">
      <ItemContainer v-if="patchingFeedbackId"
                     title="更新反馈状态"
                     ref="Feedback"
                     v-bind:resourceUri="patchingFeedbackResourceUrl"
                     resourceName="feedback" @fetched="onFetchedPatchingFeedback">
      </ItemContainer>
    </div>

  </div>
</template>

<style scoped>
  .main-title{
    line-height: 36px;
  }
  div.select{
    margin-bottom: 20px;
  }
  #page{
    text-align: center;
    margin-top: 24px;
  }
</style>

<script>
  import moment from 'moment'
  import ListContainer from '../ListContainer.vue'
  import ItemContainer from '../ItemContainer.vue'
  import User from '../User/User.vue'
  import Feedback from './Feedback.vue'

  export default {
    data () {
      return {
        items: [],
        filters: {
          type: null,
          status: null
        },
        filterParams: {},
        selectOpts: {
          typeOpts: [{label: '全部类型', value: ''}, {label: '投诉', value: 'complaint'}, {label: '举报', value: 'accusation'}],
          statusOpts: [{label: '全部状态', value: 0}, {label: '待处理', value: 1}, {label: '已处理', value: 2}],
          statusOptsClean: [{label: '待处理', value: 1}, {label: '已处理', value: 2}]
        },
        userView: {
          visible: false,
          title: null,
          userId: null
        },
        feedbackView: {
          visible: false,
          title: null,
          feedbackId: null
        },
        patchingFeedbackId: null,
        patchingFeedbackValue: null
      }
    },
    computed: {
      patchingFeedbackResourceUrl () {
        return 'feedback/feedback/' + this.patchingFeedbackId
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
      onFilterChange () {
        for (let i in this.filters) {
          if (this.filters[i]) {
            this.filterParams[i] = this.filters[i]
          } else {
            delete this.filterParams[i]
          }
        }
        console.log(this.$refs.FeedbackList.fetchData)
        this.$refs.FeedbackList.fetchData()
      },
      changeRowStatus (feedbackId, status) {
        console.log(feedbackId, status)
        this.patchingFeedbackId = feedbackId
        this.patchingFeedbackValue = status
      },
      onFetchedPatchingFeedback () {
        console.log(this.$refs.Feedback)
        this.$refs.Feedback.patchData({
          status: this.patchingFeedbackValue
        })
        this.patchingFeedbackId = null
        this.patchingFeedbackValue = null
      },
      showFeedback: function (title, feedbackId) {
        this.feedbackView.title = title
        this.feedbackView.feedbackId = feedbackId
        this.feedbackView.visible = true
      }
    },
    components: {
      ListContainer, ItemContainer, User, Feedback
    }
  }
</script>
