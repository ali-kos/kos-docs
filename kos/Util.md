# KOS.Util

这个类主要提供了一下在基于KOS开发时需要用到的常用API，业务开发时，请谨慎调用

## 一、基本API

**1、Util.wrapperDispatch(dispach,namespace)(action)**

* **说明：** 这是一个高阶函数，根据第一阶函数dispatch和namespace，执行第二阶的action的dispatch
* **参数：**
 * dispatch：store.dispatch方法
 * namespace：命名空间名称
* **返回值：** action

**2、Util.getParam()**

* **说明：** 获取参数，包括url后面的query和hashUrl后面的query，不包括路由的match
* **参数：** 无
* **返回值：** key:value形式的参数对象，例如`{id:1}`

**3、Util.getActionType(actionType)**

* **说明：** 拆分actionType为namespace和type
* **参数：**
 + actionType：包含namepsace的type，例如：'page-add/save'
* **返回值：** 返回namespace和type组成的对象，例如上面的将返回 

`{namespace:'page-add',type:'save'}`
