# FieldHOC({FieldWrapper, FieldProps})

FieldHOC是一个高阶组件，为了通用性考虑，不依赖于antd，项目实际使用时，可以自己根据这个FieldHOC，组装自己的Field组件；


**示例：**

`kos-form-antd.js`

```js
import * as KosForm from 'kos-form';
import { Form as AntDForm } from 'antd';


const formItemLayout = {
  labelCol: {
    xs: { span: 24 },
    sm: { span: 8 },
  },
  wrapperCol: {
    xs: { span: 24 },
    sm: { span: 16 },
  },
};

const tailFormItemLayout = {
  wrapperCol: {
    xs: {
      span: 24,
      offset: 0,
    },
    sm: {
      span: 16,
      offset: 8,
    },
  },
};

export const Field = KosForm.FieldHOC({
  FieldWrapper: AntDForm.Item,
  FieldProps: formItemLayout
});

export const ToolbarField = KosForm.FieldHOC({
  FieldWrapper: AntDForm.Item,
  FieldProps: tailFormItemLayout
});

export const Form = KosForm.Form;
```


