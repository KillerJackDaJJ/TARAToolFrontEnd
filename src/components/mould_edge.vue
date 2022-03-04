<template>
  <div
    id="MouldEdge"
    ref="MouldEdge"
  >
    <el-tag type="info" class="mouldedge" mould="Ethernet" offset="100">【Ethernet】</el-tag><br>
    <el-tag type="info" class="mouldedge" mould="CAN FD" offset="100">【CAN FD】</el-tag><br>
    <el-tag type="info" class="mouldedge" mould="CAN" offset="100">【CAN】</el-tag><br>
    <el-tag type="info" class="mouldedge" mould="LIN" offset="100">【LIN】</el-tag><br>
    <el-tag type="info" class="mouldedge" mould="FlexRay" offset="100">【FlexRay】</el-tag><br>
  </div>
</template>

<script>
  export default {
    name: 'MouldEdge',
    mounted () {
      // 拖拽结束
      this.$refs.MouldEdge.addEventListener('click', e => {
        if(e.target.classList.contains('mouldedge')) {
          this.$emit('canvas-add-edge', e);
        }
      });
    },
  };
</script>