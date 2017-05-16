# tree-select

> A tree-select component based on vue2.X


## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:8080
npm run dev

# build for production with minification
npm run build
```

## 介绍
因公司项目需求，需要做一个树形的部门选择组件。起先是用的element-ui中的tree组件+el-input组合成的一个tree-select组件，后来发现当数据量较大的时候（比如：5000部门数据）这个组合起来的组件实在是卡，每次点击都会等个0.5s左右才反应过来，而且筛选的时候也并不是很快能，删除筛选后所有的节点全部展开了，交互并不是很好。

最后折腾来折腾去，还是决定按照element-ui中tree组件的原型，重新自己写过，发现效果还行，不卡了，也不迟钝了。分析原因可能是element-ui中组合了太多的功能，使得el-tree有点笨重了。所以我这里只使用自己需要的功能，会相对的性能好些。

这里只实现了单选的功能，后期有多选的需求再补充。

## 代码部分

    <tree-select
          ref="tree-select"
          :treeData="treeData"
          :treeProps="treeProps"
          v-model="treeSelected"
          placeholder="请选择部门"
          @setSelectedId="setSelectedId">
      </tree-select>

### 属性
	treeData：树节点数据，类型：Array[Object]
	treeProps:{//配置项
        label: "name", //显示的节点名称
        children: "childDepts",//子级节点属性
        level:"deptLevel" //节点级数
    }

### 事件
	setSelectedId：节点被点击时的回调，参数是节点的id，也可以在tree.vue的handleNodeClick方法中设置返回节点本身：
	this.$emit('setSelectedId',node.id) ==>
	this.$emit('setSelectedId',node);
	看个人的需要了。

### 节点数据属性要求
本着后端返回的数据量越少越小越好越快的想法，项目中就只让后端返回了下面四个属性。

	{
		id:1,
		label:'测试',
		level:1,
		children:[]子节点
	}
而实际上节点中的node属性是被扩展了的，通过./utils/tools.js中的objArrDeepCopy()方法将源数据进行了深度拷贝，并扩展了以下属性：

	{
		visible:true,//用于设置搜索时节点的显示隐藏
		expanded:true//用于设置节点的展开与收起
	}

### 操作效果图

![Markdown](http://i2.muimg.com/1949/46cd392b73c2a92d.gif)