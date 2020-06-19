<template>
        <div id="view-container2">
            <el-row :gutter="20" type="flex" justify="center">
              <el-col :span=6>
                  <div class="Infrastructure-select">
                    <span :class=" selectIndex==0 ? 'selected' : 'un-select'" @click="selectIndex=0">系统管理</span>
                    <br/>
                    <span :class=" selectIndex==1 ? 'selected' : 'un-select'" @click="selectIndex=1">应用管理</span>
                    <br/>
                    <span :class=" selectIndex==2 ? 'selected' : 'un-select'" @click="selectIndex=2">ambari</span>
                    <br/>
                    <span :class=" selectIndex==3 ? 'selected' : 'un-select'" @click="selectIndex=3">cloudera manager</span>
                    <br/>
                  </div>
              </el-col>
              <el-col :span=18>
                  <div id="clusterManageList" v-if="selectIndex==0">
                    <dv-border-box-13>
                    <div class="bg-color-black">
                      <div class="d-flex">
                        <span style="color:#5cd9e8">
                          <icon name="chart-line"></icon>
                        </span>
                        <div class="d-flex">
                          <span style="font-size:0.3rem;color:#c3cbde">系统列表</span>
                        </div>
                      </div>
                        <el-row  type="flex" justify="space-around" align="middle">
                          <el-col :span=22>
                              <dv-scroll-board :config="clusterManageListConfig"/>
                          </el-col>
                          <el-col :span=2>
                            <icon name="plus-square" scale="2" class="dv-shadow"/>
                            <br/><br/><br/>
                            <icon name="check-square" scale="2" class="dv-shadow"/>
                          </el-col>
                        </el-row>
                    </div>
                    </dv-border-box-13>
                  </div>
                  <div id="serviceManageList" v-if="selectIndex==1">
                    <dv-border-box-13>
                    <div class="bg-color-black">
                      <div class="d-flex">
                        <span style="color:#5cd9e8">
                          <icon name="chart-line"></icon>
                        </span>
                        <div class="d-flex">
                          <span style="font-size:0.3rem;color:#c3cbde">应用列表</span>
                        </div>
                      </div>
                        <el-row  type="flex" justify="space-around" align="middle">
                          <el-col :span=22>
                              <dv-scroll-board :config="serviceManageListConfig"/>
                          </el-col>
                          <el-col :span=2>
                            <icon name="plus-square" scale="2" class="dv-shadow"/>
                            <br/><br/><br/>
                            <icon name="check-square" scale="2" class="dv-shadow"/>
                          </el-col>
                        </el-row>
                    </div>
                    </dv-border-box-13>
                  </div>
              </el-col>
            </el-row>
        </div>
</template>

