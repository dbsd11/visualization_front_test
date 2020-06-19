<template>
  <div id="view-container2">
          <el-row>
          <el-col :span="15">
          <div class="data-view">
            <div class="main-title">基础设施统计</div>
            <el-row :gutter="20" type="flex" justify="center">
              <el-col :span="5">
                  <div class="common-title">
                    <img src="../../assets/moka/title.png"/>
                    <label>主机</label>
                  </div>
                  <el-form :label-position="left" ref="form" label-width="80px">
                      <template v-for='(machineStat,index) in machineStats'>
                          <el-form-item :label="machineStat.statLabel">
                            {{machineStat.statValue}}
                          </el-form-item>
                      </template>
                  </el-form>
              </el-col>
              <el-col :span="5">
                  <div class="common-title">
                    <img src="../../assets/moka/title.png"/>
                    <label>存储</label>
                  </div>
                  <el-form :label-position="left" ref="form" label-width="80px">
                      <template v-for='(machineStat,index) in machineStats'>
                          <el-form-item :label="machineStat.statLabel">
                            {{machineStat.statValue}}
                          </el-form-item>
                      </template>
                  </el-form>
              </el-col>
              <el-col :span="5">
                  <div class="common-title">
                    <img src="../../assets/moka/title.png"/>
                    <label>路由器</label>
                  </div>
                  <el-form :label-position="left" ref="form" label-width="80px">
                      <template v-for='(machineStat,index) in machineStats'>
                          <el-form-item :label="machineStat.statLabel">
                            {{machineStat.statValue}}
                          </el-form-item>
                      </template>
                  </el-form>
              </el-col>
            </el-row>
          </div>
          </el-col>
          <el-col :span="9">
            <div class="quality-all-equip">
                <div class="all-equip-title">报警情况总览</div>
                <div class="error-num-view">
                    <div class="infinite-list-wrapper" style="overflow:auto">
                        <ul class="list" v-infinite-scroll="errorLoad" infinite-scroll-disabled="errorDisabled">
                            <li v-for="error in errorList" class="list-item">{{ error.message }}</li>
                        </ul>
                        <p v-if="errorLoading">加载中...</p>
                        <p v-if="errorNoMore">没有更多了</p>
                    </div>
                </div>
            </div>
          </el-col>
          </el-row>
          <br/><br/><br/>
          <el-row>
          <div class="application-view">
            <div class="app-title">主<br/>机<br/>信<br/>息<br/>查<br/>询<br/></div>
            <div class="common-title app-common-title">
              <img src="../../assets/moka/title.png"/>
              <label>主机信息查询</label>
            </div>
            <div class="application-main-view">
                <el-form ref="machineSearchForm" :model="machineSearchForm" label-width="70px" class="searchInput">
                  <el-row class="label-first" :gutter="20">
                    <el-col :lg="12" :xl="12">
                      <el-form-item label="主机名称">
                        <el-input v-model.trim="keywords" size="small" type="text" placeholder="请输入主机名称" />
                      </el-form-item>
                    </el-col>
                    <el-col :lg="12" :xl="12">
                      <el-form-item label="主机ip">
                        <el-input v-model.trim="ip" size="small" type="text" placeholder="请输入主机ip" />
                      </el-form-item>
                    </el-col>
                    <el-col :lg="24" :xl="24">
                      <el-form-item label="标签">
                        <el-tag v-for="dynamicTag in tag.dynamicTags" closable :disable-transitions="false" @close="tagHandleClose(dynamicTag)">{{dynamicTag}}</el-tag>
                        <el-input class="input-new-tag" v-if="tag.inputVisible" v-model="tag.inputValue"
                          ref="saveTagInput" size="small" @keyup.enter.native="tagHandleInputConfirm" @blur="tagHandleInputConfirm">
                        </el-input>
                        <el-button v-else class="button-new-tag" size="small" @click="tagShowInput">+ New Tag</el-button>
                      </el-form-item>
                    </el-col>
                    </el-col>
                  </el-row>
                </el-form>
                <el-row :gutter="20" type="flex" justify="space-around" align="middle">
                  <el-col :lg="16" :xl="16">
                    <el-collapse v-model="machineSelectId" accordion style="background-color: transparent">
                    <el-collapse-item title="bzdbdfap1001" name="1">
                      <div>系统名称: 数据工厂</div>
                      <div>服务名称：智能决策后管</div>
                      <div>操纵系统: Linux7u01</div>
                      <div>cpu核数：2</div>
                      <div>内存: 4000MB</div>
                      <div>硬盘: 50000MB</div>
                    </el-collapse-item>
                    <el-collapse-item title="bzdbdfap1002" name="2">
                      <div>系统名称: 数据工厂</div>
                      <div>服务名称：智能决策后管</div>
                      <div>操纵系统: Linux7u01</div>
                      <div>cpu核数：2</div>
                      <div>内存: 4000MB</div>
                      <div>硬盘: 50000MB</div>
                    </el-collapse-item>
                    </el-collapse>
                  </el-col>
                  <el-col :lg="8" :xl="8">
                    <el-button class="selected">查看更多</el-button>
                  </el-col>
                </el-row>
            </div>       
          </div>  
          <br/><br/><br/>
          <div class="application-view">
            <div class="app-title">存<br/>储<br/>信<br/>息<br/>查<br/>询<br/></div>
            <div class="common-title app-common-title">
              <img src="../../assets/moka/title.png"/>
              <label>存储信息查询</label>
            </div>
            <div class="application-main-view">
                <el-form ref="machineSearchForm" :model="machineSearchForm" label-width="70px" class="searchInput">
                  <el-row class="label-first" :gutter="20">
                    <el-col :lg="12" :xl="12">
                      <el-form-item label="主机名称">
                        <el-input v-model.trim="keywords" size="small" type="text" placeholder="请输入主机名称" />
                      </el-form-item>
                    </el-col>
                    <el-col :lg="12" :xl="12">
                      <el-form-item label="主机ip">
                        <el-input v-model.trim="ip" size="small" type="text" placeholder="请输入主机ip" />
                      </el-form-item>
                    </el-col>
                    <el-col :lg="24" :xl="24">
                      <el-form-item label="标签">
                        <el-tag v-for="dynamicTag in tag.dynamicTags" closable :disable-transitions="false" @close="tagHandleClose(dynamicTag)">{{dynamicTag}}</el-tag>
                        <el-input class="input-new-tag" v-if="tag.inputVisible" v-model="tag.inputValue"
                          ref="saveTagInput" size="small" @keyup.enter.native="tagHandleInputConfirm" @blur="tagHandleInputConfirm">
                        </el-input>
                        <el-button v-else class="button-new-tag" size="small" @click="tagShowInput">+ New Tag</el-button>
                      </el-form-item>
                    </el-col>
                    </el-col>
                  </el-row>
                </el-form>
                <el-row :gutter="20" type="flex" justify="space-around" align="middle">
                  <el-col :lg="16" :xl="16">
                    <el-collapse v-model="storageSelectId" accordion style="background-color: transparent">
                    <el-collapse-item title="bzdbdfap1001" name="1">
                      <div>系统名称: 数据工厂</div>
                      <div>服务名称：智能决策后管</div>
                      <div>操纵系统: Linux7u01</div>
                      <div>cpu核数：2</div>
                      <div>内存: 4000MB</div>
                      <div>硬盘: 50000MB</div>
                    </el-collapse-item>
                    <el-collapse-item title="bzdbdfap1002" name="2">
                      <div>系统名称: 数据工厂</div>
                      <div>服务名称：智能决策后管</div>
                      <div>操纵系统: Linux7u01</div>
                      <div>cpu核数：2</div>
                      <div>内存: 4000MB</div>
                      <div>硬盘: 50000MB</div>
                    </el-collapse-item>
                    </el-collapse>
                  </el-col>
                  <el-col :lg="8" :xl="8">
                    <el-button class="selected">查看更多</el-button>
                  </el-col>
                </el-row>
            </div>       
          </div>
        </el-row>   
      </div>
