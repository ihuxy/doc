## 派生 state

### 派生 state 的常见 bug

名词“受控”和“非受控”通常用来指代表单的 inputs，但是也可以用来描述数据频繁更新的组件。用 props 传入数据的话，组件可以被认为是受控（因为组件被父级传入的 props 控制）。数据只保存在组件内部的 state 的话，是非受控组件（因为外部没办法直接控制 state）。

常见的错误就是把两者混为一谈。当一个派生 state 值也被 setState 方法更新时，这个值就不是一个单一来源的值了。加载外部数据示例描述的行为和这个类似，但是有很重要的区别。在加载外部数据示例中，数据 source 和 loading 都有非常清晰并且唯一的数据来源。当 prop 改变时，loading 的状态一定会改变。相反，state 只有在 prop 改变时才会改变，除非组件内部还有其他行为改变这个状态。

上述条件如果有一个不满足，就会导致问题，最常见的就是在两个表单里修改数据。



### 反面模式:

#### 直接复制 prop 到 state

最常见的误解就是 getDerivedStateFromProps 和 componentWillReceiveProps 只会在 props “改变”时才会调用。实际上只要父级重新渲染时，这两个生命周期函数就会重新调用，不管 props 有没有“变化”。所以，在这两个方法内直接复制（unconditionally）props 到 state 是不安全的。这样做会导致 state 后没有正确渲染。

在实践中，一个组件会接收多个 prop，任何一个 prop 的改变都会导致重新渲染和不正确的状态重置。加上行内函数和对象 prop，创建一个完全可靠的 shouldComponentUpdate 会变得越来越难。而且 shouldComponentUpdate 的最佳实践是用于性能提升，而不是改正不合适的派生 state。

#### 在 props 变化后修改 state

使用 componentWillReceiveProps、 getDerivedStateFromProps。

虽然这个设计就有问题，但是这样的错误很常见，幸运的是，有两个方案能解决这些问题。这两者的关键在于，***任何数据，都要保证只有一个数据来源，而且避免直接复制它。***我们来看看这两个方案。


### 建议的模式

#### 完全可控的组件

删除 state，完全使用props。

#### 有 key 的非可控组件

让组件自己存储临时的 state。在这种情况下，组件仍然可以从 prop 接收“初始值”，但是更改之后的值就和 prop 没关系了。

每次 key 变化，组件都会用新的初始值重新创建。

大部分情况下，这是处理重置 state 的最好的办法。

> 这听起来很慢，但是这点的性能是可以忽略的。如果在组件树的更新上有很重的逻辑，这样反而会更快，因为省略了子组件 diff。


### 概括

设计组件时，重要的是确定组件是受控组件还是非受控组件。

不要直接复制（mirror） props 的值到 state 中，而是去实现一个受控的组件，然后在父组件里合并两个值。比如，不要在子组件里被动的接受 props.value 并跟踪一个临时的 state.value，而要在父组件里管理 state.draftValue 和 state.committedValue，直接控制子组件里的值。这样数据才更加明确可预测。

对于不受控的组件，当你想在 prop 变化（通常是 ID ）时重置 state 的话，可以选择以下几种方式：

- 建议: 重置内部所有的初始 state，使用 key 属性
- 选项一：仅更改某些字段，观察特殊属性的变化（比如 props.userID）。
- 选项二：使用 ref 调用实例方法。


```
function Button({ color, children }) {
  const textColor = useMemo(
    () => slowlyCalculateTextColor(color),
    [color] // ✅ 除非 `color` 改变，不会重新计算
  );
  return (
    <button className={'Button-' + color + ' Button-text-' + textColor}>
      {children}
    </button>
  );
}

```



***需要停止将 “接收 props” 视为与 “渲染” 不同的东西。 由父组件引起的重渲染不应与由本地状态更改引起的重渲染不同。组件应该具有弹性，能适应更少或更频繁地渲染，否则它们与特定父组件存在过多耦合。***


当你真正想从 props 派生 state 时，尽管有一些不同的解决方案。

通常你应该使用一个完全受控制的组件

```
function TextInput({ value, onChange }) {
  return (
    <input
      value={value}
      onChange={onChange}
    />
  );
}

```

或者使用一个不受控的组件，加上 key 来重置它

```
function TextInput() {
  const [value, setValue] = useState('');
  return (
    <input
      value={value}
      onChange={e => setValue(e.target.value)}
    />
  );
}

// 之后我们能通过更改 key 来重置内部 state：
<TextInput key={formId} />

```

要对你的组件进行压力测试，可以将这段代码临时添加到它的父组件：

```
componentDidMount() {
  // 之后别忘了删除这行！
  setInterval(() => this.forceUpdate(), 100);
}

```


不要将 “接受 props” 视作特殊的事件。避免 “同步” props 和 state。大部分情况下，每个值都应该是完全控制的（通过 props），或者完全不受控制的（在本地 state 里）。

### 优化

- 将多个有关配置的 props 组合成一个 options 是个不错的实践。
- 组件的props存在不兼容性时，是时候考虑拆分组件了。
- 将 props 作为 state 的初始值。使用 useMemo 代替 useState。
- state装太多，使用枚举来管理状态。
- useState 过多，合并。






```

const useStateFromProp=(initialValue)=>{
  const [value, setValue] = useState(initialValue);

  useEffect(() => setValue(initialValue), [initialValue]);

  return [value, setValue];
}


```



## 异步渲染


```
 const LazyComponent = React.lazy(() => import("./some-component"));

 function ParentComponent() {

    return (

        <React.Suspense fallback={<div>Loading...</div>}>

            <div>

                <LazyComponent />

            </div>

        </React.Suspense>

    );

}

```


递归非常适合遍历树型结构. 但是它有一个最大的局限性, 那就是不能将某个工作拆分为粒度更小的单元. 我们不能暂停组件的更新工作并且在后续的某个时间段内恢复它. React 会一直通过这种方式来遍历, 直到处理完所有的组件并且调用栈为空.

***那么 React 是如何不通过递归的形式去遍历整棵树的呢? 事实上, React 采用了单链表树状结构的遍历算法. 这使得可以暂定遍历并且抑制调用栈的增长.***


要实现这个算法, 需要一个数据结构, 它有三个字段:

- child - 指向第一个子节点
- sibling - 指向第一个兄弟节点
- return - 指向父节点

在新的 reconciliation 算法条件下, 由 Fiber 来调用上述字段组成的数据结构. 





```
const useTest=initState=>{
  const [state,setState]=useState(handle(initState));
  const update=useCallback(state=>setState(handle(state)),[]);
  return [state,update];
};


const useDerivedState=(propValue,compute)=>{
  const [previousPropValue,setPreviousPropValue]=useState(propValue);
  const [value,setValue]=useState(()=>compute(propValue,undefined));
  if(previousPropValue!==propValue){
    setValue(state=>compute(propValue,state));
    setPreviousPropValue(propValue);
  }
  return [value,setValue];
};


```









### 薛定谔的function














