<template>
  <div>
    <ListContainer title="社交个体"
                   resource-uri="social/person"
                   resource-name="person"
                   v-on:fetched="onFetched">
      <div slot="items">
        <el-table :data="items" stripe style="width: 100%; font-size: 60%;">
          <el-table-column
            prop="id"
            label="序号" width="80">
          </el-table-column>
          <el-table-column
            prop="user.nickname"
            label="姓名" width="150">
          </el-table-column>
          <el-table-column
            label="头像" width="100">
            <template scope="scope">
              <div style="width: 4rem; height: 4rem; line-height: 4rem; text-align: center; overflow: hidden; margin: .5rem auto;">
                <img v-if="scope.row.user.avatar" style="width: 100%" v-bind:src="scope.row.user.avatar">
                <span v-else>
                  无
                </span>
              </div>

            </template>
          </el-table-column>
          <el-table-column
            label="性别" width="80">
            <template scope="scope">
              <span v-if="scope.row.user.sex">{{sexOption[parseInt(scope.row.user.sex)]}}</span>
              <span v-else>未填写</span>
            </template>
          </el-table-column>
          <el-table-column
            label="会员" width="250">
            <template scope="scope">
              <div>
                身份：
                <span v-if="scope.row.vip_status">{{scope.row.vip_status.name}}</span>
                <span v-else>无</span>
                <br>
                状态：
                <span v-if="scope.row.vip">有效</span>
                <span v-else>无效</span>
                <br>
                到期：
                <span v-if="scope.row.vip_expire_time">{{scope.row.vip_expire_time | timeFormatterFilter}}</span>
                <span v-else>不可用</span>
              </div>
            </template>
          </el-table-column>
          <el-table-column
            label="操作">
            <template scope="scope">
              <div style="margin: .5rem 0;">
                <div>
                  <el-button
                    size="small"
                    type="primary"
                    @click="showUser(scope.row.user.nickname + '用户资料', scope.row.user.id)">查看用户资料</el-button>
                  <el-button
                    size="small"
                    type="primary"
                    @click="showPhoto(scope.row.user.nickname + '封面照',scope.row.cover_image)">封面照</el-button>
                </div>
                <div style="margin-top: .5rem;">
                  <el-button
                    size="small"
                    type="primary"
                    @click="showRequirement(scope.row.user.nickname + '交友要求', scope.row.requirement.id)">查看交友要求</el-button>
                  <el-button
                    size="small"
                    type="primary"
                    @click="showPhoto(scope.row.user.nickname + '形象照', scope.row.appearance_image)">形象照</el-button>
                </div>
                <div style="margin-top: .5rem;">
                  <el-button
                    size="small"
                    type="primary"
                    @click="redirectToUserAlbumList(scope.row.user.id)">相册</el-button>
                </div>
              </div>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </ListContainer>

    <el-dialog v-bind:title="photoView.title" v-model="photoView.visible" size="small">
      <div style="width: 100%; overflow: hidden;">
        <img v-if="photoView.photoUrl" style="width: 100%" v-bind:src="photoView.photoUrl">
        <span v-else>没有上传</span>
      </div>
    </el-dialog>

    <el-dialog v-bind:title="userView.title" v-model="userView.visible" size="small">
      <div style="width: 100%; overflow: hidden;" v-if="userView.visible">
        <User v-bind:user-id="userView.userId"></User>
      </div>
    </el-dialog>

    <el-dialog v-bind:title="requirementView.title" v-model="requirementView.visible" size="small">
      <div style="width: 100%; overflow: hidden;" v-if="requirementView.visible">
        <Requirement v-bind:requirement-id="requirementView.requirementId"></Requirement>
      </div>
    </el-dialog>
  </div>
</template>

<style scoped>

</style>

<script>
  import ListContainer from '../ListContainer.vue'
  import User from '../User/User.vue'
  import Requirement from './Requirement.vue'
  import moment from 'moment'

  export default {
    data () {
      return {
        items: [],
        sexOption: ['未填写', '男', '女'],
        photoView: {
          visible: false,
          title: null,
          photoUrl: null
        },
        userView: {
          visible: false,
          title: null,
          userId: null
        },
        requirementView: {
          visible: false,
          title: null,
          requirementId: null
        },
        doNotPatchUser: true
      }
    },
    components: {
      ListContainer, User, Requirement
    },
    methods: {
      onFetched: function (data) {
        this.items = data
      },
      redirectToUser: function (userId) {
        this.$router.push('/workbench/user/user/' + userId)
      },
      redirectToUserAlbumList: function (userId) {
        this.$router.push('/workbench/photo-album/album-list/' + userId)
      },
      showPhoto: function (title, url) {
        this.photoView.title = title
        this.photoView.photoUrl = url
        this.photoView.visible = true
      },
      showUser: function (title, userId) {
        this.userView.title = title
        this.userView.userId = userId
        this.userView.visible = true
      },
      showRequirement: function (title, requirementId) {
        this.requirementView.title = title
        this.requirementView.requirementId = requirementId
        this.requirementView.visible = true
      }
    },
    filters: {
      timeFormatterFilter: (val) => {
        return moment.unix(val).format('YYYY-MM-DD HH:mm:ss')
      }
    }
  }
</script>
