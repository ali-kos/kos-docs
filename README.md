# KOS

## 背景

口碑业务快速发展，前端技术选型也同阿里集团和蚂蚁集团一样，选择了React，业务快速迭代发展，业务环境也更加的复杂，开发人群也是非常的多，遇到了如下的问题：

* 缺乏提供统一的基于React的最佳开发实践
* PC端和移动端的开发场景存在差异
* 开发人员群体复杂，有ISV，有外包，还有口碑的前端

基于如上的背景情况，决定引入数据流管理的框架，来解决，希望籍此来构建口碑前端基于React的研发生态

## 定位

我们对于数据流管理框架的要求是：

* 轻量：因为要覆盖移动端的数据流管理场景，要求框架本身要足够的轻量
* 业务低耦合：因为要解决多种多样的业务场景问题，要求框架本身无业务含义
* 简单易用：因为要面对不同能力背景的用户群体，要求足够的简单易用，概念少

集团和市面上有足够多的数据流管理框架，类似dva、tarot等，但是我们还是决定自己来完成数据流管理框架的核心能力，并期望后续能够有更丰富的生态来提供更充足的能力

而提供丰富生态的基础是redux的中间件

## KOS基本概念

### 技术依赖

* **Redux：** [详见文档](http://cn.redux.js.org/index.html)
* **React-Redu：** [详见文档](http://www.cnblogs.com/hhhyaaon/p/5863408.html)

### 基本概念

* **Model：** Model是KOS的一个核心概念，Model承载View的数据提供、各类数据操作逻辑，扩展中间件时，需要使用到的基础配置，也将在Model上承载，详见[Model]('./model.html)

* **View：** 经过kos.Wrapper包装后返回的组件，称之为view，本质上也是一个React.Component，详见[View](./View.html)

* **Wrapper：** 高阶函数，第一级别参数为config，第二级参数为一个React.Component包裹器，返回一个高阶组件将Model和View做了糅合，详见：[Middleware]('./Middleware.html')

* **Middleware：** Redux的中间件，详见[Middle]('./Middleware.html')，kos基于redux的中间件来扩展自己的能力

* **namespace：** 命名空间，View和Model要整合，namespace是必须的，
  
  namespace的几个用处：

  + 用于在store.state中开辟数据存储空间；
  + 用于标志具体的action将由那个Model来处理；

  namespace的配置方式：
  
  + 通过Model.namesapce配置
  + 通过config.namespace配置
  + 通过View的props传入，`<View namespace="abc"/>` 

  优先级：

  `View.props.namespace` > `config.namespace` > `Model.namespace`

