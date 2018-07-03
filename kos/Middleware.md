# Middleware

kos的中间件就是redux中间件，kos中间件的开发参见：[redux中间件开发](http://www.ruanyifeng.com/blog/2016/09/redux_tutorial_part_two_async_operations.html)


## async-middleware

KOS内置了异步处理中间件，用于处理异步的action，使用async/await来解决异步问题，让代码更加的流畅，异步逻辑的支持情况如下:


`view.js`
```js
dispatch({
  type:'loadLis',
  paylaod:{}
})
```


`model.js`

```js
{
  asyncs:{
    async loadList(dispatch,getState,action){
      const params={};
      await fetch({
        url:'',
        data:param
      });

      dispatch({
        type:'setState',
        payload:{
          ...
        }
      });
    }
  }
}

```
