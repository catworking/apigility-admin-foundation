<template>
  <div class="wrap">

    <ItemContainer resourceUri="user/user"
                   v-bind:resourceId="userId"
                   resourceName="user"
                   v-on:fetched="onFetched">
      <div slot="dataView">

        <div v-if="userData">

          <div v-if="userData.avatar" class="avatar"><img :src="userData.avatar"></div>

          <el-form ref="userData" :model="userData" label-width="80px">

            <el-form-item label="标识">
              <el-input v-model="userData.id"></el-input>
            </el-form-item>

            <el-form-item label="昵称">
              <el-input v-model="userData.nickname"></el-input>
            </el-form-item>

            <el-form-item label="性别">
              <el-radio-group v-model="userData.sex">
                <el-radio :label="1">男</el-radio>
                <el-radio :label="2">女</el-radio>
              </el-radio-group>
            </el-form-item>

            <el-form-item label="年龄">
              <el-input v-model="userData.age"></el-input>
            </el-form-item>

            <el-form-item label="生日">
              <el-date-picker
                v-model="userData.birthday"
                type="date"
                placeholder="选择日期"
                :picker-options="pickerOptions0">
              </el-date-picker>
            </el-form-item>

            <el-form-item label="身高">
              <el-input v-model="userData.stature"></el-input>
            </el-form-item>

            <el-form-item label="体重">
              <el-input v-model="userData.weight"></el-input>
            </el-form-item>

            <el-form-item label="教育程度">
              <el-select v-model="userData.education" placeholder="请选择">
                <el-option
                  v-for="item in educations"
                  :label="item.label"
                  :value="item.value">
                </el-option>
              </el-select>
            </el-form-item>

            <el-form-item label="感情状态">
              <el-select v-model="userData.emotion" placeholder="请选择">
                <el-option
                  v-for="item in emotions"
                  :label="item.label"
                  :value="item.value">
                </el-option>
              </el-select>
            </el-form-item>

            <el-form-item label="星座">
              <el-select v-model="userData.zodiac" placeholder="请选择">
                <el-option
                  v-for="item in zodiacs"
                  :label="item.label"
                  :value="item.value">
                </el-option>
              </el-select>
            </el-form-item>

            <el-form-item label="生肖">
              <el-select v-model="userData.chinese_zodiac" placeholder="请选择">
                <el-option
                  v-for="item in chineseZodiacs"
                  :label="item.label"
                  :value="item.value">
                </el-option>
              </el-select>
            </el-form-item>

            <el-form-item label="职业">
              <el-select v-model="userData.occupation" placeholder="请选择">
                <el-option
                  v-for="item in occupationData"
                  :label="item.name"
                  :value="item.id">
                </el-option>
              </el-select>
            </el-form-item>

            <el-form-item label="收入等级">
              <el-select v-model="userData.income_level" placeholder="请选择">
                <el-option
                  v-for="item in IncomeLevelData"
                  :label="getIncomeLevelName(item.min_income, item.max_income)"
                  :value="item.id">
                </el-option>
              </el-select>
            </el-form-item>

            <el-form-item label="居住地址">
              <div v-if="userData.residence_address">
                {{userData.residence_address.province.name}} (省/直辖市/自治区) -
                {{userData.residence_address.city.name}} (市) -
                {{userData.residence_address.district.name}} (县/县级市/区)<br>
                {{userData.residence_address.detail}}
              </div>
              <div v-else>没有填写</div>
            </el-form-item>

            <el-form-item label="户籍地址">
              <div v-if="userData.census_register_address">
                {{userData.census_register_address.province.name}} (省/直辖市/自治区) -
                {{userData.census_register_address.city.name}} (市) -
                {{userData.census_register_address.district.name}} (县/县级市/区)<br>
                {{userData.census_register_address.detail}}
              </div>
              <div v-else>没有填写</div>
            </el-form-item>

            <el-form-item label="实名认证">
              <el-card :body-style="{ padding: '0px'}" v-if="userData.personal_certification">
                <div v-if="userData.personal_certification.identity_card_image_front" class="image_box">
                  <el-tooltip class="item" effect="dark" content="身份证正面照" placement="left">
                    <img :src="userData.personal_certification.identity_card_image_front">
                  </el-tooltip>
                </div>
                <div v-if="userData.personal_certification.identity_card_image_back" class="image_box">
                  <el-tooltip class="item" effect="dark" content="身份证反面照" placement="left">
                    <img :src="userData.personal_certification.identity_card_image_back">
                  </el-tooltip>
                </div>
                <div v-if="userData.personal_certification.holding_identity_card_image" class="image_box">
                  <el-tooltip class="item" effect="dark" content="手持身份证照" placement="left">
                    <img :src="userData.personal_certification.holding_identity_card_image">
                  </el-tooltip>
                </div>
                <div style="padding: 14px;">
                  <span v-if="userData.personal_certification.identity_card_number">身份证号码：{{userData.personal_certification.identity_card_number}}</span>
                  <div class="bottom clearfix">
                    <el-form-item label="审核状态">
                      <el-radio-group v-model="userData.personal_certification.status" v-on:change="modifyPersonalCertificationStatus">
                        <el-radio :label="1">未审核</el-radio>
                        <el-radio :label="2">拒绝</el-radio>
                        <el-radio :label="3">通过</el-radio>
                      </el-radio-group>
                    </el-form-item>
                  </div>
                </div>
              </el-card>
            </el-form-item>

            <el-form-item>
              <el-button type="primary">保存</el-button>
            </el-form-item>

          </el-form>

        </div>

        <div v-else>没有数据</div>

        <div style="display: none">
          <ListContainer title="职业列表"
                         resource-uri="user/occupation"
                         resource-name="occupation"
                         v-on:fetched="onOccupationFetched"></ListContainer>
          <ListContainer title="收入等级列表"
                         resource-uri="user/income-level"
                         resource-name="income_level"
                         v-on:fetched="onIncomeLevelFetched"></ListContainer>
          <ItemContainer title="实名认证"
                         ref="PersonalCertification"
                         resourceUri="user/personal-certification"
                         v-bind:resourceId="personalCertificationResourceId"
                         resourceName="personal_certification"
                         v-on:fetched="onFetched">
          </ItemContainer>
        </div>
      </div>
    </ItemContainer>

  </div>
</template>

<style scoped>
  .avatar {
    width: 5rem; height: 5rem;
    overflow: hidden;
    border: .5rem #EEEEEE solid;
    margin: 1rem auto;
    border-radius: .4rem;
  }
  .avatar img {
    width: 100%;
  }
  .image_box {

  }
  .image_box img {
    max-width: 100%;
    display: block;
  }
</style>

<script>
  import ItemContainer from '../ItemContainer.vue'
  import ListContainer from '../ListContainer.vue'
  export default {
    data () {
      return {
        userData: null,
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
    props: ['userId'],
    computed: {
      personalCertificationResourceId: function () {
        if (this.userData && this.userData.personal_certification) {
          return this.userData.personal_certification.id
        } else {
          return null
        }
      }
    },
    methods: {
      onFetched: function (data) {
        console.log(data)
        this.userData = data
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
          string = parseInt(min) + ' - ' + parseInt(max)
        }
        return string
      },
      modifyPersonalCertificationStatus (value) {
        console.log(this.userData.personal_certification.status)
        console.log(value)
        this.$refs.PersonalCertification.patchData({
          status: value
        })
      }
    },
    components: {
      ItemContainer,
      ListContainer
    }
  }
</script>
