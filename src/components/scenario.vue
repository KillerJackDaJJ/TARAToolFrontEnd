<template>
  <div id="SCENARIO" ref="SCENARIO">
    <el-table :data="tableData" style="width: 100%" height="500" default-sort="label">
    <el-table-column fixed sortable prop="Scenario" label="危害场景" width="150"></el-table-column>
    <el-table-column sortable prop="Threat" label="威胁场景" width="150"></el-table-column>
    </el-table>
  </div>
</template>

<script>
  export default {
    name: 'SCENARIO',
    props:['tableData'],
    watch: {
        tableData(n){
            this.tableData=n
        }
    },
  };
</script>