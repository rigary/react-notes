# effect

示例：
```
app.model({
  namespace: 'todos',
  effects: {
    *addRemote({ payload: todo }, { put, call }) {
      yield call(addTodo, todo);
      yield put({ type: 'add', payload: todo });
    },
  },
});
```

### put

用于触发action

```
yield put({ type: 'todos/add', payload: 'Learn Dva'});
```

### call

用于调用异步逻辑，支持Promise。

```
const result = yield call(fetch, '/todos'); 
```

这个call与JS的call用法大概一致，这个call的第一个参数是你要调用的函数，第二个参数开始是你要传递的参数，可一 一传递。

### select

用于从state里获取数据。

```
const todos = yield select(state => state.todos); 
```


