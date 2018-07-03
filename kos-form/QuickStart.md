# kos-form 快速上手


## 依赖安装

```
npm install kos-core --save;

npm install kos-form --save;
```

kos-from依赖kos，所以需要实现安装kos-core





## 代码示例


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



`form.js`

```js
import React from 'react';
import KOS from 'kos-core';
import model from './model'

import { Input, Tooltip, Icon, Cascader, Select, Row, Col, Radio, Button, AutoComplete } from 'antd';
import { Form, Field, ToolbarField } from './kos-form-antd';


const RadioButton = Radio.Button;
const RadioGroup = Radio.Group;


@KOS.Wrapper({ model })
class PageForm extends React.PureComponent {
  /**
   * 用于直接的事件绑定提交逻辑
   *
   * */
  handleSubmit() {
    const { dispatch } = this.props;
    const { getNamespace } = this.props;


    const result = Form.validate(getNamespace(), 'addForm', (result) => {
      result && dispatch({
        type: 'save'
      });
    });
  }
  submitForm(formData) {
    const { dispatch } = this.props;
    dispatch({
      type: 'save'
    });
  }
  render() {
    const { title, dispatch } = this.props;

    return <Form name="addForm" onSubmit={() => this.submitForm()}>
      <Field label="页面名称：" field="page_name">
        <Input onChange={() => { console.log('onChange') }} />
      </Field>
      <Field label="页面类型：" field="page_type">
        <RadioGroup>
          <RadioButton value="pc">PC</RadioButton>
          <RadioButton value="h5">H5</RadioButton>
        </RadioGroup>
      </Field>
      <Field label="说明：" field="page_desc">
        <Input.TextArea rows={4} />
      </Field>
      <ToolbarField>
        <Button type="primary" htmlType="submit">新增</Button>
      </ToolbarField>
    </Form>
  }
}

export default PageForm

```


`model.js`
```js
import fetch from 'lib/fetch';
import { notification } from 'antd';


const model = {
  namespace: 'page-form-1',
  initial: {
    addForm: {
      page_name: '1234'
    }
  },
  asyncs: {
    async save(dispatch, getState, { payload }) {
      const state = getState();
      const { addForm } = state;
      const { param } = state;

      const data = await fetch({
        url: '/page/add',
        data: {
          ...addForm,
          page_init_data: '',
          app_id: param.appId
        }
      });

      if (data.ok) {
        notification.success({
          message: '恭喜您！',
          description: '添加成功！'
        });
        history.push(`/page/list/${param.appId}`);
      }
    }
  },
  validators: [{
    formName: 'addForm',
    validators: [{
      field: 'page_name',
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
  }],
  formFieldDisplay: {
    'addForm': {
      'page_type': (getState, { field, value }) => {
        console.log(value);

        return {
          'page_desc': value === 'pc'
        }
      }
    }
  }
}

export default model;

```
