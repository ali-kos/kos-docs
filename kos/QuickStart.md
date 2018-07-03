# KOS快速上手
---

## 前期准备：

* 1、kos依赖redux,react-redux和react-redux-dom；
* 2、decorator受限编译环境，推荐使用webpack
* 3、decorator的替代模式是直接调用KOS.Wrapper这个API

> 建议使用官方的demo，来进行试验，避免环境的准备

## 代码示例

**1、安装kos**

```
npm install kos-core
```

**2、编写Model文件**

`model.js`
```js
const Model={
  namespace:'kos-test',
  // state的初始化数据
  initial:{
    counter:0,
    title:'KOS-Test'
  },
  // 用于处理数据变更的同步的action
  reducers:{
    setTitle(state,action){
      const {title} = action.payload;

      return {
        ...state,
        title
      };
    }
  },
  // 用于处理异步的action
  asyncs:{
    async loadInitData(dispatch,getState,action){
      ///...
      const state = getState();
      setTimeout(()=>{
        dispatch({
          type:'setState',
          payload:{
            count:state.count++
          }
        });
      },1000)
    }
  },
  // 页面初始化会调用
  async setup(dispatch,getState,action){

  }
}

export default Model;
```

**3、编写View文件**

`view.js`

```js
import React from 'react';
import KOS from 'kos-core';

import model from './model';

@KOS.Wrapper({model})
const View = class extends React.Component{
  setTitle(){
    const {dispatch}=this.props;

    dispatch({
      type:'setTitle',
      payload:{
        title:'新的title'
      }
    })；
  }
  loadCount(){
    const {dispatch}=this.props;

    dispatch({
      type:'loadCount',
      payload:{}
    })
  }
  render(){
    const {title,count}=this.props;

    return (<div>
      <h1>Title：{title}</h1>
      <div>Count：{count}</h1>
      <div>
        <button onClick={()=>{this.setTitle()}}>setTitle</button>
        <button onClick={()=>{this.setCount()}}>setCount</button>
      </div>
    </div>)

  }
}

export default View
```

至此，一个基于kos封装的Component已经提供完毕，可以直接使用；

> 注意：如果您的编译环境没有decorator的能力，可以直接使用，也可以使用如下方式来完成

```js
import React from 'react';
import KOS from 'kos-core';
import model from './model';
const View=class extends React.COmponent{
  render(){
    return (...);
  }
}

export default KOS.Wrapper({model})(View);
```


**4、编写出口文件**

`index.js`
```js
import React from 'react';
import KOS from 'kos-core';
import View from './view';

KOS.start(View,'#main');
```

代码简洁清爽
