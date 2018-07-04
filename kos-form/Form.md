# Form


Form是kos-form提供的表单组件，Field组件必须在Form的子组件来使用


```js
import {Form,FieldHoc,formMiddleware} from 'kos-form';
import {Form as AntDFrom,Input,Button} from 'antd';
import React from 'react';

const Field=FieldHoc({
  FieldWrapper:AntDForm.Item
});


const AddForm = class extends React.Component{
  submitForm(formData){
    dispatch({
      type:'saveForm',
      payload:{
        formData
      }
    });
  }
  render(){
    return <Form name="addForm" onSubmit={(formData)=>this.submitForm(formData)}>
      <Field field="name">
        <Input/>
      </Field>
      <Button htmlType="submit">提交</Button>
    </Form>
  }
}

kos.use(formMiddleware);

```


## props

### props.name

* 类型：String
* 是否必填：必填
* 说明：当前namesapce下的唯一的表单名


## props

### props.onSubmit

* 类型：function
* 是否必填：否
* 说明：表单的onSubmit方法，会事先进行表单校验，校验通过才调用该方法
* 入参：
  + formData：Object，当前表单的所有值内容


## 静态方法

### Form.getForm(namesapce,formName)

* 说明：根据namespace和formName，获取表单对象
* 参数：
  + namespace：String，当前组件的namespace
  + formName：Strign，表单名
* 返回值：表单对象

### Form.getFormData(namesapce,formName)

* 说明：根据namespace和formName，获取表单数据值
* 参数：
  + namespace：String，当前组件的namespace
  + formName：Strign，表单名
* 返回值：表单当前数据值


### Form.resetForm(namesapce,formName)

* 说明：重置表单的值，将Model.initial中的值重置到form中
* 参数：
  + namespace：String，当前组件的namespace
  + formName：Strign，表单名


## 对象API

### form.reset();

* 说明：重置表单的值，将Model.initial[formName]的值，覆盖当前表单的值
* 返回值：无


### form.getData()

* 说明：获取表单的值对象,key为field
* 返回值：表单的值对象，例如：{name:1,age:2}


### form.setData(data)

* 说明：回填表单数据
* 参数：
  + data：key/value形式的表单值对象


### form.validate(callback)

* 说明：执行表单校验
* 参数：
  + callback：执行校验完成后的回调方法
    - 参数：result，true为校验成功，false为校验失败
* 返回值：无
