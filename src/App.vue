<template>
  <div class="page">
    <Cascader :data="catalogs" v-model="catalogIds" @on-change="onChange" @on-clear="onClear"></Cascader>
  </div>
</template>

<script>
  import TreeUtils from '@dadcici/tree-utils';
  import Cascader from './components/cascader';

export default {
  name: 'app',
  components: {
    Cascader
  },
  data () {
    return {
      tree: {},
      catalogIds: [],
      catalogs: [{
      id: 1,
      visible: true,
      name: 'name1',
      layer: 1,
      isOpen: false,
      children: [],
    }, {
      id: 2,
      visible: true,
      name: '8 level name2',
      layer: 1,
      isOpen: false,
      children: [{
        id: 21,
        visible: true,
        name: '8 level name2-1',
        layer: 2,
        isOpen: false,
        children: [{
          id: 211,
          visible: true,
          name: '8 level name2-1-1',
          layer: 3,
          isOpen: false,
          children: []
        }, {
          id: 212,
          visible: true,
          name: '8 level name2-1-2',
          layer: 3,
          isOpen: false,
          children: [ {
            id: 2121,
            visible: true,
            name: '8 level name2-1-2-1',
            layer: 4,
            isOpen: false,
            children: [{
              id: 21211,
              visible: true,
              name: '8 level name2-1-2-1-1',
              layer: 5,
              isOpen: false,
              children: [{
                id: 212111,
                visible: true,
                name: '8 level name2-1-2-1-1-1',
                layer: 6,
                isOpen: false,
                children: [{
                  id: 2121111,
                  visible: true,
                  name: '8 level name2-1-2-1-1-1-1',
                  layer: 7,
                  isOpen: false,
                  children: [{
                    id: 21211111,
                    visible: true,
                    name: '8 level name2-1-2-1-1-1-1-1',
                    layer: 8,
                    isOpen: false,
                    type: 'catalog',
                    children: []
                  }]
                }]
              }]
            }, {
              id: 21212,
              visible: true,
              name: '8 level name2-1-2-1-2',
              layer: 5,
              isOpen: false,
              children: []
            }]
          }]
        },  {
          id: 213,
          visible: true,
          name: 'name2-1-3',
          layer: 3,
          isOpen: false,
          children: []
        }]
      }, {
        id: 22,
        visible: true,
        name: 'name2-2',
        layer: 2,
        isOpen: false,
        children: []
      }, {
        id: 23,
        visible: true,
        name: 'name2-3',
        layer: 2,
        isOpen: false,
        children: []
      }],
    }, {
      id: 3,
      visible: true,
      name: 'name3',
      layer: 1,
      isOpen: false,
      children: [],
    }, {
      id: 4,
      visible: true,
      name: 'longest name，I am so long, yes, I am longest on the world.ah',
      layer: 1,
      isOpen: false,
      children: [{
        id: 41,
        visible: true,
        name: 'name4-1',
        layer: 2,
        isOpen: false,
        children: [{
          id: 411,
          visible: true,
          name: 'name4-1-1',
          layer: 3,
          isOpen: false,
          children: [],
        }],
      }],
    }, {
      id: 5,
      visible: true,
      name: 'name5',
      layer: 1,
      isOpen: false,
      children: [],
    }],
    }
  },
  methods: {
    onChange(ids) {
      this.catalogIds = ids;
    },
    onClear() {
      this.catalogIds = [];
    }
  },
  created() {
    this.tree = { id: 0, root: 'root', children: this.catalogs };
    // 挂children和pid、isOpen
    TreeUtils.resetTree(this.tree, 'children', 0, 0);
    // 处理当前选中
    this.catalogIds = TreeUtils.getTreeChain(TreeUtils.tree2List(this.tree), 21212, 'id');
  },
}
</script>

<style lang="scss">
.page {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
  margin-top: 60px;
  ul {
    li {
      list-style: none;
      padding: 0;
      margin: 0;
    }
  }
}
</style>
