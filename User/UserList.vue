<template>
  <div>
    <ListContainer title="用户列表"
                   resource-uri="user/user"
                   resource-name="user"
                   v-on:fetched="onFetched">
      <div slot="items">
        <el-table :data="items" stripe style="width: 100%; font-size: 60%;">
          <el-table-column
            prop="id"
            label="序号" width="80">
          </el-table-column>
          <el-table-column
            prop="nickname"
            label="姓名" width="150">
          </el-table-column>
          <el-table-column
            label="头像" width="100">
            <template scope="scope">
              <div style="width: 4rem; height: 4rem; line-height: 4rem; text-align: center; overflow: hidden; margin: .5rem auto;">
                <img v-if="scope.row.avatar" style="width: 100%" v-bind:src="scope.row.avatar">
                <span v-else>
                  无
                </span>
              </div>

            </template>
          </el-table-column>
          <el-table-column
            label="性别" width="80">
            <template scope="scope">
              <span v-if="scope.row.sex">{{sexOption[parseInt(scope.row.sex)]}}</span>
              <span v-else>未填写</span>
            </template>
          </el-table-column>
          <el-table-column
            label="实名认证" width="250">
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
                <span v-if="scope.row.vip_expire_time">{{scope.row.vip_expire_time}}</span>
                <span v-else>不可用</span>
              </div>
            </template>
          </el-table-column>
          <el-table-column
            label="操作">
            <template scope="scope">
              <div>
                <el-button
                  size="small"
                  type="primary"
                  @click="showUser('用户资料', scope.row.user.id)">查看用户资料</el-button>
              </div>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </ListContainer>

    <el-dialog v-bind:title="userView.title" v-model="userView.visible" size="small">
      <div style="width: 100%; overflow: hidden;">
        <User v-bind:user-id="userView.userId"></User>
      </div>
    </el-dialog>
  </div>
</template>

<style scoped>

</style>

<script>
  import ListContainer from '../ListContainer.vue'
  import User from '../User/User.vue'

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
        }
      }
    },
    components: {
      ListContainer, User
    },
    methods: {
      onFetched: function (data) {
        this.items = data
      },
      redirectToUser: function (userId) {
        this.$router.push('/workbench/user/user/' + userId)
      },
      showPhoto: function (title, url) {
        this.photoView.title = title
        this.photoView.photoUrl = url
        this.photoView.visible = true
      },
      showUser: function (title, userId) {
        console.log(userId)
        this.userView.title = title
        this.userView.userId = userId
        this.userView.visible = true
      }
    }
  }
</script>
