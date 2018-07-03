# 核心API

## 一、业务常用API

### 1、KOS.use(middleware)

* **说明：** 新增middleware
* **参数：** 
 + **middleware：** redux中间件
* **返回值：** 无


### 2、KOS.start(App,Container='#main')

* **说明：** 启动应用，单页应用和多页应用均调用这个API
* **参数：** 
 + **App：** React.Component
 + **Container：** React.Component渲染到的DOM节点，默认是id为main的节点

* **说明：** 用于启动一个应用程序


### 3、KOS.Wrapper(config)(Component)

* **说明：** 将Component使用Wrapper组件进行包装后，挂在到connect下面，用户将Model和View进行包装
* **参数：**
 + **config.model：** 需要绑定的model，model说明详见 [Model](#)
 + **config.autoLoad：** 在执行到Wrapper的componentDidMount的时候，是否自动执行Model.setup方法
* **返回值：** 和Model结合之后的高阶组件
* **特别说明：**
在项目中，通常建议的使用方式是：

```js
import React from 'react';
import KOS form 'kos-core';
import model from './model';

@KOS.Wrapper({model})
const class App extends React.Component({
  render(){
    return <div></div>
  }
})
```


## 二、扩展API

以下API，主要是在扩展kos中间件的时候回使用，在日常业务逻辑中用到概率较低

### 1、KOS.registerModel(mode)

* **说明：** 注册model，KOS.Wrapper高阶组件会调用，同时也可以用来处理一些通用的业务逻辑
* **参数：** 
 + **model：** 需要注册的model
* **返回值：** 返回Model对象，注意：此处的Model对象非`KOS.Wrapper({model})`，时传入的model

### 2、KOS.getModel(namespace)

* **说明：** 根据namespace获取Model对象
* **参数：** 
 + **namepace：** 命名空间
* **返回值：** Model对象，详细见[Model说明](./model.html)


### 3、KOS.removeModel(namespace)

* **说明：** 根据namespace移除Model，谨慎调用
* **参数：** 
 + **namepace：** 命名空间
* **返回值：** 无


# 二、更多扩展

* 工具类[Util](./util.html)

