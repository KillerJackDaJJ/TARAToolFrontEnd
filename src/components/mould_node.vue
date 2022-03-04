<template>
  <div
    id="MouldNode"
    ref="MouldNode"
  >
    <el-tag draggable="true" type="info" data-shape="circle" class="mouldnode" mould="T-Box">【T-Box】</el-tag><br>
    <el-tag draggable="true" type="info" data-shape="circle" class="mouldnode" mould="GW">【GW】</el-tag><br>
    <el-tag draggable="true" type="info" data-shape="circle" class="mouldnode" mould="IVI">【IVI】</el-tag><br>
  </div>
</template>

<script>
  export default {
    name: 'MouldNode',
    mounted () {
      // 拖拽结束
      this.$refs.MouldNode.addEventListener('dragend', e => {
        if(e.target.classList.contains('mouldnode')) {
          this.$emit('canvas-add-node', e);
        }
      });

      // 阻止默认动作
      document.addEventListener('drop', e => {
        e.preventDefault();
      }, false);
    },
  };
</script>
