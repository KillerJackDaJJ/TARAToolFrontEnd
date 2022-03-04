<template>
  <div id="TARA" ref="TARA">
    <el-table :data="tableData" style="width: 100%" height="500" default-sort="label">
    <el-table-column sortable prop="Threat" label="威胁场景" width="150"></el-table-column>
    <el-table-column sortable prop="Attack_Path" label="攻击路径" width="150"></el-table-column>
    <el-table-column sortable prop="Attack_Feasibility" label="攻击可行性等级" width="150"></el-table-column>
    <el-table-column sortable prop="Impact" label="影响等级" width="150"></el-table-column>
    <el-table-column sortable prop="Risk_Value" label="风险值" width="150"></el-table-column>
    <el-table-column sortable prop="Goal" label="安全目标" width="150"></el-table-column>
    </el-table>
  </div>
</template>

<script>
  export default {
    name: 'TARA',
    props:['tableData'],
    watch: {
        tableData(n){
            this.tableData=n
        }
    },
  };
</script>