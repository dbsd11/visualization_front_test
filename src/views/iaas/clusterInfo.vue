<template>
    <div id="view-container2">
        <el-row :gutter="200" >
            <el-col :span="5">
                <div class="data-view">
                    <div class="main-title">集群信息</div>
                    <br/><br/><br/><br/><br/>
                    <el-row>
                        <div class="common-title">
                            <img src="../../assets/moka/title.png"/>
                            <label>机房列表</label>
                        </div>
                        <div class="data-status">

                        </div>
                    </el-row>
                    <br/>
                    <el-row>
                        <div class="common-title">
                            <img src="../../assets/moka/title.png"/>
                            <label>集群列表</label>
                        </div>
                        <div class="data-status">

                        </div>
                    </el-row>
                    <br/>
                    <el-row>
                        <div class="common-title">
                            <img src="../../assets/moka/title.png"/>
                            <label>命名空间列表</label>
                        </div>
                        <div class="data-status">
                        </div>
                    </el-row>
                </div>
            </el-col>
            <el-col :span="16">
                <div style="color:#fff;z-index: 1;position: absolute;text-align: center;margin-left: 30%;margin-top: 450px;height: 200p;font-size: large;vertical-align: baseline;">
                    <label>集群地图</label>
                </div>
                <div class="dataMap-view">
                    <div style="background: url('https://img-blog.csdnimg.cn/20200324104057618.gif') no-repeat center;background-size:80% 60%;height:100%;width:100%"></div>
                </div>
                <br/>
                <div style="position:relative;left:8%">
                    <div class="common-title">
                        <img src="../../assets/moka/title.png"/>
                        <label>详细信息</label>
                    </div>
                    <br/>
                    <div class="accuse-all">
                    </div>
                    
                    <br/>
                    <div class="accuse-status">
                        <div class="tip-title">
                        <div class="tip-img"></div>
                        <label>运维工单</label>
                        </div>
                        <div class="accuse-status-view">
                        <accuse-status :object="webPlatform"></accuse-status>
                        </div>
                    </div>
                </div>
                
            </el-col>
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
        
      }
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

  .data-view {
    z-index: 1;
    position: relative;
    width: 350px;
    height: 1200px;
    top: 2%;
    left: 1%;
    background: url("../../assets/moka/aqsj-bg.png") no-repeat;
    background-size: 100% 100%;
  }

  .data-status {
    position: relative;
    top: 8%;
    width: 88%;
    height: 250px;
    margin: auto;
    background: url("../../assets/moka/aqsbzt-bg.png") no-repeat;
    background-size: 100% 100%;
  }

  .main-title {
    position: relative;
    color: #fff;
    font-size: 0.4rem;
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

  .dataMap-view {
    position: relative;
    top: 8%;
    left: 5%;
    width: 95%;
    height: 500px;
    margin: auto;
    background: url("../../assets/moka/aqts-bg2.png") no-repeat;
    background-size: 100% 100%;
  }

  .accuse-all {
    position: relative;
    width: 90%;
    height: 300px;
    left: 3%;
    background: url("../../assets/moka/tscltj.png");
    background-size: 100% 100%;
  }

  .accuse-status {
    position: relative;
    width: 100%;
    height: 40%;
    left: 3%;
    /*margin-top: 1%;*/
    /*background-color: #fff;*/
  }

  .tip-title {
    position: relative;
    width: 100%;
    height: 20px;
    color: #fff;
    font-size: 16px;
    line-height: 20px;
    vertical-align: middle;
  }

  .tip-img {
    position: absolute;
    width: 10px;
    height: 20px;
    background-color: #32bef2;
  }

  .tip-title label {
    position: absolute;
    left: 15px;
  }

  .accuse-status-view {
    position: relative;
    width: 90%;
    height: 200px;
    margin-top: 2%;
    /*background-color: #fff;*/
    background: url("../../assets/moka/tsqkbg.png");
    background-size: 100% 100%;
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