<script>
  import { mapGetters } from 'vuex'

  import '@/utils/flexible.js' // flexible

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
        selectIndex: 0,
        clusterManageListConfig: {
          header: ["名称", "所属机房", "所在子网", "命名空间", "应用列表", "<span style='padding-left:0.3rem'>操作</span>"],
          data: [
            ["bmr", "中金", "172.16.xx.xx", "", "['kafka', 'elasticsearch','hdfs']", "<span style='padding-left:0.3rem'>查看详情</span>"],
            ["HDP", "中金", "172.17.xx.xx", "", "['kafka', 'elasticsearch','hdfs']", "<span style='padding-left:0.3rem'>查看详情</span>"],
            ["备用集群", "汇天", "172.18.xx.xx", "", "['kafka', 'elasticsearch','hdfs']", "<span style='padding-left:0.3rem'>查看详情</span>"],
            ["数据工厂", "中金", "172.16.100.xx", "主集群", "['智能决策']", "<span style='padding-left:0.3rem'>查看详情</span>"], 
            ["数据工厂", "中金", "172.16.101.xx", "备集群", "['智能决策']", "<span style='padding-left:0.3rem'>查看详情</span>"],
            ["数据工厂", "中金", "172.17.100.xx", "测试环境A", "['智能决策']", "<span style='padding-left:0.3rem'>查看详情</span>"]
          ],
          rowNum: 10, //表格行数
          headerHeight: 35,
          headerBGC: "#0f1325", //表头
          oddRowBGC: "#0f1325", //奇数行
          evenRowBGC: "#171c33", //偶数行
          index: true,
          columnWidth: [50],
          align: ["center"]
        },
        serviceManageListConfig: {
          header: ["名称", "所属系统", "命名空间", "机器列表", "<span style='padding-left:0.3rem'>操作</span>"],
          data: [
            ["kafka", "bmr", "", "['172.16.100.22','172.16.100.23','172.16.100.24']", "<span style='padding-left:0.3rem'>查看详情</span>"],
            ["elasticsearch", "bmr", "", "['172.16.100.22','172.16.100.22','172.16.100.22']", "<span style='padding-left:0.3rem'>查看详情</span>"],
            ["智能决策", "数据工厂", "主集群", "['172.16.100.15','172.16.100.16','172.100.145.17']", "<span style='padding-left:0.3rem'>查看详情</span>"],
          ],
          rowNum: 10, //表格行数
          headerHeight: 35,
          headerBGC: "#0f1325", //表头
          oddRowBGC: "#0f1325", //奇数行
          evenRowBGC: "#171c33", //偶数行
          index: true,
          columnWidth: [50],
          align: ["center"]
        }
      };
    },
    computed: {
      ...mapGetters([
      ])
    },
    methods: {
      
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

  .Infrastructure-select {
    position: relative;
    width: 100%;
    height: 800px;
    margin: auto;
    margin-top: 10%;
    text-align: center;
  }

  .Infrastructure-select span {
    display: inline-block;
    position: relative;
    background-size: 100% 100%;
    height: 7%;
    line-height: 100%;
    vertical-align: middle;
    padding-top: 5%;
    padding-bottom: 5%;
    margin-top: 5%;
    font-size: 0.3rem;
    width: 50%;
    color: #fff;
  }

  #clusterManageList {
    position: relative;
    display: flex;
    min-width: 3.75rem;
    padding: 0.2rem;
    border-radius: 0.0625rem;
  }

  #clusterManageList .dv-border-box-13 {
    width:14rem;
    height:11.5rem;
    position: relative;
  }

  #clusterManageList .bg-color-black {
    border-radius: 0.125rem;
    background-color: rgba(19, 25, 47, 0.6);
    margin:0.25rem 0.3rem 0rem 0.2rem;
    padding:1% 0 1rem 0;
  }

  #clusterManageList .body-box {
    border-radius: 0.125rem;
    overflow: hidden;
  }

  #clusterManageList .dv-scroll-board {
    margin-top: 0.5rem;
    width: 12rem;
    height: 9rem;
  }

  #serviceManageList {
    position: relative;
    display: flex;
    min-width: 3.75rem;
    padding: 0.2rem;
    border-radius: 0.0625rem;
  }

  #serviceManageList .dv-border-box-13 {
    width:14rem;
    height:11.5rem;
    position: relative;
  }

  #serviceManageList .bg-color-black {
    border-radius: 0.125rem;
    background-color: rgba(19, 25, 47, 0.6);
    margin:0.25rem 0.3rem 0rem 0.2rem;
    padding:1% 0 1rem 0;
  }

  #serviceManageList .body-box {
    border-radius: 0.125rem;
    overflow: hidden;
  }

  #serviceManageList .dv-scroll-board {
    margin-top: 0.5rem;
    width: 12rem;
    height: 9rem;
  }


  .selected {
    background: url("../../assets/moka/selected.png") no-repeat;
    background-size: 100% 100%;
  }

  .un-select {
    background: url("../../assets/moka/unselect.png") no-repeat;
    background-size: 100% 100%;
  }

  .dv-shadow {
    -webkit-box-shadow:inset 0px 0px 20px #fff;
    -moz-box-shadow:inset 0px 0px 20px #fff;
    box-shadow:inset 0px 0px 20px #fff;
    border-radius:8px;
  }
</style>
