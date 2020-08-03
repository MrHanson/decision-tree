<template>
  <div class="decision-tree">
    <div class="operation">
      <div class="operation-row">
        <div class="operation-row-item" v-for="tag in tagList" :key="tag.label">
          {{ tag.label }}
        </div>
      </div>
      <div class="operation-row">
        <div class="operation-row-item" v-for="sign in signList" :key="sign.label">
          {{ sign.label }}
        </div>
      </div>
    </div>
    <mind-map ref="mindMap" class="mind-map" v-model="nodeData"></mind-map>
  </div>
</template>

<script>
import MindMap from './components/MindMap'
export default {
  components: { MindMap },
  data: () => ({
    tagList: [
      /** some tag */
    ],
    signList: [
      { label: '>', value: '>' },
      { label: '<', value: '<' },
      { label: '+', value: '+' },
      { label: '-', value: '-' },
      { label: 'in', value: 'in' }
    ],
    nodeData: [
      {
        name: '如何学习D3',
        children: [
          {
            name: '预备知识',
            children: [
              { name: 'HTML & CSS' },
              { name: 'JavaScript' },
              { name: 'DOM' },
              { name: 'SVG' },
              { name: 'test' }
            ]
          }
        ]
      }
    ]
  }),

  created() {
    new Array(5).fill().forEach((_, index) => {
      this.tagList.push({ label: index, value: index })
    })
  },

  methods: {
    addMindNode(val) {
      this.$refs['mindMap'].makeNodeAdd(val)
    }
  }
}
</script>

<style lang="less">
html,
body {
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
</style>

<style lang="less" scoped>
.decision-tree {
  width: 60%;
  margin: 0 auto;
  height: 100%;
  border: 1px solid #e8e8e8;
  border-radius: 8px;
  .operation {
    display: flex;
    flex-direction: column;
    &-row {
      display: flex;
      justify-content: space-around;
      padding: 8px;
      min-height: 32px;
      border: 1px solid #e8e8e8;
      &-item {
        line-height: 32px;
        position: relative;
        display: inline-block;
        font-weight: 400;
        white-space: nowrap;
        text-align: center;
        background-image: none;
        box-shadow: 0 2px 0 rgba(0, 0, 0, 0.015);
        cursor: pointer;
        transition: all 0.3s cubic-bezier(0.645, 0.045, 0.355, 1);
        -webkit-user-select: none;
        -moz-user-select: none;
        -ms-user-select: none;
        user-select: none;
        touch-action: manipulation;
        height: 32px;
        padding: 0 15px;
        font-size: 14px;
        border-radius: 4px;
        color: rgba(0, 0, 0, 0.65);
        background-color: #fff;
        border: 1px solid #d9d9d9;
        &:hover {
          color: #40a9ff;
          background-color: #fff;
          border-color: #40a9ff;
        }
        &:active {
          color: #096dd9;
          background-color: #fff;
          border-color: #096dd9;
        }
      }
    }
  }
  .mind-map {
    min-height: 400px;
  }
}
</style>
