<template>
  <div class="wrap" v-loading.body="loading">
    <el-card>
      <div v-if="title">
        <span style="line-height: 2rem;">{{title}}</span>
      </div>
      <div>
        <slot name="dataView"></slot>
      </div>
    </el-card>
  </div>
</template>
<style>

</style>
<script>
  export default {
    data () {
      return {
        resourceData: null,
        loading: false
      }
    },
    props: ['title', 'resourceUri', 'resourceId', 'resourceName'],
    methods: {
      fetchData () {
        let vm = this
        vm.loading = true

        this.$http.get(vm.resourceUri + '/' + vm.resourceId).then((response) => {
          response.json().then((response) => {
            vm.resourceData = response
            vm.$emit('fetched', vm.resourceData)
            vm.loading = false
          })
        })
      },
      patchData (patchData) {
        let vm = this
        vm.loading = true

        this.$http.patch(vm.resourceUri + '/' + vm.resourceId, patchData).then((response) => {
          response.json().then((response) => {
            vm.resourceData = response
            vm.$emit('patched', vm.resourceData)
            vm.loading = false
            vm.$message('操作成功')
          })
        })
      },
      postData (postData) {
        let vm = this
        vm.loading = true

        this.$http.post(vm.resourceUri, postData, {
          headers: {
            Authorization: vm.$store.state.login.authorization
          }
        }).then((response) => {
          response.json().then((response) => {
            vm.resourceData = response
            vm.$emit('posted', vm.resourceData)
            vm.loading = false
            vm.$message('操作成功')
          })
        })
      },
      deleteData () {
        let vm = this
        vm.loading = true

        this.$http.delete(vm.resourceUri + '/' + vm.resourceId).then((response) => {
          response.json().then((response) => {
            vm.resourceData = null
            vm.$emit('deleted')
            vm.loading = false
          })
        })
      }
    },
    created () {
      if (this.resourceId) {
        this.fetchData()
      } else {
        this.loading = false
      }
    },
    watch: {
      // 如果 resourceUri 发生改变，这个函数就会运行
      resourceId: function (newResourceId) {
        this.fetchData()
      }
    }
  }
</script>