</template>

<script>
  import { mapGetters } from 'vuex'

  // 组件
  import SafeEvent from '../moka/components/SafeEvent'
  import EquipNum from '../moka/components/EquipNum'
  import AppNum from '../moka/components/AppNum'
  import WarnNum from '../moka/components/WarnNum'
  import VisitNum from '../moka/components/VisitNum'
  import EquipStatus from '../moka/components/EquipStatus'
  import UseRate from '../moka/components/UseRate'
  import DataCapa from '../moka/components/DataCapa'
  import AvnuTime from '../moka/components/AvnuTime'
  import TimeAnal from '../moka/components/TimeAnal'
  import WorkOrder from '../moka/components/WorkOrder'
  import AccuseStatus from '../moka/components/AccuseStatus'
  import HostView from '../moka/components/HostView'
  import HostNum from '../moka/components/HostNum'
  import MonitorNum from '../moka/components/MonitorNum'
  import ErrorNum from '../moka/components/ErrorNum'
  import DataIncrease from '../moka/components/DataIncrease'
  import BackupRate from '../moka/components/BackupRate'
  import SafeStatus from '../moka/components/SafeStatus'
  import AppStatus from '../moka/components/AppStatus'
  import OverAll from '../moka/components/OverAll'
  import StatusWater from '../moka/components/StatusWater'

  // 接口
  import { getMonitorCount } from '../../api/moka'
  import { getHostView } from '../../api/moka'
  import { getUserRateWithType, getMonitorWithType, getSysIssusStatistics, getBaseDeviceData, getSafeStatusData } from '../../api/moka'
  import { listAlertWithDeviceType } from '../../api/moka'
  import { getSafeEventData } from '../../api/moka'

  export default {
    name: 'iaasStat',
    components: {
      SafeEvent,
      EquipNum,
      AppNum,
      EquipStatus,
      WarnNum,
      UseRate,
      DataCapa,
      AvnuTime,
      VisitNum,
      TimeAnal,
      WorkOrder,
      AccuseStatus,
      HostView,
      HostNum,
      MonitorNum,
      ErrorNum,
      DataIncrease,
      BackupRate,
      SafeStatus,
      AppStatus,
      OverAll,
      StatusWater
    },
    data() {
      return {
        machineSelectId: null,
        storageSelectId: null,
        machineStats: [
          {
            "statKey": "machineStat.hostCount",
            "statLabel": "主机总数",
            "statValue": "10"
          },
          {
            "statKey": "machineStat.hostCount",
            "statLabel": "已停止",
            "statValue": "1"
          },
          {
            "statKey": "machineStat.hostCount",
            "statLabel": "异常",
            "statValue": "1"
          },
        ],
        errorCount: 0,
        errorLoading: false,
        errorList: [
          {
            "message": "可用内存低于10%"
          },
          {
            "message": "cpu占用过高"
          },
          {
            "message": "磁盘空间不足10%"
          }
        ],
        tag:{
          dynamicTags: ['标签一', '标签二', '标签三'],
          inputVisible: false,
          inputValue: ''
        }
      }
    },
    computed: {
      ...mapGetters([
      ]),
      errorNoMore () {
        return this.errorCount >= 20
      },
      errorDisabled () {
        return this.errorLoading || this.errorNoMore
      }
    },
    methods: {
      tagHandleClose(dynamicTag) {
        this.tag.dynamicTags.splice(this.tag.dynamicTags.indexOf(dynamicTag), 1);
      },

      tagShowInput() {
        this.tag.inputVisible = true;
        this.$nextTick(_ => {
          this.$refs.saveTagInput.$refs.input.focus();
        });
      },

      tagHandleInputConfirm() {
        let inputValue = this.tag.inputValue;
        if (inputValue) {
          this.tag.dynamicTags.push(inputValue);
        }
        this.tag.inputVisible = false;
        this.tag.inputValue = '';
      },
      errorLoad () {
        this.errorLoading = true
        setTimeout(() => {
          this.errorCount += 2
          this.errorLoading = false
        }, 2000)
      }
    }
  }
