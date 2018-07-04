# Validator

Validator主要配置在Mode中，形式如下：

```js
import fetch from 'lib/fetch';
import { notification } from 'antd';

const model = {
  namespace: 'page-form-1',
  initial: {
    addForm: {
      page_name: '1234',
      page_desc: 'aaaa'
    }
  },
  validators: [{
    formName: 'addForm',
    validators: [{
      field: 'page_type',
      rules: 'required'
    }, {
      field: 'page_desc',
      rules: ['required', (getState, { field, value }, data) => {
        return value && value.length <= 3;
      }],
      help: '请正确填写'
    }, {
      field: 'page_name',
      rules: ['required@好好填', {
        name: 'maxLength',
        data: 4,
        help: 'maxLength:{0}'
      }]
    }, {
      field: 'page_name',
      help: '异步校验失败',
      rules: {
        data: { a: 1 },
        fn: async (getState, { field, value }, data) => {
          const xdata = await fetch({
            url: '/app/list',
            data: {
            }
          });

          return true
        }
      }
    }]
  }]
}

export default model;

```


## validators的配置

* 类型：数组
* validator.formName：表单名
* validator.validators：校验器，单个校验器配置参见下文解释


## 单个校验器配置

* field：字段名，与表单的field控件中的field属性对应
* rules：可以是单个校验规则，也可以是多个校验规则的数组，例如
  + `rules:'reuired'`
  + `rules:['reuired','email']`

## rule的配置形态

### 标准写法

例如：
```js
{
  name:'required',
  help:{
    error:'必填！'
  }
}
```

### 字符串

`${express}:${data}@${help.error}`

例如：
```js
{
  rules:'range:2,3@长度介于{0}到{1}'
}
```

转换成标准的rule配置：

```js
{
  name:'range',
  data:[2,3],
  help:'长度介于{0}到{1}'
}
```

### 正则类型

`/d+/g`

例如：
```js
{
  rules:/d+/g
}
```

转换成标准rule配置：
```js
{
  name:'regexp',
  data:/d+/g
}
```

> 注意：单个正则的配置方式，不能传入错误消息提示，所以建议使用标准写法


### function类型

`function(getState,{field,value,formName},data){return false}`


例如：

```js
{
  rules:(getState,{field,value,formName},data)=>{
    return false;
  }
}
```

相当于

```js
{
  fn:(getState,{field,value,formName},data)=>{
    return false;
  }
}
```

> 注意：单个正则的配置方式，不能传入错误消息提示，但是可以通过fn的返回值来进行错误消息设置，所以建议使用标准写法


