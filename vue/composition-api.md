## composition-api


vue3正式版发布后，看了一下它的 [composition-api](https://composition-api.vuejs.org/zh/api.html) ，感觉和react hooks很像，于是将之前react写的一些hooks稍微修改（主要是useState、useEffect变为ref、reactive、watch等），也可以在vue中使用。有些展示组件是可以通用的，无需修改。如：

```
import styles from './index.less';
const Spinner=({global})=><div className={global?`${styles.spinner} ${styles.global}`:styles.spinner}>
  <figure className={styles.spinning} />
</div>;
export default Spinner;

```

### 使用jsx

```
npm i -D @vue/babel-plugin-jsx

const plugins=[
  ...,
  '@vue/babel-plugin-jsx'
];

```

### 使用

```
import {createApp} from 'vue';
import App from './app';
createApp(App).mount('#app');

```

```
import {ref,onMounted,reactive,watch,watchEffect} from 'vue';

const setupRouter=props=>{
  const {language,permission}=props;
  const {router:list,title}=i18ns[language];
  const {store,updateRouter,output}=vueRouter({...configs,...props,routers:permRouter(routers(list),permission),title});
  onMounted(()=>{
    const {subscribe,setState}=store;
    setState({permission:permission});
    subscribe('update-router',result=>{
      updateRouter({routers:result.menu});
    });
    setState({langCfg:{...i18ns[language],language}});
  });
  watch(()=>props.language,language=>{
    store.setState({langCfg:{...i18ns[language],language}});
    const {router:list,title}=i18ns[language];
    updateRouter({exact:false,...props,routers:permRouter(routers(list),permission),title});
  });
  return ()=>output.value.loading?<Spinner global />:<output.value.components />;
};

const Router={
  props:{
    language:'',
    permission:[],
  },
  setup:setupRouter,
};


```

### link

```
const setupLink=(props,{attrs,slots,emit})=>{
  const {to,onClick,preventDefault,stopPropagation,exact=true,...rest}=props;
  const handleClick=e=>{
    e.preventDefault();
    if(stopPropagation){
      e.stopPropagation();
    }
    if(typeof onClick==='function'){
      onClick();
    }
    if(!preventDefault){
      const opt=typeof to==='string'?{
        exact,
        path:to,
      }:{
        exact,
        ...to,
      };
      linkEmitter.emit(eventPush,opt);
    }
  };
  return ()=><a onClick={e=>handleClick(e)} href={to?.path??to??''} {...rest}>{slots.default()}</a>;
};

const Link={
  props:{
    to:{},
    preventDefault:false,
    stopPropagation:false,
    onClick:()=>{},
    rest:{},
  },
  setup:setupLink,
};

```


### use

[use](./use.md)

#### useClickAway

```
import {onMounted,onUnmounted} from 'vue';
const useClickAway=(elRef,handleEvent,events='click')=>{
  const handler=event=>{
    const el=elRef?.value??elRef;
    if(el?.contains&&!el.contains(event.target)){
      handleEvent(event);
    }
  };
  onMounted(()=>{
    document.addEventListener(events,handler,false);
  });
  onUnmounted(()=>{
    document.removeEventListener(events,handler,false);
  });
};

export default useClickAway;

```

使用：

```
useClickAway(navRef,e=>open.value=false);

```
























