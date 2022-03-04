<template>
  <div id="app">
  <el-tabs v-model="activeName" type="card" @tab-click="handleClick">
    <el-tab-pane label="系统架构" name="first">
      <el-container style="height: 700px; border: 1px solid #eee">
        <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
          <el-menu default-active="1" :default-openeds='["1","1-1","1-2"]'>

            <el-submenu index="1">
              <template slot="title"><i class="el-icon-message"></i>组件</template>        
              <el-submenu index="1-1">
                <template slot="title">节点</template>
                <el-menu-item index="1-1"><MouldNode @canvas-add-node="canvasAddNode" /></el-menu-item>
                <br><br><br><br><br>
              </el-submenu>
              <el-submenu index="1-2">
                <template slot="title">数据流</template>
                <el-menu-item index="3-4-1"><MouldEdge @canvas-add-edge="canvasAddEdge" /></el-menu-item>
                <br><br><br><br><br>
              </el-submenu>
            </el-submenu>
          </el-menu>
        </el-aside>
        
        <el-container>
          <el-header style="text-align: left; font-size: 12px">
            <el-row>
              工具栏：  
              <el-button type="success" @click="deletestate">删除</el-button>
              <el-button type="success" @click="sendpic">提交数据流图</el-button>
              <el-button type="success" @click="report">生成报告</el-button>
            </el-row>
          </el-header>
          <el-main>
            <div id="graph" />
          </el-main>
        </el-container>

        <el-aside width="200px" style="background-color: rgb(238, 241, 246)">
          <span style="text-align: center;display:block;">属性</span><br><br>
          <DetailPanel @detail-panel="adddetailpanel" :type="select_type" :node="node_select" :edge="edge_select"></DetailPanel>
        </el-aside>

      </el-container>      
    </el-tab-pane>
    <el-tab-pane label="资产识别" name="second"><Assets :tableData="send_data"></Assets></el-tab-pane>
    <el-tab-pane label="威胁场景识别" name="third"><SCENARIO :tableData="Scenario_Threat"></SCENARIO></el-tab-pane>
    <el-tab-pane label="威胁分析和风险评估" name="fourth"><TARA :tableData="TARA"></TARA></el-tab-pane>
    <el-tab-pane label="缓解措施" name="fifth"><COUNTERMEASURE :tableData="COUNTERMEASURE"></COUNTERMEASURE></el-tab-pane>
  </el-tabs>  

  </div>
</template>

