connect的作用是将组件和models结合在一起。将models中的state绑定到组件的props中。并提供一些额外的功能，譬如dispatch

connect 的使用

【connect 方法返回的也是一个 React 组件，通常称为容器组件。因为它是原始 UI 组件的容器，即在外面包了一层 State。

connect 方法传入的第一个参数是 mapStateToProps 函数，该函数需要返回一个对象，用于建立 State 到 Props 的映射关系。】

简而言之，connect接收一个函数，返回一个函数。

第一个函数会注入全部的models，你需要返回一个新的对象，挑选该组件所需要的models。

```
export default connect(({ user, login, global = {}, loading }) => ({
currentUser: user.currentUser,
collapsed: global.collapsed,
fetchingNotices: loading.effects['global/fetchNotices'],
notices: global.notices
}))(BasicLayout);

// 简化版
export default connect(
({ user, login, global = {}, loading }) => { return {
currentUser: user.currentUser,
collapsed: global.collapsed,
fetchingNotices: loading.effects['global/fetchNotices'],
notices: global.notices
}
}
)(BasicLayout);
```


@connect的使用

其实只是connect的装饰器、语法糖罢了。

```
// 将 model 和 component 串联起来
export default connect(({ user, login, global = {}, loading }) => ({
currentUser: user.currentUser,
collapsed: global.collapsed,
fetchingNotices: loading.effects['global/fetchNotices'],
notices: global.notices,
menuData: login.menuData, // by hzy
redirectData: login.redirectData, // by hzy
}))(BasicLayout);

// 改为这样（export 的不再是connect，而是class组件本身。），也是可以执行的，但要注意@connect必须放在export default class前面：
// 将 model 和 component 串联起来

@connect(({ user, login, global = {}, loading }) => ({
currentUser: user.currentUser,
collapsed: global.collapsed,
fetchingNotices: loading.effects['global/fetchNotices'],
notices: global.notices,
menuData: login.menuData,
redirectData: login.redirectData,
}))

export default class BasicLayout extends React.PureComponent { // ...
}
```
