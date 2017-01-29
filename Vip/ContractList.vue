<template>
  <div>
    <ListContainer title="会员合约"
                   resource-uri="vip/contract"
                   resource-name="contract"
                   :params="filterParams"
                   v-on:fetched="onFetched"
                   ref="ContractList">
      <div slot="filters">
        <div class="select">
          <el-select v-model="filters.order_status" @change="onFilterChange" placeholder="状态筛选">
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
          style="width: 100%; font-size: 60%;">
          <el-table-column
            prop="id"
            label="ID" width="80">
          </el-table-column>
          <el-table-column
            prop="title"
            label="合约标题">
          </el-table-column>
          <el-table-column
            prop="create_time"
            label="创建时间"
            :formatter="timeFormatter">
          </el-table-column>
          <el-table-column
            prop="effective_time_time"
            label="生效时间"
            :formatter="timeFormatter">
          </el-table-column>
          <el-table-column
            label="价格">
            <template scope="scope">
              <div style="padding: 1rem 0;">
                金额：{{scope.row.order.total}} 元<br>
                状态：
                <el-tag type="warning" v-if="scope.row.order.status === 1">未支付</el-tag>
                <el-tag type="success" v-if="scope.row.order.status === 3">已支付</el-tag><br>
                <el-tooltip class="item" effect="dark" :content="'交易流水号：' + scope.row.order.payment_series_number" placement="top" v-if="scope.row.order.payment_type">
                  方式：
                  <el-tag type="primary" v-if="scope.row.order.payment_type === 'alipay'">支付宝</el-tag>
                  <el-tag type="primary" v-if="scope.row.order.payment_type === 'wxpay'">微信支付</el-tag>
                </el-tooltip>
              </div>
            </template>
          </el-table-column>
          <el-table-column
            label="用户">
            <template scope="scope">
              <el-button
                type="text"
                @click="showUser(scope.row.user.nickname, scope.row.user.id)">
                {{scope.row.user.nickname}}
              </el-button>
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
  import User from '../User/User.vue'

  export default {
    data () {
      return {
        items: [],
        filters: {
          order_status: null
        },
        filterParams: {},
        selectOpts: {
          statusOpts: [{label: '全部状态', value: 0}, {label: '未支付', value: 1}, {label: '已支付', value: 3}]
        },
        userView: {
          visible: false,
          title: null,
          userId: null
        }
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
        console.log(this.$refs.ContractList.fetchData)
        this.$refs.ContractList.fetchData()
      }
    },
    components: {
      ListContainer, User
    }
  }
</script>
