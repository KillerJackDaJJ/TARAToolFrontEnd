<template>
  <div
    id="DetailPanel"
    ref="DetailPanel"
  >
    <div v-if="type=='node-selected'">
      <el-form-item>{{node.label}}</el-form-item>     
      <el-checkbox v-model="node.S">欺骗</el-checkbox><br>
      <el-checkbox v-model="node.T">篡改</el-checkbox><br>
      <el-checkbox v-model="node.R">抵赖</el-checkbox><br>
      <el-checkbox v-model="node.I">信息泄露</el-checkbox><br>
      <el-checkbox v-model="node.D">拒绝服务</el-checkbox><br>
      <el-checkbox v-model="node.E">提权</el-checkbox><br>
    </div>
    <div v-else-if="type=='canvas-selected'">
    </div>
    <div v-else-if="type=='edge-selected'">
       <el-form-item>{{edge.label}}</el-form-item>  
      <el-checkbox v-model="edge.S">欺骗</el-checkbox><br>
      <el-checkbox v-model="edge.T">篡改</el-checkbox><br>
      <el-checkbox v-model="edge.R">抵赖</el-checkbox><br>
      <el-checkbox v-model="edge.I">信息泄露</el-checkbox><br>
      <el-checkbox v-model="edge.D">拒绝服务</el-checkbox><br>
      <el-checkbox v-model="edge.E">提权</el-checkbox>
    </div>
  </div>
</template>

<script>
  export default {
    name: 'DetailPanel',
    props:['type','node','edge'],
    watch: {
      type(n) {
        this.type=n
      },
      node(n){
        this.node=n
      },
      edge(n){
        this.edge=n
      }
    },
    methods: {
        addr(val){
            this.$emit('detail-panel', val);
        }
    },
  };
</script>