# 前置知识

### Refs and the DOM

> Refs 提供了一种方式，允许我们访问： DOM 节点、或在 render 方法中创建的 React 元素

#### 创建 Refs

+ 通过 `React.createRef()` 创建

+ 通过 `ref` 属性附加到 React 元素

+ 在构造组件的时候将 Refs 分配给实例对象，可以在整个组件中引用

+ ```jsx
  class MyComponent extends React.Component {
  	constructor(props) {
  		super(props)
  		this.myRef = React.createRef()
  	}
  	render() {
  		return <div ref={this.myRef} />
  	}
  }
  ```

#### 访问 Refs

+ 当 `ref` 被传递给 render 中的元素时，可以在 Ref 的 `current` 属性中访问到该节点的引用

+ ```jsx
  const node = this.myRef.current
  ```

+ Ref 的值根据节点的类型而有所不同

  + 当 `ref` 属性用于 HTML 元素时，通过 React.createRef() 创建的 Ref 接收底层 DOM 元素作为其 `current` 值

    + React 会在组件挂在时给 current 属性传入 DOM 元素
    + 在组件卸载时传入 null 值
    + ref 会在 componentDidMount 或 componentDidUpdate 生命周期钩子触发前更新

  + 当 `ref` 属性用于自定义的 class 组件时，Ref 对象接收组件的挂载实例作为其 `current` 属性

  + 不能在函数组件上使用 ref 属性，因为它们没有实例

    + 可以在函数组件内部使用 ref 属性，只要其指向一个 DOM 元素或者 class 组件

    + ```jsx
      function CustomTextInput(props){
      	const textInput = useRef(null)
      	
      	function handleClick() {
      		textInput.current.focus()
      	}
      	
      	return (
      		<div>
      			<input
      				type='text'
      				ref={textInput}
      			/>
      			<input
              type="button"
              value="Focus the text input"
              onClick={handleClick}
            />
      		</div>
      	)
      }
      ```



# uesRef Hook

+ `useRef()` 返回一个可变的 Ref 对象，其 `.current` 属性被初始化为传入的参数，**返回的 Ref 对象在组件的整个生命周期内保持不变**。

  + ```jsx
    function TextInputWithFocusButton() {
      const inputEl = useRef(null);
      const onButtonClick = () => {
        // `current` 指向已挂载到 DOM 上的文本输入元素
        inputEl.current.focus();
      };
      return (
        <>
          <input ref={inputEl} type="text" />
          <button onClick={onButtonClick}>Focus the input</button>
        </>
      );
    }
    ```

  + 本质上，`useRef` 就像是可以在其 `.current` 属性中保存一个可变值的 “盒子”。

  + `useRef()` 比 `ref` 属性更有用， `useRef()` 创建的是一个普通 Javascript 对象，会在每次渲染时返回同一个 Ref 对象。

    +  `ref` 属性访问 DOM 的主要方式，如果将 Ref 对象以 `<div ref={myRef} />` 形式传入组件，则无论该节点如何改变，React 都会将 ref 对象的 `.current` 属性设置为相应的 DOM 节点。

+ 当 Ref 对象内容发生变化时， useRef 不会通知你，**变更 `.current` 属性不会引发组件重新渲染**。

  + 如果想要在 React 绑定或解绑 DOM 节点的 ref 时运行某些代码，则需要使用回调 ref 来实现。

+ `useRef()` 可类似实例变量

  + Ref 对象是一个 current 属性可变且可以容纳任意值的通用容器，类似于一个 class 的实例属性。

  + 可以在 useEffect 内部对其进行写入：

  + ```jsx
    function Timer() {
      const intervalRef = useRef();
    
      useEffect(() => {
        const id = setInterval(() => {
          // ...
        });
        intervalRef.current = id;
        return () => {
          clearInterval(intervalRef.current);
        };
      });
    
      // ...
    }
    ```

  + 实例
  
    + [Hooks 学习：useRef——useEffect 的依赖项在多个函数中重写（1）](https://codesandbox.io/s/hooks-xuexiuserefuseeffect-deyilaixiangzaiduogehanshuzhongchongxie1-9mbhu)
    + [Hooks 学习：useRef——useEffect 的依赖项在多个函数中重写（2）](https://codesandbox.io/s/hooks-xuexiuserefuseeffect-deyilaixiangzaiduogehanshuzhongchongxie2-2uwib)
    + [Hooks 学习：useRef——useEffect 的依赖项在多个函数中重写（3）](https://codesandbox.io/s/hooks-xuexiuserefuseeffect-deyilaixiangzaiduogehanshuzhongchongxie3-755xy)
    + [Hooks 学习：useRef——useEffect 的依赖项在多个函数中重写（4）](https://codesandbox.io/s/hooks-xuexiuserefuseeffect-deyilaixiangzaiduogehanshuzhongchongxie4-sbshe)
    + [Hooks 学习：useRef——useEffect 的依赖项在多个函数中重写（5）](https://codesandbox.io/s/hooks-xuexiuserefuseeffect-deyilaixiangzaiduogehanshuzhongchongxie5-kkpo3)