</script>

<style scoped>
  #view-container {
    padding: 0px;
    margin: 0px;
    top: 0px;
    left: 0px;
    border: hidden;
    width: 100%;
    height: 100%;
    position: absolute;
    background: url("../../assets/moka/bg.png") no-repeat center fixed;
    background-size: 100% 100%;
    font-family: "Microsoft YaHei";
    overflow: hidden;
  }

  .data-view {
    z-index: 1;
    position: relative;
    width: 800px;
    top: 2%;
    left: 1%;
    background: url("../../assets/moka/aqsj-bg.png") no-repeat;
    background-size: 100% 100%;
  }

  .main-title {
    position: relative;
    color: #fff;
    font-size: 0.35rem;
    font-weight: bold;
    width: 40%;
    margin: auto;
    top: 3%;
    letter-spacing: 3px;
    text-align: center;
  }

  .common-title {
    position: relative;
    top: 14px;
    left: 14px;
    width: 140px;
    height: 60px;
    text-align: left;
    color: #fff;
  }

  .common-title img {
    position: absolute;
    width: 100%;
    height: 50%;
  }

  .common-title label {
    position: absolute;
    top: 40%;
    left: 8%;
    font-size: 0.25rem;
  }

  .application-view {
    z-index: 1;
    position: relative;
    width: 800px;
    height: 400px;
    top: 3%;
    left: 1%;
    background: url("../../assets/moka/yy.png") no-repeat;
    background-size: 100% 100%;
  }

  .app-title {
    position: relative;
    color: #fff;
    font-size: 0.4rem;
    font-weight: bold;
    top: 10%;
    left: 4%;
    letter-spacing: 8px;
  }

  .app-common-title {
    position: absolute;
    left: 20%;
    top: 16%;
  }

  .application-main-view {
    /*background-color: #fff;*/
    position: absolute;
    width: 62%;
    left: 36%;
    height: 70%;
    top: 18%;
  }

  .quality-all-equip {
    position: absolute;
    disable: inline-block;
    float: right;
    width: 25%;
    left: 65%;
    height: 100%;
    background: url("../../assets/moka/ztjkqk.png") no-repeat;
    background-size: 100% 100%;
  }

  .all-equip-title {
    position: absolute;
    color: #fff;
    width: 84%;
    height: 10%;
    margin-left: 8%;
    top: 11%;
    /*line-height: 40px;*/
    vertical-align: center;
    font-size: 0.3rem;
    text-align: center;
  }

  .error-num-view {
    position: absolute;
    width: 87%;
    height: 66%;
    margin-left: 6%;
    top: 25%;
    /*background-color: #fff;*/
  }

  .el-tag + .el-tag {
    margin-left: 10px;
  }
  .button-new-tag {
    margin-left: 10px;
    height: 32px;
    line-height: 30px;
    padding-top: 0;
    padding-bottom: 0;
  }
  .input-new-tag {
    width: 90px;
    margin-left: 10px;
    vertical-align: bottom;
  }

  /deep/ .el-form-item__label {
    color: #cecdd9
  }

  /deep/ .el-form-item__content {
    color: #61f551
  }

  .infinite-list-wrapper .list-item {
    color: #ef2b21
  }

  .selected {
    background: url("../../assets/moka/selected.png") no-repeat;
    background-size: 100% 100%;
  }

  .un-select {
    background: url("../../assets/moka/unselect.png") no-repeat;
    background-size: 100% 100%;
  }

</style>
