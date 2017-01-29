<template>
  <div>
    <ListContainer title="广告位置"
                   resource-uri="ad/position"
                   resource-name="position"
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
            prop="name"
            label="名称">
          </el-table-column>
        </el-table>
      </div>
    </ListContainer>
  </div>
</template>
<style>

</style>
<script>
  import ListContainer from '../ListContainer.vue'

  export default {
    data () {
      return {
        items: []
      }
    },
    methods: {
      onFetched: function (data) {
        this.items = data
      }
    },
    components: {
      ListContainer
    }
  }
</script>
