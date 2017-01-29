<template>
  <div>
    <ListContainer title="相册列表"
                   v-bind:resource-uri="resourceUrl"
                   resource-name="photo"
                   v-on:fetched="onFetched">
      <div slot="items">
        <el-row :gutter="10" v-if="items.length">
          <el-col :span="6" v-for="row in items">
            <el-card class="photo-card">
              <div class="img" :style="{backgroundImage: 'url(' + row.uri + ')'}"></div>
              <span class="time"><i class="el-icon-time"></i> {{row.create_time | timeFormatterFilter}}</span>
            </el-card>
          </el-col>
        </el-row>
        <el-row :gutter="20" v-else>
          <el-col :span="24">
            <div class="empty">该用户相册下暂无照片</div>
          </el-col>
        </el-row>
      </div>
    </ListContainer>
  </div>
</template>

<style scoped>
  div.photo-card{
    margin-bottom: 20px;
  }
  div.img{
    background-size: cover;
    padding-bottom: 80%;
  }
  span.time{
    display: block;
    margin-top: 6px;
    font-size: 14px;
    text-align: center;
  }
  div.empty,div.waiting{
    font-size: 14px;
    text-align: center;
  }
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
        return 'photo-album/photo?album_id=' + this.$route.params.album_id
      }
    },
    methods: {
      onFetched: function (data) {
        this.items = data
      }
    },
    filters: {
      timeFormatterFilter: (val) => {
        return moment.unix(val).format('YYYY-MM-DD HH:mm:ss')
      }
    },
    components: {
      ListContainer
    }
  }
</script>
