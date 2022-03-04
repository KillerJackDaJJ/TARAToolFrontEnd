<template>
  <div id="COUNTERMEASURE" ref="COUNTERMEASURE">
    <el-tabs v-model="activeName" @tab-click="handleClick">
      <el-tab-pane v-for="item in tableData" :key=item.Threat :label=item.Threat :name=item.Threat>
        <table border="1">
            <tr>
                <td width="200px">资产</td>
                <td width="200px">风险值</td>
                <td width="200px">缓解措施</td>
            </tr>
            <tr>
                <td width="200px">{{item.Asset_}}</td>
                <td width="200px">{{item.Risk_Value}}</td>
                <td width="200px">{{item.CounterMeasure}}</td>
            </tr>
        </table>
      </el-tab-pane>
    </el-tabs>
  </div>

</template>

<script>
  export default {
    name: 'COUNTERMEASURE',
    props:['tableData'],
    watch: {
        tableData(n){
            this.tableData=n
        }
    },
  };
</script>