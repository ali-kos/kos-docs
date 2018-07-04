# Rule

表单校验规则，一个表单校验规则的完整形态是：

```js
{
  name:'rangeLength',
  data:[4,10],
  help:{
    success:'校验成功{0}',
    error:'校验失败{1}',
    warring:'注意事项',
    validating:'校验中'
  },
  fn:(getState,{field,value,formName},data)=>{

  }
}
```


## 一、属性说明：

### name

* 类型：String，
* 说明：校验规则名称


### data

* 类型：any
* 说明：传入给校验器的参数，校验器将根据这个参数来进行校验，多个参数使用数组来实现，例如rangeLength校验器的参数是一个数组，第一个表示最小长度，第二个表示最大长度

### help

* 类型：String|Object
* 说明：可以通过添加{0}这样的占位符来使用data中配置的参数，{0}表示data数组的第一个元素，以此类推
  + 类型为String：表示错误信息
  + 类型为Object：完整的四个校验中状态的信息配置，没有配置就表示默认

### fn

* 类型：function
* 说明：校验器，所有的校验器最终都是一个function，默认的校验器是内置了这个fn，这个方法可以是一个同步的，也可以是一个异步的，kos推荐编程模式是async/await；所以异步的fn写成

```js
{
  fn:async (getState,{field,value,formName},data)=>{
    // ...发送异步请求

    return false;
  }
}
```

* 返回值：String|Boolean
  + 返回值为String：表示校验失败，并且错误提示消息是返回的字符串
  + 返回值是Boolean：表示校验成功或者失败，true表示成功，false表示失败


### 属性配置优先级

`name` > `fn` ：即当配置了name属性时，会根据name去拿默认的校验器，此时`fn`配置无效，所以`name` 配置和`fn` 配置是互斥的


## 二、内置的rule

详见[内置校验规则](./defaultrule.html)
