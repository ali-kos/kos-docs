# Model

Model是View的控制器，提供View的数据源、数据操作响应（同步action，异步action）、初始化数据加载能力、表单校验和表单字段控制能力，基于kos的扩展能力redux中间件扩展所需要的配置，也将在model上承载


## 一、代码示例

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

## 二、属性说明

### 1. namespace

* 类型：string，
* 说明：namesapce是用来区分不同Model和组件的唯一标识，不能重复，封装后的组件将使用store.getState()对象下的namespace一级的数据


### 2. initial

* 类型：Object
* 说明：提供初始化的默认数据

### 3. reducers

* 类型：Object
* 说明：提供处理同步action的逻辑，key为action.type

我们会注入默认的reducers：

#### 3.1 setState

* 说明：
setState会将payload提供的数据与state进行merge;

* 代码示例

```js
this.props.dispatch({
  type:'setState',
  payload:{
    name:'test1'
  }
});
```


#### 3.1 reset

* 说明：reset会将model.initial下的数据，覆盖store.getState()[namespace]的数据，默认`componentDidMount`的时候，会出发该action

* 代码示例

```js
this.props.dispatch({
  type:'reset',
});
```

### 4 setup

* 说明：加载初始化数据时，执行的action，WrapperComponent会根据Wrapper(config)的autoLoad来确定，是否需要执行

