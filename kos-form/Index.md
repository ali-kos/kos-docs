# kos-form

kos-form是一款基于kos中间件机制，扩展出来的中间件，提供表单的校验、提交、字段操作能力


```js
import {Form,FieldHOC,formMiddleware} from 'kos-form';
```

包含如下三部分内容：

### Form

* 说明：表单组件
* 类型：React.Component
* proptypes:
  + name：必填，表单的名称
  + onSubmit：表单校验规则执行通过之后，回调的处理方法

详细见：[Form](./form.html)

### FieldHOC

* 说明：表单字段的高阶组件，kos-form要考虑通用性，所以不依赖antd，我们后续基于kos-form和antd，提供kos-form-antd的包
* 类型：高阶组件
* 返回值：
  Field：表单的字段组件
  Field.props：
  + field：表单名称，必填
  
详细见：[FieldHOC](./fieldhoc.html)



### formMiddleware

* 说明：表单处理中间件，表单的校验、数据的双向绑定、字段的展示隐藏均依赖这个中间件
* 示例：

```js
import kos from 'kos-core';
import {formMiddleware} from 'kos-form';

kos.use(formMiddleware);

kos.start(Component);
```

> 注意：kos.start必须在kos.use中间件之后运行

更多信息见：[Middleware](./formmiddleware.html)


### addRule

* 说明：添加表单校验规则
* 参数：
  + name：校验规则名称
  + fn(getState,{field,value,formName},data)：检验规则函数体，函数体
    - getState：获取当前的state数据
    - {field,value,formName}：字段名、字段值、表单名
    - data：配置的参数
  + help：校验规则的错误消息
    + success：校验正确
    + error：校验错误
    + warring：校验提示
    + validating：校验中
* 返回值：无
* 代码示例：

```js
addRule('maxLength', (getState, { value, field }, data) => {
  const maxLength=data[0];
  if (value) {
    return value.length <= maxLength;
  }
  return true;
});
```


### removeRule

* 说明：移除校验规则
* 参数：
  + name：校验器名称
* 返回值：无


