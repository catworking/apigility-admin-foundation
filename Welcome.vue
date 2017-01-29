<template>
  <div class="hello">
    <h4>欢迎使用管理后台！</h4>
    <el-card class="box-card">
      <div slot="header" class="clearfix">
        <span style="line-height: 36px;">数据统计</span>
      </div>
      <div>
        <ul>
          <li v-for="item in totalData">{{item.name}}：{{item.value}} {{item.sub_fix}}</li>
        </ul>
      </div>
    </el-card>

    <div style="display: none;">
      <ListContainer ref="PersonList"
                     resource-uri="social/person"
                     resource-name="person"
                     v-on:fetched="onPersonListFetched">
      </ListContainer>
      <ListContainer ref="VipPersonList"
                     resource-uri="social/person"
                     :params="{vip: 'yes'}"
                     resource-name="person"
                     v-on:fetched="onVipPersonListFetched">
      </ListContainer>
      <ListContainer ref="LedgerList"
                     resource-uri="finance/ledger"
                     :params="{user_id: '', account: 'vip_contract_income'}"
                     resource-name="ledger"
                     v-on:fetched="onLedgerListFetched">
      </ListContainer>
    </div>
  </div>
</template>

<script>
  import ListContainer from './ListContainer.vue'

  export default {
    data () {
      return {
        totalData: {
          userTotal: {name: '社交用户总数', value: '正在计算...', sub_fix: '人'},
          vipUserTotal: {name: '社交会员总数', value: '正在计算...', sub_fix: '人'},
          incomeTotal: {name: '会员费收入总额', value: '正在计算...', sub_fix: '元'}
        }
      }
    },
    methods: {
      onPersonListFetched (data) {
        this.totalData.userTotal.value = this.$refs.PersonList.total_items
      },
      onVipPersonListFetched () {
        this.totalData.vipUserTotal.value = this.$refs.VipPersonList.total_items
      },
      onLedgerListFetched (data) {
        console.log(data)
        if (data.length === 0) this.totalData.incomeTotal.value = 0.00
        else this.totalData.incomeTotal.value = data[0].balance
      }
    },
    components: {
      ListContainer
    }
  }
</script>

<style scoped>
  div.hello {
    min-height: 100%;
  }
</style>
