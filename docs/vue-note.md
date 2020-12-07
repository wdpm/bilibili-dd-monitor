# note

##  z-index
- dropdown 1000
- modal 3000
- notification 5000(default)

## router and init
Vue切换路由时，
- 不同组件，会触发created()
- 相同组件，会重用组件，不会created()

## Angular vtbList page
> https://angular.io/guide/zone

禁止采用原repo方式获取数据。 每次切换进入vtblist都会发新请求到backend，没有必要。 因为这种方式
- 用户必须手动切换路由，才能触发更新
- 网络数据流量每次都是2000+条，但是实际上后续更新时，一般都是50-200条之间数据发生变更。数据量大，真正有用的数据量少。

更好的解决方案：
- 初始化vtblist时，不触发任何到electron的数据请求
- 将vtbInfos标记为计算属性，监控vuex的数据。如果vuex更新，那么页面自动会更新。
这样，只有第一次数据量较大，后续更新压力很小。
- 创建 VtbInfoUpdateListenerService。专门监听后端数据更新，后端在数据更新时会使用IPC发送到前端，
前端触发actions，最终更新到vuex。