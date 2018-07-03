# kos扩展

kos是一款基于redux的数据状态管理的轻框架，本身只提供数据流的状态管理能力，内置异步处理的能力，我们希望基于这样的状态管理和redux中间件能力，提供更多的可扩展，例如kos-form，kos-loading等

**kos的扩展参考：**

* [redux中间件开发](https://www.tuicool.com/articles/u6JRjyz)


**kos扩展的使用：**

```js
import kos from 'kos-core';
import {formMiddleware} from 'kos-form';

kos.use(formMiddleware);

class Component extends React.component{
  render(){
    return <div>中间件...</div>
  }
}

kos.start(Component);
```


**kos扩展可使用的API：**

* **kos.getModel(namespace)：** 根据namespace来获取Model，从而获取到指定的配置，例：:model.reduces，例如model.asyncs等；
* **kos.Util.getActionType(type)：** 该API将完把namespace和type进行拆分


> 注意：中间件必须要在kos.start之前注入
