# 过渡组件 dva-loading

## 传统做法

比如请求一个用户页面，刚进去的时候由于要去服务器请求数据需要花费时间，这段时间页面应该是不能点击的状态。
如果不使用这个组件，传统做法是写个蒙版组件，提供两个方法 start() 和 end()，当进行 ajax 请求开始时调用 start() 方法给整个页面加上一层蒙版，此时不能进行操作，在请求结束也就是 ajax 的 success 回调函数中调用 end() 方法关闭蒙版，表明数据已经请求到了，可以操作页面。

## 作用
该组件仅仅监听异步加载状态，这从它的调用方式就可以看出来 const isLoading = loading.effects['user/query']，其中 user/query 是 model 中的异步请求方法。

loading 在异步请求发出那一刻会持续监听该异步请求方法的状态，在异步请求结束之前 isLoading 的值一直是 true，当此次异步请求结束时 isLoading 的值变成 false，同时 loading 对象停止监听。

## 配置
dva 项目的 index.js 文件：

```
import createLoading from 'dva-loading';

const app = dva();

app.use(createLoading());

```

配置完成后，在任何一个 dva 的 routes 组件中就都会有一个 loading 对象，如果你对 dva 稍有了解的话，应该不难知道它在哪。比如下面这行代码中的 loading 对象就是由于上面的配置。

```
export default connect(({ app, loading }) => ({ app, loading }))(App);
```

打印一下 loading 对象，可看到内容如下：

```
loading: {
  global: false,
  models: {app: false},
  effects: {app: false}
}
```

loading 有三个方法，其中 loading.effects['user/query'] 为监听单一异步请求状态，当页面处于异步加载状态时该值为 true，当页面加载完成时，自动监听该值为 false。

如果同时发出若干个异步请求，需求是当所有异步请求都响应才做下一步操作，可以使用 loading.global() 方法，该方法监听所有异步请求的状态。

## 怎么用？
使用 Antd 的 Table 组件 时，查阅 API 可以看到有个 loading 的属性。如果该属性值为 true，Table 组件自身会显示加载效果，该值为 false，加载效果消失。可以通过 loading 对象判断当前是否有异步加载。具体示例代码如下：

```
// src > models >user.js
export default {
  namespace: 'user',
  state: {
    userList: [],      // 存放用户列表
  },
  effects: {
    * query ({ payload = {} }, { call, put }) {
      // 获取用户列表，赋值给 userList
      // 使用 axios 或者 ajax 请求后台返回数据
      const result = axios.request('xxx/xxx');
      // 调用 reducers 中的 updateState 方法改变 state 中 userList 的值
      yield put({ type: 'updateState', payload: { userList: result.data });
    }
  },
  reducers: {
    updateState (state, { payload }) {
      return { ...state, ...payload };
    },
  },
}

// src > routes > user.js
import React from 'react';
import { connect } from 'dva';
import { Table } from 'antd';

const User = ({ dispatch, user, loading }) => {
  /** 
    根据 loading.effects 对象判断当前异步加载是否完成，并将该值传递给 Table 组件的 loading 属性，
    Table 组件会自己控制加载样式。dva-loading 在这里的作用只是提供异步加载的状态，
    具体加载样式由对应组件自己提供。
  */
  const isLoading = loading.effects['user/query']
  const { userList } = user

  return (
    <Table
      dataSource={userList}
      loading={isLoading}
      rowKey={record => record.id}
    />
  );
}

export default connect(({ user, loading }) => ({ user, loading }))(User);
```
 