<script>
import G6 from '@antv/g6';
import MouldNode from './components/mould_node';
import MouldEdge from './components/mould_edge';
import DetailPanel from './components/detail_panel';
import Assets from './components/assets'
import SCENARIO from './components/scenario'
import TARA from './components/tara'
import COUNTERMEASURE from './components/countermeasure'
import axios from 'axios';
export default {
  name:'App',
  components: {
    MouldNode,
    MouldEdge,
    DetailPanel,
    Assets,
    SCENARIO,
    TARA,
    COUNTERMEASURE
  },
  data () {
    return {
      graph: null,
      canvasEdge:{
        n1: null,
        n2: null,
        label: null,
        flag: false,
      },
      canvasOffset: {
        x:  0,
        y:  0,
        dx: 0,
        dy: 0,
      },
      deleteflag: false,
      node_select:{},
      edge_select:{},
      tmp_object:{},
      send_data:[],
      Scenario_Threat:[],
      TARA:[],
      COUNTERMEASURE:[],
      select_type:"canvas-selected",
      activeName: 'first'
    };
  },
  mounted () {
    // 创建画布
    this.$nextTick(() => {
      this.createGraphic();
    });
  },
  beforeDestroy () {
    this.graph.destroy();
  },
  methods: {
    createGraphic () {
      const graph = new G6.Graph({
        container: document.getElementById('graph'),
        width:     window.innerWidth - 450,
        height:    window.innerHeight - 200,
        fitCenter: true,
        fitView:   true,
        modes:{
          default: ['drag-node','drag-edge'],
        },
        // 布局设置
        layout:    {
          type: 'dagre',
        },
        defaultNode:{
        },
        nodeStateStyles: {
          // 鼠标点击节点，即 click 状态为 true 时的样式
          click: {
            stroke:    'steelblue',
            lineWidth: 3,
          },
        },
        edgeStateStyles: {
          // 鼠标点击节点，即 click 状态为 true 时的样式
          click: {
            stroke:    'steelblue',
            lineWidth: 3,
          },
        },
      });

      this.graph = graph;

      this.graph.on('edge:click', e => {
        //单击高亮
        const nodes = this.graph.findAllByState('node', 'click');
        const edges = this.graph.findAllByState('edge','click');
        nodes.forEach(node => {
          this.graph.setItemState(node, 'click', false);
        });
        edges.forEach(edge => {
          this.graph.setItemState(edge, 'click', false);
        });        
        this.graph.setItemState(e.item, 'click', true); 
        this.edge_select=e.item.getModel()
        this.tmp_object=e.item
        this.select_type="edge-selected"
        //删除
        if(this.deleteflag==true){
          graph.remove(e.item);
        }
      });
      
      this.graph.on('node:click', e => {
        //单击高亮
        const nodes = this.graph.findAllByState('node', 'click');
        const edges = this.graph.findAllByState('edge','click');
        nodes.forEach(node => {
          this.graph.setItemState(node, 'click', false);
        });
        edges.forEach(edge => {
          this.graph.setItemState(edge, 'click', false);
        });
        this.graph.setItemState(e.item, 'click', true);
        this.node_select=e.item.getModel()
        this.tmp_object=e.item
        this.select_type="node-selected"

        //删除，连线
        if(this.deleteflag==true){
          graph.remove(e.item);
        }
        if (this.canvasEdge.flag==true) {
          if (this.canvasEdge.n1==null) {
            this.canvasEdge.n1=e.item;
          } else {
            this.canvasEdge.n2=e.item;
            this.graph.addItem('edge', {
              source: this.canvasEdge.n1,
              target: this.canvasEdge.n2,
              type: 'quadratic',
              style: {
                stroke: 'blue',
                endArrow:{path:G6.Arrow.triangleRect(5,15,5,3,5,),d: 0},
                lineWidth:1,
              },
              label: this.canvasEdge.label,
              curveOffset: this.canvasEdge.offset,
              Func_:'',
              Asset_:'',
              S: false,
              T: false,
              R: false,
              I: false,
              D: false,
              E: false,
              Scenario: '',
            });
            this.canvasEdge.n1=null;
            this.canvasEdge.n2=null;
            this.canvasEdge.flag=false;
          }
        }
      });

      this.graph.on('canvas:click', e => {
        console.log('node on click', e);
        const nodes = this.graph.findAllByState('node', 'click');
        const edges = this.graph.findAllByState('edge','click');
        nodes.forEach(node => {
          this.graph.setItemState(node, 'click', false);
        });
        edges.forEach(edge => {
          this.graph.setItemState(edge, 'click', false);
        });
        this.select_type="canvas-selected"
        this.tmp_object=null
      });
    },
    handleClick(tab, event) {
      console.log(tab, event);
    },
    canvasAddNode(e) {
      const shape = e.target.getAttribute('data-shape');
      const mould = e.target.getAttribute('mould');

      this.graph.addItem('node', {
        label: mould,
        type:  shape,
        x:     e.clientX - this.canvasOffset.dx - 225,
        y:     e.clientY - this.canvasOffset.dy - 140,
        size:  60,
        Func_:'',
        Asset_:'',
        S: false,
        T: false,
        R: false,
        I: false,
        D: false,
        E: false,
        Scenario: '',
        anchorPoints:[[0,0.5],[1,0.5],[0.5,1],[0.5,0]]
      });
    },
    canvasAddEdge(e) {
      this.canvasEdge.label = e.target.getAttribute('mould');
      this.canvasEdge.offset = e.target.getAttribute('offset');
      this.canvasEdge.n1=null;
      this.canvasEdge.n2=null;
      this.canvasEdge.flag=true;
    },
    deletestate(){
      this.deleteflag = this.deleteflag ^ 1;
      if(this.deleteflag == true) this.$message('删除模式开启');
      else this.$message('删除模式关闭');
    },
    sendpic(){
      var nodes=new Array();
      var edges=new Array();
      var that=this;   //axios中this不指向vue
      for(var i in this.graph.getNodes()){
        nodes.push(this.graph.getNodes()[i].getModel())
      }
      for(var j in this.graph.getEdges()){
        edges.push(this.graph.getEdges()[j].getModel())
      }
      axios.post('http://127.0.0.1:5000/test', {
        node:nodes,
        edge:edges
      })
      .then(function (response) {
        that.send_data.length=0 //清空数组
        console.log(response)
        for(var i in response.data.nodes){
          that.send_data.push(response.data.nodes[i])
        }
        for(var j in response.data.edges){
          that.send_data.push(response.data.edges[j])
        }
        for(var k in response.data.Scenario_Threat){
          that.Scenario_Threat.push(response.data.Scenario_Threat[k])
        }
        for(var l in response.data.tara){
          that.TARA.push(response.data.tara[l])
        }
        for(var m in response.data.countermeasure){
          that.COUNTERMEASURE.push(response.data.countermeasure[m])
        }
      })
      .catch(function (error) {
        console.log(error);
      });
      this.$message('数据流图提交成功');
    },
    report(){
      this.$message('功能待开发');
    },
    adddetailpanel(val){
      this.tmp_object.getModel()==val
    }
  },
};
</script>

<style>
  .el-header {
    background-color: #B3C0D1;
    color: #333;
    line-height: 60px;
  }
  
  .el-aside {
    color: #333;
  }
</style>