<template>
  <div id="Assets" ref="Assets">
    <el-table :data="tableData" style="width: 100%" height="500" default-sort="label">
    <el-table-column fixed sortable prop="label" label="组件" width="150"></el-table-column>
    <el-table-column sortable prop="Func_" label="功能" width="150"></el-table-column>
    <el-table-column sortable prop="Asset_" label="资产" width="150"></el-table-column>
    <el-table-column label="安全属性" align="center">
      <el-table-column sortable prop="S" label="欺骗" width="60" align="center"></el-table-column>
      <el-table-column sortable prop="T" label="篡改" width="60" align="center"></el-table-column>
      <el-table-column sortable prop="R" label="抵赖" width="60" align="center"></el-table-column>
      <el-table-column sortable prop="I" label="信息泄露" width="60" align="center"></el-table-column>
      <el-table-column sortable prop="D" label="拒绝服务" width="60" align="center"></el-table-column>
      <el-table-column sortable prop="E" label="提权" width="60" align="center"></el-table-column>
    </el-table-column>
    <el-table-column sortable prop="Scenario" label="危害场景" width="150">
    </el-table-column>
    </el-table>

  </div>
</template>

<script>
  export default {
    name: 'Assets',
    props:['tableData'],
    watch: {
        tableData(n){
            this.tableData=n
        }
    },
  };
</script>