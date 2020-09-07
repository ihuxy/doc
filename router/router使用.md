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

















