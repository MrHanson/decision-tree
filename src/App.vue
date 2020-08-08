<template>
  <div class="decision-tree">
    <div class="operation">
      <div class="operation-row">
        <div
          class="operation-row-item"
          v-for="tag in tagList"
          :key="tag.label"
          @click="onAddLabel(tag.value, 'tag')"
        >{{tag.label}}</div>
      </div>
      <div class="operation-row">
        <div
          class="operation-row-item"
          v-for="sign in signList"
          :key="sign.label"
          @click="onAddLabel(sign.value, 'sign')"
        >{{ sign.label }}</div>
      </div>
    </div>
    <mind-map ref="mindMap" class="mind-map" v-model="nodeData" @selected="onSelected"></mind-map>
  </div>
</template>

<script>
import MindMap from './components/MindMap'

function getTarget(idList = [], nodeData) {
  let tar
  if (!idList.length) {
    tar = nodeData[0]
    return tar
  }

  let rt = nodeData[0]
  idList.forEach(id => {
    if (!Array.isArray(rt.children)) return
    rt = tar = rt.children[+id]
  })
  return tar
}

export default {
  name: 'decision-tree',

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
        name: 'Root',
        children: [
          {
            name: 'Father'
          }
        ]
      }
    ],

    selectedId: ''
  }),

  methods: {
    addMindNode(val) {
      this.$refs['mindMap'].makeNodeAdd(val)
    },
    onSelected(id) {
      this.selectedId = id
    },
    onAddLabel(val) {
      if (!this.selectedId) return

      const idList = this.selectedId.split('-')
      // 根结点出队列
      idList.shift()

      const tar = getTarget(idList, this.nodeData)
      console.log(tar, val)
      
      const node = { name: val, children: [] }
      Array.isArray(tar.children) && tar.children.unshift(node)
      this.selectedId = ''
    }
  },

  created() {
    new Array(5).fill().forEach((_, index) => {
      this.tagList.push({ label: index, value: index })
    })
  }
}
</script>

<style lang="less">
html,
body {
  width: 100%;
  height: 100%;
  margin: 0;
  padding: 16px;
  box-sizing: border-box;
}
</style>

<style lang="less" scoped>
.decision-tree {
  width: 60%;
  margin: 0 auto;
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
