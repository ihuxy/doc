## router的使用

router注入了全局管理工具，

- router:{push,replace}
- eventBus
- store
- updateRouter
- loading
- 自定义

### router

```
const Menu1=props=>{
  const addFn=item=>{
    props.router.push(`/test1/add`);
  };
  const editFn=item=>{
    props.router.push({
      path:`/test1/edit/${item.id}`,
      state:item,
    });
  };
}

```


### eventBus

[emitter](./emitter)


### store

```
const Demo1=props=>{
  const {store}=props;
  const updateRouter=useCallback(menu=>{
    store.setState({'update-router':{menu}});
  },[]);
  useEffect(()=>{
    const {subscribe}=store;
    subscribe('nav-data',result=>{
      setList(result.list);
    });
  },[]);
}

```

### updateRouter

```
const Demo2=props=>{
  const {updateRouter}=props;
  useEffect(()=>{
    // updateRouter(configs);
    updateRouter(prev=>{...prev,...configs});
  },[]);
}

```

### loading

```
const App=props=>{
  const {components,loading}=useRouter(props);
  return <>
    {components}
    {loading&&<Spinner global />}
  </>;
};

```

### 自定义

```
const configs={
  browserRouter:true,
  title:'app1',
  beforeRender:input=>{},
  basepath:'/app',
  customCfg:{},
  customFunc:()=>{},
}
const {components,loading}=useRouter(configs);

```

### 路由属性

- url：路径
- name：展示名
- icon：图标
- redirect：重定向
- children：子菜单
- component：页面
- denied：权限控制
- hideMenu：菜单隐藏展示
- errorBoundary：错误边界。默认有错误边界处理，可自定义。
- resolve：数据请求并缓存，可用store.getState(key)获取，store.setState(state)更新。
- loadData:数据请求，不缓存数据。


### 数据请求

```
const routers=[
  {
    path:'/',
    redirect:initPath,
    name:'用户管理',
    icon:'users',
    component:()=>import('@layout/usermanager'),
    resolve:{
      users:getUsers,
    },
    children:[
      {
        path:'/app1',
        name:'app1',
        icon:'icon-th-list',
        component:()=>import('@layout/app'),
        loadData:{
          app1List:getList,
        },
      },
    ],
  },
];

// users.js
useEffect(()=>{
  const users=props.store.getState('users');
},[]);

// app1.js
useEffect(()=>{
  props.store.subscribe('app1List',result=>{
    setList(result.data);
  });
},[]);

```













