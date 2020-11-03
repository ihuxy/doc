## 常用Hooks

[use](../use/use.md)


### useAsync

[useAsync](../use/useAsync.md)

```
const useAsync=(initState={})=>{
  const {cancelablePromise}=useCancelablePromise();
  const state=ref(initState);
  const resultRef=ref({});
  const pendingRef=ref({});
  const clearResult=(keys=null)=>{
    if(Array.isArray(keys)&&keys.length){
      keys.map(key=>resultRef.value[key]=initState[key]);
    }else{
      resultRef.value=initState;
      pendingRef.value={};
    }
    state.value=resultRef.value;
  };
  const update=(asyncFns,handler,reset=false)=>{
    state.value={...};
  };
  return [state,update,clearResult];
};

```

### useCancelablePromise

```
const useCancelablePromise=()=>{
  const promises=ref([]);
  onUnmounted(()=>{
    promises.value.map(fn=>fn.cancelFn());
    promises.value=[];
  },[]);
  const cancelablePromise=(fn,delay=true)=>{
    const wrapPromise=handlePromise(fn,delay);
    if(promises.value.indexOf(wrapPromise)===-1){
      promises.value.push(wrapPromise);
    }
    return wrapPromise.promiseFn;
  };
  return {cancelablePromise};
};

```

### useRaf

```
const useRaf=(initialState={})=>{
  const frame=ref(0);
  const state=ref(initialState);
  const setRaf=value=>{
    cancelAnimationFrame(frame.value);
    frame.value=requestAnimationFrame(()=>{
      state.value=value;
    });
  };
  onUnmounted(()=>{
    cancelAnimationFrame(frame.value);
  });
  return [state,setRaf];
};

```

### useScroll

```
const useScroll=(element=null)=>{
  const listener=isElement(element)?element:window;
  const [state,setState]=useRaf(getOffset(element));
  const handler=()=>setState(getOffset(element));
  onMounted(()=>{
    listener.addEventListener('scroll',handler,{capture:false,passive:true});
  });
  onUnmounted(()=>{
    listener.removeEventListener('scroll',handler);
  });
  return state;
};

```

### useSearch

[useSearch](../use/useSearch.md)

```
const useSearch=(initState,str2Dom=str2Vue)=>{
  const state=ref(initState);
  const setList=(...args)=>{
    const [data,keyword,...rest]=args;
    if(!keyword){
      state.value=null;
    }else{
      args=[data,keyword,str2Dom,...rest];
      const newList=filterList(...args);
      state.value=newList;
    }
  };
  return [state,setList];
};

```

### useWinResize

```
const useWinResize=()=>{
  const [state,setState]=useRaf(getViewportSize());
  const handler=()=>setState(getViewportSize());
  onMounted(()=>{
    window.addEventListener('resize',handler,false);
  });
  onUnmounted(()=>{
    window.removeEventListener('resize',handler,false);
  });
  return state;
};

```

### useEleResize

[useViewSize](../use/useViewSize.md)

```
const useEleResize=(element=null)=>{
  const {bind,destroy}=resize(element);
  const [state,setState]=useRaf(getViewportSize(element));
  const handler=()=>setState(getViewportSize(element));
  onMounted(()=>{
    bind(handler);
  });
  onUnmounted(()=>{
    destroy();
  });
  return state;
};

```

### useClickAway

```
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

```

