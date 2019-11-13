<template>
  <div class="panel-cascader" v-click-outside="handleClose" :style="{'width': width+'px','max-width': width+'px'}">
    <div class="cascader-input">
      <span @click="toggleOpen">
          <input
            id="input"
            :readonly="true"
            :value="displayRender"
            :placeholder="placeholder"/>
        </span>
      <i v-if="oldSelected.length>0" class="icon iconfont icon-clear" @click="clear">&#xe619;</i>
      <span v-else>
        <i v-if="!visible" class="icon iconfont icon-arrow" @click="toggleOpen">&#xe615;</i>
        <i v-if="visible" class="icon iconfont icon-arrow" @click="toggleOpen">&#xe616;</i>
      </span>
    </div>
    <div v-if="visible" class="select-dropdown">
      <div v-if="hasBack" class="back" @click="goBack"><i class="icon iconfont fold-icon">&#xe75b;</i>{{backText}}</div>
      <div class="cascader-menu" :style="{'width': width+'px','max-width': width+'px'}">
        <div v-for="item in currentData" :key="item.id" @click="onSelect(item)" class="menu-item" :class="{'active':item.id===active.id}">
          <div class="name" :style="{'width': width-20+'px','max-width': width-20+'px'}">{{item.name}}</div>
          <i v-if="item.children.length>0" class="icon iconfont fold-icon">&#xe628;</i>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
  import { directive as clickOutside } from "v-click-outside-x";
  import TreeUtils from '@dadcici/tree-utils';

  export default {
    name: 'Cascader',
    directives: {
      clickOutside
    },
    props: {
      data: {
        type: Array,
        default () {
          return [];
        }
      },
      value: {
        type: Array,
        default () {
          return [];
        }
      },
      placeholder: {
        type: String
      },
      width: {
        type: Number,
        default () {
          return 260;
        }
      },
      backText: {
        type: String,
        default () {
          return '返回'
        }
      }
    },
    data() {
      return {
        visible: false, // 下拉框可见
        oldSelected: [], // 原值
        newSelected: [], // 新值
        currentData: this.data // 可选项
      };
    },
    computed: {
      hasBack() {
        return this.currentData[0].pid > 0;
      },
      displayRender() {
        let label = [];
        let len = this.oldSelected.length;
        if (len > 0) {
          if (this.oldSelected[len - 1].childrenCount > 0) {
            return '';
          }
          label = this.oldSelected.map(item => { return item.name; });
          return label.join('/');
        }
        return '';
      },
      active() {
        return this.newSelected[this.newSelected.length - 1] || {};
      }
    },
    methods: {
      clear() {
        this.oldSelected = [];
        this.newSelected = [];
        this.currentData = this.data;
        this.visible = false;
        this.$emit('on-clear');
      },
      handleClose() {
        this.visible = false;
      },
      toggleOpen() {
        this.visible = !this.visible;
      },
      onSelect(item) {
        if (item.pid === this.active.pid) { // 同层级重选
          this.newSelected.splice(this.newSelected.length - 1, 1);
        }
        this.newSelected.push({ id: item.id, name: item.name, pid: item.pid, childrenCount: item.children.length });
        if (item.children && item.children.length > 0) {
          this.currentData = item.children;
        } else {
          this.visible = false;
          this.oldSelected = JSON.parse(JSON.stringify(this.newSelected));
          this.$emit('on-change', this.newSelected.map(sItem => { return sItem.id; }));
        }
      },
      goBack() {
        let len = this.newSelected.length;
        if (len === 1) {
          this.newSelected = [];
          this.currentData = this.data;
        } else if (len >= 2) {
          let tree = { id: 0, root: 'root', children: this.data };
          let pid = this.newSelected[len - 2].pid; // 祖父id
          this.newSelected.splice(len - 1, 1); // 干掉自已
          this.currentData = TreeUtils.getTreePNode(tree, pid).children; // 祖父的儿子们
        }
      },
      setSelected() {
        let list = [];
        if (this.data.length > 0 && this.value.length > 0) {
          let tree = { id: 0, root: 'root', children: this.data };
          this.value.forEach(id => {
            TreeUtils.traverse(tree, item => {
              if (item.id === id && item.id !== 0) { // 排除root
                list.push({ id: item.id, name: item.name, pid: item.pid, childrenCount: item.children.length });
              }
            });
          });
          this.newSelected = list;
          this.oldSelected = JSON.parse(JSON.stringify(list));
          this.currentData = TreeUtils.getTreePNode(tree, this.active.pid).children;
        } else {
          this.currentData = this.data;
        }
      }
    },
    watch: {
      data(n, o) {
        if (n !== o) {
          this.setSelected();
        }
      },
      value(n, o) {
        if (n !== o) {
          this.setSelected();
        }
      }
    }
  };
</script>

<style scoped lang="scss">
  .panel-cascader {
    position: relative;
    .cascader-input {
      height: 32px;
      input {
        box-sizing: border-box;
        -webkit-box-sizing: border-box;
        -moz-box-sizing: border-box;
        display: inline-block;
        width: 100%;
        height: 32px;
        padding: 4px 7px;
        line-height: normal;
        font-size: 12px;
        border: 1px solid #d7dde4;
        border-radius: 4px;
        color: #657180;
        background-color: #fff;
        position: relative;
        cursor: pointer;
        padding-right: 30px;
        &:hover {
          border-color: #57a3f3;
        }
        &:focus {
          outline: 0;
          box-shadow: 0 0 0 2px rgba(45,140,240,.2);
        }
      }
      .icon-clear {
        position: absolute;
        top: 50%;
        right: 8px;
        line-height: 1;
        margin-top: -7px;
        font-size: 14px;
        color: #9ea7b4;
        transition: all .2s ease-in-out;
        z-index: 1000;
        &:hover {
          cursor: pointer;
          color: #777;
        }
      }
      .icon-arrow {
        position: absolute;
        top: 50%;
        right: 8px;
        line-height: 1;
        margin-top: -7px;
        font-size: 14px;
        color: #9ea7b4;
        transition: all .2s ease-in-out;
      }
    }
    .select-dropdown {
      margin: 5px 0;
      padding: 5px 0;
      background-color: #fff;
      box-sizing: border-box;
      border-radius: 4px;
      box-shadow: 0 1px 6px rgba(0,0,0,.2);
      position: absolute;
      z-index: 900;
      font-size: 12px!important;
      color: #657180;
      .back {
        padding: 10px;
        border-bottom: 1px solid #eee;
        &:hover {
          background-color: #eee;
          cursor: pointer;
        }
        .fold-icon {
          color: #aaa;
        }
      }
      .cascader-menu {
        display: inline-block;
        min-width: 100px;
        height: 180px;
        margin: 0;
        padding: 5px 0!important;
        vertical-align: top;
        border-right: 1px solid #e3e8ee;
        overflow: auto;
        .menu-item {
          line-height: normal;
          padding: 10px;
          clear: both;
          display: flex;
          &:hover {
            background-color: #eee;
            cursor: pointer;
          }
          .name {
            white-space: nowrap;
            text-overflow: ellipsis;
            overflow: hidden;
          }
          .fold-icon {
            color: #aaa;
            position: relative;
          }
          &.active {
            background-color: #eee;
            color: #2d8cf0!important;
            .fold-icon {
              color: #2d8cf0!important;
            }
          }
        }
      }
    }
  }
</style>
