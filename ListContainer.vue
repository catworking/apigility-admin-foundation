<template>
  <div>
    <el-card>
      <div slot="header" class="title">
        <span>{{title}}</span>
        <el-button style="float: right;" type="default" icon="arrow-left"size="small" @click="back()">返回</el-button>
      </div>
      <div>
        <slot name="filters"></slot>
      </div>
      <div v-loading.body="loading">
        <slot name="items"></slot>
      </div>
      <div class="pagination">
        <el-pagination
          @current-change="handleCurrentPageChange"
          :current-page="page"
          :page-size="page_size"
          layout="total, prev, pager, next, jumper"
          :total="total_items">
        </el-pagination>
      </div>
    </el-card>
  </div>
</template>

<style scoped>
  .title {
    font-weight: bold;
    margin-bottom: 1rem;
  }
  .pagination {
    margin-top: 1rem;
  }
</style>

<script>
  export default {
    data () {
      return {
        items: [],
        page_size: 25,
        total_items: 0,
        page: 1,
        loading: true
      }
    },
    props: ['title', 'resourceUri', 'resourceName', 'params'],
    methods: {
      fetchData () {
        let vm = this
        let params = {
          page: this.page,
          ...vm.params
        }

        this.$http.get(vm.resourceUri, {
          params: params
        }).then((response) => {
          response.json().then((response) => {
            eval('vm.items = response._embedded.' + vm.resourceName)
            this.page_size = response.page_size
            this.total_items = response.total_items
            this.page = response.page < 1 ? 1 : response.page
            vm.$emit('fetched', vm.items)
            vm.loading = false
          })
        })
      },
      handleCurrentPageChange (val) {
        this.page = parseInt(val)
        this.fetchData()
      },
      back () {
        this.$router.go(-1)
      }
    },
    created () {
      this.fetchData()
    }
  }
</script>
