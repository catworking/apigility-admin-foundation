<template>
  <div>
    <ListContainer title="相册列表"
                   v-bind:resource-uri="resourceUrl"
                   resource-name="album"
                   v-on:fetched="onFetched">
      <div slot="items">
        <el-table
          :data="items"
          stripe
          style="width: 100%">
          <el-table-column
            prop="id"
            label="ID">
          </el-table-column>
          <el-table-column
            prop="name"
            label="相册名称">
          </el-table-column>
          <el-table-column
            prop="create_time"
            label="创建时间"
            :formatter="timeFormatter">
          </el-table-column>
          <el-table-column
            label="操作">
            <template scope="scope">
              <el-button
                size="small"
                type="primary"
                @click="handleViewDetail(scope.row.id)">查看照片</el-button>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </ListContainer>
  </div>
</template>

<style scoped>
</style>

<script>
  import moment from 'moment'
  import ListContainer from '../ListContainer.vue'

  export default {
    data () {
      return {
        items: []
      }
    },
    computed: {
      resourceUrl: function () {
        return 'photo-album/album?user_id=' + this.$route.params.user_id
      }
    },
    methods: {
      onFetched: function (data) {
        this.items = data
      },
      timeFormatter (row, column) {
        return moment.unix(row.create_time).format('YYYY-MM-DD HH:mm:ss')
      },
      handleViewDetail (albumId) {
        this.$router.push('/workbench/photo-album/photo-list/' + albumId)
      }
    },
    components: {
      ListContainer
    }
  }
</script>
