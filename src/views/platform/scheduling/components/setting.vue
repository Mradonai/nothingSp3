<template>
    <div>
        <div style="margin-top: 40px">
            <table class="setting-info-table"  cellspacing="0" cellpadding="0" >
                <tr class="header">
                    <td width="10%">工种</td>
                    <td width="15%">结算方式</td>
                    <td width="5%">性别</td>
                    <td width="10%">上班时间</td>
                    <td width="10%">下班时间</td>
                    <td width="50%" >报酬设置</td>
                </tr>
                <tbody v-for="(item,index) in form"> 
                  <tr>
                    <td rowspan="4">{{item.industryName}}</td>
                    <td rowspan="2">{{salary[index][0].settlementType | formatSettle}}

                      <span class="setting-tag" v-if="salary[index][0].valuationType == 1">计时</span>
                      <span class="setting-tag" v-if="salary[index][0].valuationType == 2 || salary[index][0].valuationType == 3 || salary[index][0].valuationType == 4">计量</span>
                      <span class="setting-tag" v-if="salary[index][0].valuationType == 5 || salary[index][0].valuationType == 6 || salary[index][0].valuationType == 7">计时+计量</span>

                    </td>
                    <td>{{salary[index][0].price[0].sex == 1 ? '男' : '女'}}</td>
                    <td rowspan="4">{{item.workStartTime | formatTimeString}}</td>
                    <td rowspan="4" v-if="item.endWorkType == 2">{{item.endWorkType == 2 ? '次日' : ''}}{{item.workEndTime | formatTimeString}}</td>
                    <td rowspan="4" v-else-if="item.endWorkType == 1">{{item.endWorkType == 1 ? '当日' : ''}}{{item.workEndTime | formatTimeString}}</td>
                    <td rowspan="4" v-else>{{item.endWorkType == 1 ? '当日' : ''}}{{item.workEndTime | formatTimeString}}</td>
                    <td >
                      <!-- 计时 1-->
                  <br/>
                  <span v-if="salary[index][0].valuationType ==1 || salary[index][0].valuationType ==6 || salary[index][0].valuationType == 5 || salary[index][0].valuationType == 7">{{salary[index][0].price[0].countTime}}元/小时<br/></span>
                  <!-- 阶梯 -->
                  <span v-if="(salary[index][0].valuationType ==6 || salary[index][0].valuationType ==3) && i == 0" v-for="(data,i) in salary[index][0].price[0].ladderObj">{{data.count}}{{salary[index][0].unit | formatType}}以下(含)， {{data.unit}}元/{{salary[index][0].unit | formatType}}<br/></span>
                  
                  <span v-if="(salary[index][0].valuationType ==6 || salary[index][0].valuationType ==3) && i != 0 && i != salary[index][0].price[0].ladderObj.length - 1" v-for="(data,i) in salary[index][0].price[0].ladderObj">{{Number(salary[index][0].price[0].ladderObj[i-1].count) + 1}} - {{Number(data.count)}}{{salary[index][0].unit | formatType}}(含)， {{data.unit}}元/{{salary[index][0].unit | formatType}}<br/></span>

                  <span v-if="(salary[index][0].valuationType ==6 || salary[index][0].valuationType ==3) && i == salary[index][0].price[0].ladderObj.length - 1" v-for="(data,i) in salary[index][0].price[0].ladderObj">{{salary[index][0].price[0].ladderObj[i-1].count}}{{salary[index][0].unit | formatType}}以上， {{data.overUnit}}元/{{salary[index][0].unit | formatType}}<br/><br/></span>

                  <!-- 计量 -->
                  <span v-if="(salary[index][0].valuationType == 2 || salary[index][0].valuationType == 5) && salary[index][0].unit == 2">{{salary[index][0].price[0].countDai}}元/{{salary[index][0].unit | formatType}}<br/></span>
                  <span v-if="(salary[index][0].valuationType == 2 || salary[index][0].valuationType == 5) && salary[index][0].unit == 1">{{salary[index][0].price[0].countSettle}}元/{{salary[index][0].unit | formatType}}<br/></span>
                  <span v-if="(salary[index][0].valuationType == 2 || salary[index][0].valuationType == 5) && salary[index][0].unit == 3">{{salary[index][0].price[0].countChe}}元/{{salary[index][0].unit | formatType}}<br/></span>
                  <span v-if="(salary[index][0].valuationType == 2 || salary[index][0].valuationType == 5) && salary[index][0].unit == 4">{{salary[index][0].price[0].countDun}}元/{{salary[index][0].unit | formatType}}<br/></span>

                  <!-- 组合 -->
                  <span v-if="(salary[index][0].valuationType == 4 || salary[index][0].valuationType == 7) && salary[index][0].price[0].countSettle">{{salary[index][0].price[0].countSettle}}元/件<br/></span>
                  <span v-if="(salary[index][0].valuationType == 4 || salary[index][0].valuationType == 7) && salary[index][0].price[0].countDai">{{salary[index][0].price[0].countDai}}元/袋<br/></span>
                  <span v-if="(salary[index][0].valuationType == 4 || salary[index][0].valuationType == 7) && salary[index][0].price[0].countChe">{{salary[index][0].price[0].countChe}}元/车<br/></span>
                  <span v-if="(salary[index][0].valuationType == 4 || salary[index][0].valuationType == 7) && salary[index][0].price[0].countDun">{{salary[index][0].price[0].countDun}}元/吨<br/></span>
                    </td>
                  </tr>
                  <tr>
                      <td>{{salary[index][0].price[1].sex == 1 ? '男' : '女'}}</td>
                      <!-- <td rowspan="2">{{item.workStartTime | formatTimeString}}</td>
                    <td rowspan="2">{{item.workEndTime | formatTimeString}}</td> -->
                      <td>
                          <!-- 计时 2-->
                  <br/>
                  <span v-if="salary[index][0].valuationType ==1 || salary[index][0].valuationType ==6 || salary[index][0].valuationType == 5 || salary[index][0].valuationType == 7">{{salary[index][0].price[1].countTime}}元/小时<br/></span>
                  <!-- 阶梯 -->
                  <span v-if="(salary[index][0].valuationType ==6 || salary[index][0].valuationType ==3) && i == 0" v-for="(data,i) in salary[index][0].price[1].ladderObj">{{data.count}}{{salary[index][0].unit | formatType}}以下(含)， {{data.unit}}元/{{salary[index][0].unit | formatType}}<br/></span>
                  
                  <span v-if="(salary[index][0].valuationType ==6 || salary[index][0].valuationType ==3) && i != 0 && i != salary[index][0].price[1].ladderObj.length - 1" v-for="(data,i) in salary[index][0].price[1].ladderObj">{{Number(salary[index][0].price[1].ladderObj[i-1].count) + 1}} - {{Number(data.count)}}{{salary[index][0].unit | formatType}}(含)， {{data.unit}}元/{{salary[index][0].unit | formatType}}<br/></span>

                  <span v-if="(salary[index][0].valuationType ==6 || salary[index][0].valuationType ==3) && i == salary[index][0].price[1].ladderObj.length - 1" v-for="(data,i) in salary[index][0].price[1].ladderObj">{{salary[index][0].price[1].ladderObj[i-1].count}}{{salary[index][0].unit | formatType}}以上， {{data.overUnit}}元/{{salary[index][0].unit | formatType}}<br/><br/></span>

                  <!-- 计量 -->
                  <span v-if="(salary[index][0].valuationType == 2 || salary[index][0].valuationType == 5) && salary[index][0].unit == 2">{{salary[index][0].price[1].countDai}}元/{{salary[index][0].unit | formatType}}<br/></span>
                  <span v-if="(salary[index][0].valuationType == 2 || salary[index][0].valuationType == 5) && salary[index][0].unit == 1">{{salary[index][0].price[1].countSettle}}元/{{salary[index][0].unit | formatType}}<br/></span>
                  <span v-if="(salary[index][0].valuationType == 2 || salary[index][0].valuationType == 5) && salary[index][0].unit == 3">{{salary[index][0].price[1].countChe}}元/{{salary[index][0].unit | formatType}}<br/></span>
                  <span v-if="(salary[index][0].valuationType == 2 || salary[index][0].valuationType == 5) && salary[index][0].unit == 4">{{salary[index][0].price[1].countDun}}元/{{salary[index][0].unit | formatType}}<br/></span>

                  <!-- 组合 -->
                  <span v-if="(salary[index][0].valuationType == 4 || salary[index][0].valuationType == 7) && salary[index][0].price[1].countSettle">{{salary[index][0].price[1].countSettle}}元/件<br/></span>
                  <span v-if="(salary[index][0].valuationType == 4 || salary[index][0].valuationType == 7) && salary[index][0].price[1].countDai">{{salary[index][0].price[1].countDai}}元/袋<br/></span>
                  <span v-if="(salary[index][0].valuationType == 4 || salary[index][0].valuationType == 7) && salary[index][0].price[1].countChe">{{salary[index][0].price[1].countChe}}元/车<br/></span>
                  <span v-if="(salary[index][0].valuationType == 4 || salary[index][0].valuationType == 7) && salary[index][0].price[1].countDun">{{salary[index][0].price[1].countDun}}元/吨<br/></span>
                      </td>
                  </tr>
                  <tr>
                      <td rowspan="2">{{salary[index][1].settlementType | formatSettle}}

                        <span class="setting-tag" v-if="salary[index][0].valuationType == 1">计时</span>
                      <span class="setting-tag" v-if="salary[index][0].valuationType == 2 || salary[index][0].valuationType == 3 || salary[index][0].valuationType == 4">计量</span>
                      <span class="setting-tag" v-if="salary[index][0].valuationType == 5 || salary[index][0].valuationType == 6 || salary[index][0].valuationType == 7">计时+计量</span>
                      </td>
                      <td>{{salary[index][1].price[0].sex == 1 ? '男' : '女'}}</td>
                      <!-- <td rowspan="2">{{item.workStartTime | formatTimeString}}</td>
                  <td rowspan="2">{{item.endWorkType == 2 ? '次日' : ''}}{{item.workEndTime | formatTimeString}}</td> -->
                      <td>
                        <!-- 计时3 -->
                  <br/>
                  <span v-if="salary[index][1].valuationType ==1 || salary[index][1].valuationType ==6 || salary[index][1].valuationType == 5 || salary[index][1].valuationType == 7">{{salary[index][1].price[0].countTime}}元/小时<br/></span>
                  <!-- 阶梯 -->
                  <span v-if="(salary[index][1].valuationType ==6 || salary[index][1].valuationType ==3) && i == 0" v-for="(data,i) in salary[index][1].price[0].ladderObj">{{data.count}}{{salary[index][1].unit | formatType}}以下(含)， {{data.unit}}元/{{salary[index][1].unit | formatType}}<br/></span>
                  
                  <span v-if="(salary[index][1].valuationType ==6 || salary[index][1].valuationType ==3) && i != 0 && i != salary[index][1].price[0].ladderObj.length - 1" v-for="(data,i) in salary[index][1].price[0].ladderObj">{{Number(salary[index][1].price[0].ladderObj[i-1].count) + 1}} - {{Number(data.count)}}{{salary[index][1].unit | formatType}}(含)， {{data.unit}}元/{{salary[index][1].unit | formatType}}<br/></span>

                  <span v-if="(salary[index][1].valuationType ==6 || salary[index][1].valuationType ==3) && i == salary[index][1].price[0].ladderObj.length - 1" v-for="(data,i) in salary[index][1].price[0].ladderObj">{{salary[index][1].price[0].ladderObj[i-1].count}}{{salary[index][1].unit | formatType}}以上， {{data.overUnit}}元/{{salary[index][1].unit | formatType}}<br/></span>

                  <!-- 计量 -->
                  <span v-if="(salary[index][1].valuationType == 2 || salary[index][1].valuationType == 5) && salary[index][1].unit == 2">{{salary[index][1].price[0].countDai}}元/{{salary[index][1].unit | formatType}}<br/></span>
                  <span v-if="(salary[index][1].valuationType == 2 || salary[index][1].valuationType == 5) && salary[index][1].unit == 1">{{salary[index][1].price[0].countSettle}}元/{{salary[index][1].unit | formatType}}<br/></span>
                  <span v-if="(salary[index][1].valuationType == 2 || salary[index][1].valuationType == 5) && salary[index][1].unit == 3">{{salary[index][1].price[0].countChe}}元/{{salary[index][1].unit | formatType}}<br/></span>
                  <span v-if="(salary[index][1].valuationType == 2 || salary[index][1].valuationType == 5) && salary[index][1].unit == 4">{{salary[index][1].price[0].countDun}}元/{{salary[index][1].unit | formatType}}<br/></span>

                  <!-- 组合 -->
                  <span v-if="(salary[index][1].valuationType == 4 || salary[index][1].valuationType == 7) && salary[index][1].price[0].countSettle">{{salary[index][1].price[0].countSettle}}元/件<br/></span>
                  <span v-if="(salary[index][1].valuationType == 4 || salary[index][1].valuationType == 7) && salary[index][1].price[0].countDai">{{salary[index][1].price[0].countDai}}元/袋<br/></span>
                  <span v-if="(salary[index][1].valuationType == 4 || salary[index][1].valuationType == 7) && salary[index][1].price[0].countChe">{{salary[index][1].price[0].countChe}}元/车<br/></span>
                  <span v-if="(salary[index][1].valuationType == 4 || salary[index][1].valuationType == 7) && salary[index][1].price[0].countDun">{{salary[index][1].price[0].countDun}}元/吨<br/></span>
                  <br/>
                      </td>
                  </tr>
                  <tr>
                      <td>{{salary[index][1].price[1].sex == 1 ? '男' : '女'}}</td>
                      <!-- <td rowspan="2">{{item.workStartTime | formatTimeString}}</td>
                    <td rowspan="2">{{item.workEndTime | formatTimeString}}</td> -->
                      <td>
                        <!-- 计时4 -->
                  <br/>
                  <span v-if="salary[index][1].valuationType ==1 || salary[index][1].valuationType ==6 || salary[index][1].valuationType == 5 || salary[index][1].valuationType == 7">{{salary[index][1].price[1].countTime}}元/小时<br/></span>
                  <!-- 阶梯 -->
                  <span v-if="(salary[index][1].valuationType ==6 || salary[index][1].valuationType ==3) && i == 0" v-for="(data,i) in salary[index][1].price[1].ladderObj">{{data.count}}{{salary[index][1].unit | formatType}}以下(含)， {{data.unit}}元/{{salary[index][1].unit | formatType}}<br/></span>
                  
                  <span v-if="(salary[index][1].valuationType ==6 || salary[index][1].valuationType ==3) && i != 0 && i != salary[index][1].price[1].ladderObj.length - 1" v-for="(data,i) in salary[index][1].price[1].ladderObj">{{Number(salary[index][1].price[1].ladderObj[i-1].count) + 1}} - {{Number(data.count)}}{{salary[index][1].unit | formatType}}(含)， {{data.unit}}元/{{salary[index][1].unit | formatType}}<br/></span>

                  <span v-if="(salary[index][1].valuationType ==6 || salary[index][1].valuationType ==3) && i == salary[index][1].price[1].ladderObj.length - 1" v-for="(data,i) in salary[index][1].price[1].ladderObj">{{salary[index][1].price[1].ladderObj[i-1].count}}{{salary[index][1].unit | formatType}}以上， {{data.overUnit}}元/{{salary[index][1].unit | formatType}}<br/></span>

                  <!-- 计量 -->
                  <span v-if="(salary[index][1].valuationType == 2 || salary[index][1].valuationType == 5) && salary[index][1].unit == 2">{{salary[index][1].price[1].countDai}}元/{{salary[index][1].unit | formatType}}<br/></span>
                  <span v-if="(salary[index][1].valuationType == 2 || salary[index][1].valuationType == 5) && salary[index][1].unit == 1">{{salary[index][1].price[1].countSettle}}元/{{salary[index][1].unit | formatType}}<br/></span>
                  <span v-if="(salary[index][1].valuationType == 2 || salary[index][1].valuationType == 5) && salary[index][1].unit == 3">{{salary[index][1].price[1].countChe}}元/{{salary[index][1].unit | formatType}}<br/></span>
                  <span v-if="(salary[index][1].valuationType == 2 || salary[index][1].valuationType == 5) && salary[index][1].unit == 4">{{salary[index][1].price[1].countDun}}元/{{salary[index][1].unit | formatType}}<br/></span>

                  <!-- 组合 -->
                  <span v-if="(salary[index][1].valuationType == 4 || salary[index][1].valuationType == 7) && salary[index][1].price[1].countSettle">{{salary[index][1].price[1].countSettle}}元/件<br/></span>
                  <span v-if="(salary[index][1].valuationType == 4 || salary[index][1].valuationType == 7) && salary[index][1].price[1].countDai">{{salary[index][1].price[1].countDai}}元/袋<br/></span>
                  <span v-if="(salary[index][1].valuationType == 4 || salary[index][1].valuationType == 7) && salary[index][1].price[1].countChe">{{salary[index][1].price[1].countChe}}元/车<br/></span>
                  <span v-if="(salary[index][1].valuationType == 4 || salary[index][1].valuationType == 7) && salary[index][1].price[1].countDun">{{salary[index][1].price[1].countDun}}元/吨<br/></span>
                  <br/>
                      </td>
                  </tr>
              </tbody>
            </table>
        </div>
    </div>
