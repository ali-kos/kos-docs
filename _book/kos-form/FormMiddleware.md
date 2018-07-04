# formMiddleware

formMiddleware是表单中间件的核心，表单的字段校验、表单的字段展示隐藏、表单的字段值都是通过该中间件来完成的


**使用示例：**

```js
import kos from 'kos-core';
import {formMiddleware} from 'kos-form';


kos.use(formMiddleware);

kos.start(Compoent,'#main');

```


在kos.start之前，调用kos.use来完成middleware的注入；
