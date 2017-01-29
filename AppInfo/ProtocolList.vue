<template>
  <div>
    <ListContainer title="协议"
                   resource-uri="appinfo/protocol"
                   resource-name="protocol"
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
            prop="title"
            label="标题">
          </el-table-column>

          <el-table-column
            label="操作">
            <template scope="scope">
              <el-button type="primary"
                         size="small"
                         icon="edit"
                         @click="showProtocol(scope.row.title, scope.row.id)">
                编辑
              </el-button>
            </template>
          </el-table-column>
        </el-table>
      </div>
    </ListContainer>

    <el-dialog v-bind:title="protocolView.title" v-model="protocolView.visible" size="full">
      <div style="width: 100%; overflow: hidden;" v-if="protocolView.visible">
        <Protocol v-bind:protocol-id="protocolView.protocolId"></Protocol>
      </div>
    </el-dialog>
  </div>
</template>
<style>

</style>
<script>
  import ListContainer from '../ListContainer.vue'
  import Protocol from './Protocol.vue'

  export default {
    data () {
      return {
        items: [],
        protocolView: {
          visible: false,
          title: null,
          protocolId: null
        }
      }
    },
    methods: {
      onFetched: function (data) {
        this.items = data
      },
      showProtocol: function (title, protocolId) {
        this.protocolView.title = title
        this.protocolView.protocolId = protocolId
        this.protocolView.visible = true
      }
    },
    components: {
      ListContainer, Protocol
    }
  }
</script>
