# cascader

> A vue cascading component, without tier restrictions, is selected from a set of associated data sets, often used in provincial, corporate, transactional, directory-level, and so on. Compared to the Select component, you can complete the selection in one go and experience better.

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```

## Demo

<img src="https://cici-mistake-1251309346.cos.ap-chengdu.myqcloud.com/npm/cascader.png">


## Setup

`npm install @dadcici/cascader --save`

## Simple usage

```html
<Cascader :data="catalogs" v-model="catalogIds" @on-change="onChange" @on-clear="onClear"></Cascader>
```

```javascript
import TreeUtils from '@dadcici/tree-utils';
import Cascader from '@dadcici/cascader';

export default {
  components: {
    Cascader
  },
  data() {
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
      }]
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
    // create attribute of the node, example children pid and isOpen
    TreeUtils.resetTree(this.tree, 'children', 0, 0);
    // deal current selected node
    this.catalogIds = TreeUtils.getTreeChain(TreeUtils.tree2List(this.tree), 21212, 'id');
  },
};
```

## API

### Cascader props

| name     | description    | type     | default      |
|----------|----------------|----------|--------------|
| data | Optional data source, format reference example | Array | [] |
| value | Currently selected data, format reference example | Array | [] |
| placeholder | Input box placeholder | String | '' |
| width | nput box and drop-down box width | Number | 260 |
| backText | Return button title | String | '返回' |

### Cascader events

| name     | description    | return | return type
|---------------|----------------|----------|----------|
| on-change | After selecting the completed callback, the return value ids is the id of all nodes of the path where the selected value is located. | ids | Array |
| on-clear | Clear option value | - | - |

## License

Copyright (c) 2018 by www.dadcici.com
