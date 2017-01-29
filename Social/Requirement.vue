<template>
  <div>
    <ItemContainer resourceUri="social/requirement"
                   v-bind:resourceId="requirementId"
                   resourceName="requirement"
                   v-on:fetched="onFetched">
      <div slot="dataView">
        <div v-if="requirementData">

          <el-form ref="requirementData" :model="requirementData" label-width="80px">

            <el-form-item label="性别">
              <el-radio-group v-model="requirementData.sex">
                <el-radio :label="1">男</el-radio>
                <el-radio :label="2">女</el-radio>
              </el-radio-group>
            </el-form-item>

            <el-form-item label="最小年龄">
              <el-input v-model="requirementData.age_min"></el-input>
            </el-form-item>

            <el-form-item label="最大年龄">
              <el-input v-model="requirementData.age_max"></el-input>
            </el-form-item>

            <el-form-item label="最小身高">
              <el-input v-model="requirementData.stature_min"></el-input>
            </el-form-item>

            <el-form-item label="最大身高">
              <el-input v-model="requirementData.stature_max"></el-input>
            </el-form-item>

            <el-form-item label="教育程度">
              <el-select v-model="requirementData.education" placeholder="请选择">
                <el-option
                  v-for="item in educations"
                  :label="item.label+' 以上'"
                  :value="item.value">
                </el-option>
              </el-select>
            </el-form-item>

            <el-form-item label="感情状态">
              <el-select v-model="requirementData.emotion" placeholder="请选择">
                <el-option
                  v-for="item in emotions"
                  :label="item.label"
                  :value="item.value">
                </el-option>
              </el-select>
            </el-form-item>

            <el-form-item label="星座">
              <el-select v-model="requirementData.zodiac" placeholder="请选择">
                <el-option
                  v-for="item in zodiacs"
                  :label="item.label"
                  :value="item.value">
                </el-option>
              </el-select>
            </el-form-item>

            <el-form-item label="生肖">
              <el-select v-model="requirementData.chinese_zodiac" placeholder="请选择">
                <el-option
                  v-for="item in chineseZodiacs"
                  :label="item.label"
                  :value="item.value">
                </el-option>
              </el-select>
            </el-form-item>

            <el-form-item label="职业">
              <el-select v-model="occupationId" placeholder="请选择">
                <el-option
                  v-for="item in occupationData"
                  :label="item.name"
                  :value="item.id">
                </el-option>
              </el-select>
            </el-form-item>

            <el-form-item label="收入等级">
              <el-select v-model="incomeLevelId" placeholder="请选择">
                <el-option
                  v-for="item in IncomeLevelData"
                  :label="getIncomeLevelName(item.min_income, item.max_income)"
                  :value="item.id">
                </el-option>
              </el-select>
            </el-form-item>

            <el-form-item label="居住地址">
              <div v-if="requirementData.residence_address">
                {{requirementData.residence_address.province.name}} (省/直辖市/自治区) -
                {{requirementData.residence_address.city.name}} (市) -
                {{requirementData.residence_address.district.name}} (县/县级市/区)<br>
                {{requirementData.residence_address.detail}}
              </div>
              <div v-else>没有填写</div>
            </el-form-item>

            <el-form-item label="户籍地址">
              <div v-if="requirementData.census_register_address">
                {{requirementData.census_register_address.province.name}} (省/直辖市/自治区) -
                {{requirementData.census_register_address.city.name}} (市) -
                {{requirementData.census_register_address.district.name}} (县/县级市/区)<br>
                {{requirementData.census_register_address.detail}}
              </div>
              <div v-else>没有填写</div>
            </el-form-item>

          </el-form>

        </div>
      </div>
    </ItemContainer>

    <div style="display: none">
      <ListContainer title="职业列表"
                     resource-uri="user/occupation"
                     resource-name="occupation"
                     v-on:fetched="onOccupationFetched"></ListContainer>
      <ListContainer title="收入等级列表"
                     resource-uri="user/income-level"
                     resource-name="income_level"
                     v-on:fetched="onIncomeLevelFetched"></ListContainer>
    </div>
  </div>
</template>

<style>

</style>

<script>
  import ItemContainer from '../ItemContainer.vue'
  import ListContainer from '../ListContainer.vue'

  export default{
    data () {
      return {
        requirementData: null,
        occupationData: null,
        IncomeLevelData: null,
        educations: [
          {value: 1, label: '没有受过教育'},
          {value: 2, label: '小学'},
          {value: 3, label: '初中'},
          {value: 4, label: '高中'},
          {value: 5, label: '大专'},
          {value: 6, label: '本科'},
          {value: 7, label: '硕士'},
          {value: 8, label: '博士'}
        ],
        emotions: [
          {value: 1, label: '单身'},
          {value: 2, label: '热恋'},
          {value: 3, label: '已婚'}
        ],
        zodiacs: [
          {value: 1, label: '魔羯座'},
          {value: 2, label: '水瓶座'},
          {value: 3, label: '双鱼座'},
          {value: 4, label: '牡羊座'},
          {value: 5, label: '金牛座'},
          {value: 6, label: '双子座'},
          {value: 7, label: '巨蟹座'},
          {value: 8, label: '狮子座'},
          {value: 9, label: '处女座'},
          {value: 10, label: '天秤座'},
          {value: 11, label: '天蝎座'},
          {value: 12, label: '射手座'}
        ],
        chineseZodiacs: [
          {value: 1, label: '鼠'},
          {value: 2, label: '牛'},
          {value: 3, label: '虎'},
          {value: 4, label: '兔'},
          {value: 5, label: '龙'},
          {value: 6, label: '蛇'},
          {value: 7, label: '马'},
          {value: 8, label: '羊'},
          {value: 9, label: '猴'},
          {value: 10, label: '鸡'},
          {value: 11, label: '狗'},
          {value: 12, label: '猪'}
        ]
      }
    },
    props: ['requirementId'],
    computed: {
      occupationId () {
        if (this.requirementData.occupation) return this.requirementData.occupation.id
        else return null
      },
      incomeLevelId () {
        if (this.requirementData.income_level) return this.requirementData.income_level.id
        else return null
      }
    },
    methods: {
      onFetched: function (data) {
        console.log(data)
        this.requirementData = data
      },
      onOccupationFetched: function (data) {
        console.log(data)
        this.occupationData = data
      },
      onIncomeLevelFetched: function (data) {
        console.log(data)
        this.IncomeLevelData = data
      },
      getIncomeLevelName (min, max) {
        let string = null
        if (!min) {
          string = parseInt(max) + '以下'
        } else if (!max) {
          string = parseInt(min) + '以上'
        } else {
          string = parseInt(min) + '以上'
        }
        return string
      }
    },
    components: {
      ItemContainer, ListContainer
    }
  }
</script>