</template>

<script>
  import BigImage from "../../../../components/bigImage.vue";
  import ElButton from "../../../../../node_modules/element-ui/packages/button/src/button.vue";

  export default {
    name: "",
    components: {
      ElButton,
      BigImage
    },
    data() {
      return {
        form: [{},{}],
        visible: false,
        msg: "Hello Vue.js",
        salary:[]
      }
    },

    filters: {
      formatTimeString(val){
        if(val){
          return val.substring(0,5)
        }
      },
      formatSettle(val){
        if(val==1){
          return '日结或周结'
        }else if(val==2){
          return '月结'
        }else if(val==3){
          return '实时结'
        }else{
          return ''
        }
      },
      formataccountStatus(val) {
        if (val == 1) {
          return '可用'
        } else if (val == 2) {
          return '冻结'
        } else {
          return ''
        }
      },
      formatFeeStatus(val) {
        if (val == 1) {
          return '未开通'
        } else if (val == 2) {
          return '免费试用'
        } else if (val == 3) {
          return '正常'
        } else if (val == 4) {
          return '欠费'
        } else {
          return ''
        }
      },
      formatAccountStatus(val) {

      },
      formatType(val) {
        if (val == 1) {
          return '件'
        } else if (val == 2) {
          return '袋'
        } else if (val == 3) {
          return '车'
        } else {
          return '吨'
        }
      },
    },

    created() {

    },
    mounted() {
      this.getaskSalaryList()
    },
    methods: {

      getaskSalaryList(){
        const params = {
          id:this.$route.params.id,
//          id:1,
        }
        this.$api.scheduleSetting(params).then((response) => {
          if( response.respCode==0){
            this.form = response.data
            for(let i in this.form){
              this.salary.push(JSON.parse(this.form[i].salary))
              // this.
            }
            console.log(this.salary)
          }
        }).catch((error) => {
          console.log(error);
        });
      },
      handleWrapperClick() {
        this.visible = false
      },
    }
  }
</script>

<style scoped>
    table{
        border-right:solid 1px #e4e4e4;
        border-bottom:solid 1px #e4e4e4;
        font-size: 14px;
    }
    td{
        border-left:solid 1px #e4e4e4;
        border-top:solid 1px #e4e4e4;
        /* height: 140px; */
    }
    .header{
        background-color: #f6f6f6;
        font-family: MicrosoftYaHei;
        font-weight: normal;
        font-stretch: normal;
        letter-spacing: 0px;
        color: #262626;
    }
    .header td{
        height: 40px;
    }
    .setting-info-table{
        width:100%;
        text-align: center;
    }
    .setting-tag{
        border-radius: 8px;
        border: solid 1px #e84518;
        font-family: PingFang-SC-Medium;
        font-size: 12px;
        letter-spacing: 0px;
        color: #e84518;
        padding: 4px 5px;
        /* margin-left: 17px; */
    }
</style>