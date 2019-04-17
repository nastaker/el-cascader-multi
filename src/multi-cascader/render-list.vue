<template>
  <ul>
    <li class="li-style"
      v-for="(node, nodeIndex) in list"
      :key="getKey(node)"
      :class="{'active-li': activeList[level - 1] === node.id}"
      @mouseenter="handleMouseEnter(node, nodeIndex, level)"
    >
      <el-checkbox @change="handleCheck($event, node)" v-model="node.checked" :disabled="node.disabled"></el-checkbox>
      <span @click="handleClick(node, nodeIndex, level)">
        {{node[labelKey]}}
        <i v-if="node.childNodes && node.childNodes.length > 0" class="li-label-icon el-icon-arrow-right"></i>
      </span>
    </li>
  </ul>
</template>

<script>
export default {
  props: {
    activeList: {
      type: Array,
      default: () => []
    },
    list: {
      type: Array,
      default: () => []
    },
    level: {
      type: [Number, String]
    },
    labelKey: {
      type: String,
      default: 'label'
    },
    expandTrigger: {
      type: String,
      default: 'click'
    }
  },
  methods: {
    handleMouseEnter (node, levelIndex, level) {
      if (this.expandTrigger === 'hover') {
        this.$emit('handle-click', node, levelIndex, level)
      }
    },
    handleClick (node, levelIndex, level) {
      if (this.expandTrigger === 'click') {
        this.$emit('handle-click', node, levelIndex, level)
      }
    },
    handleCheck (v, node) {
      node.checked = v
      this.$emit('handle-check', node)
    },
    getKey () {
      return (+new Date() + Math.random()).toString()
    }
  }
}
</script>

<style lang="scss">
  ul {
    padding: 0;
  }
  
  ul li.li-style {
    display: flex;
    width: 160px;
    height: 34px;
    line-height: 34px;
    list-style: none;
    white-space: nowrap;
    overflow: hidden;
    text-overflow: ellipsis; 
    &:hover{
      background-color: rgba(69,200,220,.1);
    }
    &.selected {
      color: #45c8dc;
    }
    &.active-li {
      background-color: rgba(69,200,220,.1);
      color: #45c8dc;
    }
  }
  ul li label.el-checkbox {
    margin-left: 10px;
    margin-right: 10px;
  }
</style>
