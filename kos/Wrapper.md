# Wrapper(config)(Component)

Wrapper是一个高阶函数，做成二阶函数的原因是为了配合ES7的新特性decorator来使用，高阶函数执行完之后，返回一个高阶组件，Component作为WrapperComponent的子组件

## 一、用法示例


### 1，作为decorator使用

```js
import KOS from 'kos-core';

import model from './model';

@KOS.Wrapper({model,autoLoad:false})
class View extends React.Component{
  render(){
    return '<div></div>'
  }
}

export default View
```

> decorator是一个ES7的特性，依赖编译环境

### 2、作为高阶方法使用

```js
import KOS from 'kos-core';

import model from './model';

const View=class extends React.Component{
  render(){
    return '<div></div>'
  }
}

export default @KOS.Wrapper({model,autoLoad:false})(View);
```



## 二、config配置项

### 1. config.model

* 配置类型：Object<Model>
* 默认值：无，
* 是否必填：是
* 配置说明：Model为Store提供当前namespace空间的默认数据，并提供绑定该Model的一切操作行为，例如：同步action响应、异步action响应、表单校验、表单字段展示隐藏控制、组件初始化数据加载能力等；
* 配置参考：[详见Model](./model.html)

### 2. config.autoLoad

* 配置类型：Boolean
* 默认值：true
* 是否必填：否
* 说明：用于标志在WrapperComponent高阶组件在`componentDidMount`生命周期阶段，初始化数据时，是否会触发`setup`的`action`；setup的action响应为Model.setup，用于加载组件的初始化数据


### 3. config.namesapce

* 配置类型：String
* 默认值：无
* 是否必填：否
* 说明：kos.Wrapper将根据这个传入的config.namespce，和config.model，注册一个新的model



## 三、Component

Component为一个React.Component类，WrapperComponent将吧Component作为子组件来渲染，同时会进行props的注入


### 1. Component的props注入

```js
const {dispatch,getParam,getNamespace} = this.props;
```

props同样会注入`store.getState()[namespace]`下的所有数据

#### 1.1 dispatch(action)

* 类型：function
* 参数：
  + action.type：redux的action.type的概念
  + action.payload：这是一个建议传入属性，将所有的参数通过payload来传入
* 说明：这是一个被包装后的dispatch方法，dispatch方法会根据action.type是否包含'/'来判定，是否需要默认加上namespace


#### 1.2 getParam()

* 类型：function
* 参数：无
* 说明：返回this.props.math，hashUrl，location.href的全量参数

Router配置的地址
```
/pages/edit/:id，
```

实际地址
```
/pages/edit/1234?name=abc#set?type=1
```

获取的数据
```
{
  id:1,
  name:'abc',
  type:1
}
```
#### 1.3 getNamespace()

* 类型：无
* 返回值：当前View绑定的Model指定的命名空间


### 2. Component的childContext


#### 2.1 dispatch(action)

* 类型：function
* 参数：
  + action.type：redux的action.type的概念
  + action.payload：这是一个建议传入属性，将所有的参数通过payload来传入
* 说明：这是一个被包装后的dispatch方法，dispatch方法会根据action.type是否包含'/'来判定，是否需要默认加上namespace


#### 2.1 getState()

* 类型：function
* 参数：无
* 说明：用于获取当前WrapperComponnet的props

#### 2.3 getNamespace()

* 类型：无
* 返回值：当前View绑定的Model指定的命名空间

