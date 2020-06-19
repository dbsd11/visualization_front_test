<template>
  <div id="view-container">
      <div class="view-title">
        <div :class=" indexStatus===1?'primary-title':'secondary-title' " @click="indexStatus=1">基础设施</div>
        <div :class=" indexStatus===2?'primary-title':'secondary-title' " @click="indexStatus=2">集群信息</div>
        <div :class=" indexStatus===3?'primary-title':'secondary-title' " @click="indexStatus=3">应用管理</div>
      </div>
      <div class="view-body">
        <div v-if="indexStatus === 1">
          <IaasStat/>
        </div>
        <div v-if="indexStatus === 2">
            <ClusterInfo/>
        </div>
        <div v-if="indexStatus === 3">
            <ServiceInfo/>
        </div>
      </div>
  </div>

</template>

<script>
  import { mapGetters } from 'vuex'

  import IaasStat from './iaasStat'
  import ClusterInfo from './clusterInfo'
  import ServiceInfo from './serviceInfo'

  export default {
    name: 'testView',
    components: {
      IaasStat,
      ClusterInfo,
      ServiceInfo
    },
    data() {
      return {
        indexStatus: 1,
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
    height: 200%;
    position: absolute;
    background: url("../../assets/moka/bg.png") no-repeat center fixed;
    background-size: 100% 100%;
    font-family: "Microsoft YaHei";
    overflow: hidden;
  }

  .view-title {
    position: fixed;
    width: 24%;
    height: 8%;
    margin: 2% 38%;
    /*background-color: #fff;*/
    text-align: center;
  }

  .view-title div {
    padding: 0;
    display: inline-block;
    position: relative;
    height: 100%;
    color: #fff;
  }

  .view-body {
    padding: 5% 2% 2% 5%;
  }

  .primary-title {
    width: 34%;
    font-size: 0.45rem;
    font-weight: 900;
  }

  .secondary-title {
    width: 30%;
    font-size: 0.35rem;
    font-weight: 700;
  }
</style>
