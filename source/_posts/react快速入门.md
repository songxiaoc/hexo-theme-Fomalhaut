---
title: React 快速入门
sticky: 1
tag:
  - 编程学习
  - 知识分享
  - 前端
abbrlink: 61aa4cfb
---
# React 快速入门

参考：

视频：https://www.bilibili.com/video/BV1tY411G7UP

笔记：http://codesohigh.com/


假设已经有了其他框架的基础（比如 Vue）。

在学习 React 时会看到小程序的一些影子；Vue3 也参照了 React。可以说，React 一直走在各个框架的前沿，是标杆级别的存在。



## 准备工作

Vscode

- 插件 react：快速生成 jsx 页面的基本结构
  - 输入 rcc（React Class Component），生成 React 类组件
  - 输入 rfc（React Function Component），生成 React 函数式组件

- 需要配置输入 tab 自动在 jsx 页面中补全 html 标签

## 初始化项目

通过脚手架构建项目：

```bash
# demo 是项目名称
npx create-react-app demo
```

> npx：一次性安装，命令执行完后，安装的依赖就删除了

安装完之后，使用集成开发环境打开（VsCode、WebStorm... 都行）该文件夹，查看 `package.json` 中的 `scripts`，查看有哪些可运行的脚本

启动项目

```bash
# start 是脚本的名称，run 可省略
npm run start
```

项目默认在 `http://localhost:3000` 运行，打开浏览器，看到转动的 React 图标，大功告成！

<img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1307/image-20221224163244093.png" alt="image-20221224163244093" style="zoom: 33%;" />



## 入口文件

初始化文件结构：

<img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1307/image-20221224203934668.png" alt="image-20221224203934668" style="zoom:67%;" />



删除 `src` 下的所有文件，保留 `index.js` ，对其进行修改，如下：

这是 React18 版本前的写法

```jsx
import ReactDOM from 'react-dom';

ReactDOM.render(
  <h2>Hello, World</h2>,
  document.getElementById('root')
)
```

此时，页面会出现 `Hello, World`，但是控制台可能会报错

React18 版本写法

```jsx
import ReactDOM from 'react-dom/client';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <h2>Hello, World</h2>
);
```



## 组件化开发

实际开发过程中，不可能只有一个 `h2` 标签，如果把代码都写到入口文件中，显然是不妥的。

在 `src` 下新建一个文件：`App.tsx` ，学过 Vue 的应该知道，这是一个最大的组件。（关于 tsx 后缀，后面会提到）

### 类组件

类组件基于 es6 的 class 关键字。

```jsx
// 引入的 React 对象中有很多的属性和方法，以后都可能会用到
import React from "react";

const msg = 'hello, world';

// ES6 语法，定义一个类，类名一般和文件名保持一致
class App extends React.Component {
  // 生命周期函数，return 的内容表示要渲染的内容，任何一个组件都必须要有 render 函数
  render() {
    // 使用圆括号，可以在 js 中嵌套 html
    return (
      <div>
        {/* 使用花括号，可以在 html 中嵌套 js */}
        <h2>{msg}</h2>
        <input type="text" />
      </div>
    )
  }
}

export default App;
```

在 `index.js` 中使用该组件

```js
import ReactDOM from 'react-dom';
// 引入组件
import App from './App';

ReactDOM.render(
  // 使用
  <App />,
  document.getElementById('root')
)
```

回到浏览器看看效果吧~

### 函数式组件

见 [函数式组件](#jump1)



## JSX 语法

- 使用圆括号 `()`，可以在 js 中嵌套 html

- 使用花括号 `{}`，可以在 html 中嵌套 js （变量）。**不是双花括号，这不是 Vue 哦**



1. for => htmlFor

   ```jsx
   import React from "react";
   
   export default class App extends React.Component {
     render() {
       return (
         <div>
           {/* label 用来聚焦，当点击 “用户名：” 时，输入框会获取焦点 */}
           {/* 原始写法，使用 for，会报错 */}
           {/* <label for="username">用户名：</label> */}
           {/* jsx 写法，使用 htmlFor */}
           <label htmlFor="username">用户名：</label>
           <input type="text" id="username" />
         </div>
       )
     }
   }
   ```

   在 js 语法中，for 其实是用来写 for 循环的，所以会出现冲突报错

<img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1307/image-20221224213858597.png" alt="image-20221224213858597" style="zoom: 80%;" />

​	

2. class => className

   在 js 语法中，class 用来定义一个类（ES6），所以会出现冲突报错

   ```jsx
   import React from "react";
   
   export default class App extends React.Component {
     render() {
       return (
         <div>
           {/* 原始写法，使用 class，会报错 */}
           {/* <div class="box">盒子</div> */}
           <div className="box">盒子</div>
         </div>
       )
     }
   }
   ```

   

3. html 标签中的属性，不能使用引号，要使用单花括号，表示要写 js 变量了

   ```jsx
   <button onClick={() => {}}>累加</button>
   ```

   

4. 双花括号

   ```jsx
   import React from "react";
   
   export default class App extends React.Component {
     render() {
       return (
         <div>
           {/* 原始写法 */}
           {/* <div style="background: blue;">content</div> */}
           {/* 注意使用驼峰代替短横线，属性值使用引号引起来 */}
           <div style={{ backgroundColor: 'blue' }}>content</div>
         </div>
       )
     }
   }
   ```

   原始写法会报错，style 的值不能是一个字符串，因为在 js 中引号引起来表示这是一个字符串

   ![image-20221224220605622](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1307/image-20221224220605622.png)

   其实，不是平白无故突然冒出来一个双花括号的，可以理解为单花括号中是一个对象（样式对象）

   ```jsx
   import React from "react";
   // 一个对象
   const myStyle = { backgroundColor: 'blue', textAlign: 'center' }
   
   export default class App extends React.Component {
     render() {
       return (
         <div>
           <div style={myStyle}>content</div>
         </div>
       )
     }
   }
   ```



5. 文件名可以是 jsx 或者 js，不影响文件中的代码（**我们这里约定，jsx 用来表示这是一个组件，里面可能写了一些 html；js 后缀就纯粹写 js 代码**）

6. 组件名称必须大写，否则控制台会报错

![image-20221224210824673](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1307/image-20221224210824673.png)



## 根标签

render() 的 return 值要有一个根标签，否则页面会报错

我们首先想到的是 `div` ，但是会平白无故多出一个 `div` 标签。react 提供了一种解决方案

```jsx
render() {
    return (
      <>
        <div style={{ backgroundColor: flag ? 'skyblue' : 'pink'}}>content</div>
      </>
    )
}
```



## for循环

在 react 中，for 循环只能使用 map，不能使用 forEach。因为我们需要 for 循环的返回值来作为页面的内容，而只有 map 才有返回值。

```jsx
import React from "react";

const arr = ['刘备', '关羽', '张飞'];

export default class App extends React.Component {
  render() {
    // 在 js 中写 html，用圆括号
    return (
      <>
        {/* 在 html 中写js，用单花括号 */}
        <ul>
          {
            arr.map((item, index) => {
              // 在 js 中写 html，用圆括号
              // return (
              //   <li key={index}>{item}</li>
              // )
              // 简写
              return <li key={index}>{item}</li>

            })
            // 简写
            // arr.map((item, index) => <li key={index}>{item}</li>)
          }
        </ul>
      </>
    )
  }
}
```

花括号必须写，圆括号可写可不写，jsx 会识别出来



## setState()和累加

### 累加案例

每个变量修改，都要更新到视图吗？这样性能不高。

以下写法可以实现累加，但是页面并出现累加的效果（不会重新渲染），因为 num 的变化没有被监听到。

```jsx
import React, { Component } from 'react'

let num = 1;

export default class App extends Component {
  render() {
    return (
      <div>
        <h2>数字为：{num}</h2>
        <button onClick={() => {
          num++;
          console.log(num);
          }}>累加</button>
      </div>
    )
  }
}
```

<span style='font-weight: bold; color: red;'>当数据发生修改时，如何触发视图更新呢？</span>通过 `setState()`

首先将数据的定义放到 state 对象中。

数据的获取通过  `this.state.xxx`

数据的修改通过  `this.setState()`

```jsx
import React, { Component } from 'react'

export default class App3 extends Component {
/* 完整写法
  constructor(props) {
    super(props);
    this.state = {
      num: 1
    }
  }
   */
  // 简写
  state = {
    num: 1
  }
  render() {
    return (
      <div>
        <h2>数字为：{this.state.num}</h2>
        {/* 为什么不是 this.state.num++ ？ */}
        {/* 我们修改是通过 setState 来修改，this.state.num++ 相当于直接修改 num，再通过 setState 赋值给 num */}
        {/* 不建议直接修改，建议通过 setState 来修改 */}
        <button onClick={() => { this.setState({ num: this.state.num + 1 }) }}>累加</button>
      </div>
    )
  }
}
```



### setState()的几种写法

```jsx
import React, { Component } from 'react'

export default class App3 extends Component {
  constructor(props) {
    super(props);
    this.state = {
      num: 1,
    }
    // 相当于就是写法2，只是换个变量而已，有人会这么写
    this.addNum3 = this.addNum1.bind(this)
  }
  render() {
    return (
      <div>
        <h2>数字为：{this.state.num}</h2>
        {/* 写法1：适合代码简短 */}
        <button onClick={() => { this.setState({ num: this.state.num + 1 }) }}>累加1</button>
        {/* 写法2 */}
        {/* <button onClick={this.addNum1}>累加2 调用普通函数</button> */}
        <button onClick={this.addNum1.bind(this)}>累加2 调用普通函数</button>
        {/* 写法3 使用箭头函数规避 this 的指向问题 */}
        <button onClick={this.addNum2}>累加3 直接调用箭头函数</button>
        <button onClick={() => this.addNum1()}>累加3 箭头函数中调用普通函数</button>{/* 这里，addNum1 后面要加括号，因为箭头函数内部调用的 addNum1 需要立即执行 */}
        {/* 写法4 本质和写法2相同 */}
        <button onClick={this.addNum3}>写法3</button>
      </div>
    )
  }
  // 普通函数
  addNum1() {
    // console.log(this);// undefined，需要使用 bind 修改 this 的指向
    this.setState({
      num: this.state.num + 1
    })
  }
  // 箭头函数
  addNum2 = () => {
    console.log(this);// App3，当前类
    this.setState({
      num: this.state.num + 1
    })
  }
}
```



## 函数传参

点击按钮 1，就打印出 1，点击按钮 2，就打印出 2，用同一个函数来实现

```jsx
import React, { Component } from 'react'

export default class App4 extends Component {
  render() {
    return (
      <div>
        <button onClick={() => this.btnClick(1)}>按钮1</button>
        <button onClick={() => this.btnClick(2)}>按钮2</button>
      </div>
    )
  }
  btnClick(num) {
    console.log(num);
  }
}
```



## jsx 文件中引入 css 样式

直接写

```jsx
import React, { Component } from 'react'

export default class App4 extends Component {
  render() {
    return (
      <div style={{background: 'pink', width: '100px', height: '100px'}}></div>
    )
  }
}
```

引入样式

```jsx
import React, { Component } from 'react'
import './App4.css'

export default class App4 extends Component {
  render() {
    return (
      <div className='box'></div>
    )
  }
}
```



## <span id="jump1">函数式组件</span>

函数式组件基于构造函数。

箭头函数

```jsx
const App = () => {
  console.log(this);// undefined
  return (
    <div>
      hello
    </div>
  );
}

export default App;
```

普通函数

```jsx
function App() {
  return (
    <div>
      hello
    </div>
  );
}

export default App;
```

#### 函数式组件的特点

1. 函数式组件没有生命周期
2. 函数式组件没有 this
3. 函数式组件没有 state 状态（this.state.xxx ，this 都没有，哪来的 state？）



起初，类组件和函数式组件是同时存在的，但是由于函数式组件没有生命周期、this、state，啥都用不了，导致使用它的开发者很少。直到 hooks 的出现，函数式组件变的极其灵活，而且十分简洁。



## 组件传参

### 父传子

把 num 参数的值传递到 Child 组件中

App3 => Father => Child

```jsx
import React from 'react'

function Child(props) {
    return <h2>num = {props.num}</h2>
}

function Father(props) {
    return <Child num={props.num} />
}

export default function App3() {
    return (
        <Father num={456} />
    )
}
```

### 子传父

无论是 vue，小程序还是 react，所谓的子传父，真实在干活的是父组件。

子传父的前提是要父传子，即父组件传递给子组件一个用来修改对应数据的函数，相当于子组件调用了父组件的方法来修改值**（儿子改不了爸爸，只能通知爸爸，让爸爸自己修改自己）**

```jsx
import React, { useState } from 'react'

function Child(props) {
    return (
        <>
            <h2>num = {props.num}</h2>
            <button onClick={() => props.changeNum(props.num + 1)}>修改父组件的数据</button>
        </>
    )
}

function Father(props) {
    return <Child num={props.num} changeNum={props.changeNum} />
}

export default function App3() {
    const [num, setNum] = useState(123)
	
    const changeNum = (newNum) => {
        setNum(newNum)
    }
    return (
        // 也可以直接传 setNum 函数
        <Father num={num} changeNum={changeNum} />
    )
}
```

### <span id='jump3'>任意组件传值</span>

在子传父的例子中，如何直接从顶级组件 App 将参数**直接传递到** Child 组件？（跨过 Father 组件）

```jsx
// 引入 createContext
import React, { useState, createContext } from 'react'

// 创建上下文空间，名称随意
// 一旦创建了上下文空间，就会有两个东西：Provider，Consumer
const NumContext = createContext();

export default function App4() {
    const [num, setNum] = useState(123)

    return (
        // 1. Provider 是一对标签，通过 value 属性来提供数据
        <NumContext.Provider value={{ num, setNum }}> {/* 如果是传多个参数，需要再加一个花括号，变成对象形式 */}
            <Father /> {/* 将 组件放在 Provider 标签中才能共享数据 */}
            {/* <Child /> 直接传递给 Child 组件也是可以的 */}
        </NumContext.Provider>
    )
}

// 普通函数形式的组件
// function Father() {
//     return <Child />
// }

// 箭头函数形式的组件，只有一行
const Father = () => <Child />

function Child() {
    return ( // 圆括号表示要在 js 中写 html
        <NumContext.Consumer>
            { // 花括号表示 要在 html 中写 js
                ({ num, setNum }) => { // 如果要接收多个参数，需要进行解构
                    return ( // 圆括号表示要在 js 中写 html
                        <>
                            <h2>{num}</h2>
                            <button onClick={() => setNum(num + 1)}>累加</button>
                        </>
                    )
                }
            }
        </NumContext.Consumer>
    )
}
```



我们可以发现，使用 createContext 之后，Child 组件的代码嵌套的层级很多，一旦多起来就容易乱。可以使用 [useContext](#jump2) 这个 Hook 来代替。



## React Hooks

React 官方定义的 Hook

开发人员自定义的 Hook

<span style='font-weight: bold; color: red;'>Hook 只能用在组件函数中的最顶层！</span>



### useState

useState 是一个生命周期函数，既然是函数，就有返回值

```jsx
// 可以在控制台看看，useState 是什么
const xxx = useState();
console.log(xxx); // [undefined, ƒ]
```

可以看到，useState 返回的是一个数组，这个数组有两个参数。

第一个参数 `undefined` 是我们要传入的参数，第二个参数 `f` 是一个函数，用来修改参数的值



案例：

当点击按钮的时候，修改页面显示的数据，并更新视图

```jsx
import React, { useState } from 'react'

// let msg = 'hello, world';

export default function App1() {
    // Hook 只能用在组件函数中的最顶层
    let [msg, setMsg] = useState('hello, world');// 'hello, world' 是 msg 的初始值

    // const fn1 = () => {
    //     msg = 'song'
    //     console.log(msg);
    // }

    return (
        <>
            <h2>{msg}</h2>
            {/* <button onClick={fn1}>修改 msg，但无法更新视图</button> */}
            <button onClick={() => setMsg('song')}>修改 msg</button>
        </>
    )
}
```



### useEffect

react 的函数式组件没有生命周期，即不像类组件那样有 mounted（页面挂载后请求数据）、updated（监测数据更新）、beforeDestroyed（一般在这个阶段处理脏数据、垃圾回收，如数据恢复为原始值，删除变量） 等过程。

但是我们可以使用 useEffect 来模拟这几个生命周期。



如何模拟一个销毁过程？比如过 3 秒，把挂载的组件替换成别的组件，那么原先的组件就是被销毁了。

```jsx
import ReactDOM from 'react-dom/client';
import App from './App2';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
    <App />
);
// 模拟一个销毁过程
setTimeout(() => {
    root.render(
        <input type='text'/>
    );
}, 3000)
```

模拟生命周期

```jsx
import React from 'react'
import { useState, useEffect } from 'react'

export default function App2() {
    const [num1, setNum1] = useState(1);
    const [num2, setNum2] = useState(1);

    // 模拟 mounted，在这个位置写 ajax
    useEffect(() => {
        console.log('挂载完毕~~');
    }, [])

    // 模拟 updated，监测数据更新
    useEffect(() => {
        console.log('num1 更新了~~');
    }, [num1])

    // 当 useEffect 没有第二个参数，所有数据更新都会触发回调函数
    // useEffect(() => {
    //     console.log('所有数据更新了~~');
    // })
    // 要监测页面所有数据的更新，有两个选择，将所有数据写到数组中，或者直接不要第二个数组参数
    
    // 模拟 beforeDestroy
    useEffect(() => {
        return () => {
            console.log('销毁阶段');
        }
    })

    return (
        <>
            <h2>{num1}</h2>
            <button onClick={() => setNum1(num1 + 1)}>累加 num1</button>
            <hr />
            <h2>{num2}</h2>
            <button onClick={() => setNum2(num2 + 1)}>累加 num2</button>
        </>
    )
}
```



### <span id="jump2">useContext</span>

完整代码：[任意组件传值](#jump3)

```jsx
function Child() {
    // 需要指明使用的是哪一个上下文对象，此处是 NumContext
    const { num, setNum } = useContext(NumContext); // 解构
    return (
        <>
            <h2>{num}</h2>
            <button onClick={() => setNum(num + 1)}>累加</button>
        </>

    )
}
```



### memo

memory

看一个例子：

父组件中用到了子组件，当我们点击父组件中的累加按钮，就会使得 `num` 加 1，然后重新渲染页面。

但是我们注意看控制台，发现每次点击按钮，子组件也会被重新渲染。<span style='font-weight: bold; color: red;'>这会非常的消耗性能</span>。

<img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1307/image-20230102192839427.png" alt="image-20230102192839427" style="zoom:67%;" />



```jsx
import React, { useState, memo } from 'react'

const Child = () => {
    console.log('子组件出现了~');
    return <div>Child 子组件</div>
}

export default function App7() {
    const [num, setNum] = useState(1);
    return (
        <>
            <h2>{num}</h2>
            <button onClick={ () => setNum(num + 1)}>累加</button>
            <Child />
        </>
    )
}
```

使用 memo，将子组件包裹起来，就可以解决这个问题。

```jsx
const Child = memo(() => {
    console.log('子组件出现了~');
    return <div>Child 子组件</div>
})
```

#### 注意

有时候，子组件并不是简单的 HTML，可能需要实现一些功能，如：子组件中有一个按钮，通过按钮实现累加功能

> doSth 方法 和 num 是定义在父组件的，子组件只是简单的调用

```jsx
import React, { useState, memo } from 'react'

const Child = memo((props) => {
    console.log('子组件出现了~');
    return (
        <>
            <div>Child 子组件</div>
            {/* 子组件中有一个按钮，通过按钮实现累加功能 */}
            <button onClick={props.doSth}>累加</button>
        </>
    )
})

export default function App7() {
    const [num, setNum] = useState(1);
    const doSth = () => {
        setNum(num + 1);
    }
    return (
        <>
            <h2>{num}</h2>
            <Child doSth={doSth} />
        </>
    )
}
```

当我们点击按钮的时候，发现 `memo` 失效了，子组件又再次重新渲染了。这是因为：**`memo` 只对纯静态的子组件有效**。如果子组件中有一些事件和其他功能，就会失效。这时候就要结合 `useCallback` 进行处理。



### useCallback

使用 `useCallback` 将事件套起来，配合 `memo` 可以实现组件的缓存，当我们点击子组件中的事件去修改父组件的值，同时不会触发子组件的重新渲染。

```jsx
import React, { useState, memo, useCallback } from 'react'

const Child = memo((props) => {
    console.log('子组件出现了~');
    return (
        <>
            <div>Child 子组件</div>
            {/* 子组件中有一个按钮，通过按钮实现累加功能 */}
            <button onClick={props.doSth}>累加</button>
        </>
    )
})

export default function App7() {
    const [num, setNum] = useState(1);

    /*
        1. setNum(num + 1) 直接使用新值覆盖旧值
            在这里，num 初始值为 1，点击累加， num = 初始值 + 1 = 2，但是初始值没变，再次点击累加，还是 num = 初始值 + 1 = 2
        2. setNum(num => num + 1) 回调的形式，不断使用新的值覆盖旧值，如 2 覆盖 1，3 覆盖 2...覆盖的是之前修改过的旧值

        如果在下面的 doSth 中我们使用第一种方式，那么页面只会出现一次累加的效果，其实是因为 num 一直是 2（可以打印看看）
    */

    // 使用 useCallback 将 doSth 包裹起来，同时 [] 表示不检测数据的更新（类似于 useEffect）
    const doSth = useCallback(() => {
        // 此处使用函数回调的形式
        setNum(num => num + 1);
    }, []) // 空数组表示不检测更新，这样就不会触发子组件的重新渲染了

    // console.log(num);
    return (
        <>
            <h2>{num}</h2>
            <Child doSth={doSth} />
        </>
    )
}
```



### useMemo

和 `useCallback` 相似，但是使用方式不一样，需要多套一层函数

> 函数中返回函数：**高阶函数**

```jsx
const doSth = useMemo(() => {
    // 函数中返回函数：高阶函数
    return () => setNum(num => num + 1);
}, [])
```

对比一下  `useCallback` ：

在函数中就直接调用了` setNum()` 这个方法

```jsx
const doSth = useCallback(() => {
    // 此处使用函数回调的形式
    setNum(num => num + 1);
}, []) // 空数组表示不检测更新，这样就不会触发子组件的重新渲染了
```



## 受控组件与不受控组件

受控组件和不受控组件只存在于表单元素（input、textarea...）

1. 受控组件：表单元素的 value 需要通过 state（useState） 来定义，受到 state 的控制

2. 不受控组件：表单元素的 value 无法通过 state 获取，只能使用 ref (或 useRef) 来获取

### 1. 受控组件

![image-20230102181633281](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1307/image-20230102181633281.png)

将表单元素的 value 定义在 state 中，可以实现**实现 data 变量 => 页面表单元素的值**；onChange 可以**实现页面表单元素的值 => data 变量**，从而实现双向数据绑定。



#### 案例：

**实现类似于 v-model 的效果**，同时点击按钮的时候，获取 input 输入框中的值

1. 当页面元素的值发生改变的时候，会触发 `onChange`
2. `onChange` 中的事件回调中，将页面元素身上的值 set 到 value 这个变量中，实现了双向绑定

```jsx
import React, { useState } from 'react'

export default function App6() {
    const [value, setValue] = useState('song');

    // 通过事件对象获取到页面元素身上的值，并 set 到 value 这个变量中，实现了双向绑定
    const inputChange = e => {
        // console.log(e.target.value);
        setValue(e.target.value)
    }

    return (
        <>
            {/* 当页面元素的值发生改变的时候，会触发 onChange */}
            <input type="text" value={value} onChange={inputChange} />
            <button onClick={() => console.log(value)}>获取 input 的值</button>
        </>
    )
}
```



### 2. 不受控组件

Vue 中的 ref 和 React 是一样的，这样就可以避免使用原生 JS 操作 DOM 元素。

```jsx
import React, { useRef } from 'react'

export default function App6() {
    const element = useRef();
    return (
        <>
            {/* 使用 ref，将 input 和 element 变量进行绑定 */}
            <input type="text" ref={element} />
            {/* 可以 console.log(element) 看看里面有什么 */}
            <button onClick={() => console.log(element.current.value)}>获取 input 的值</button>
        </>
    )
}
```



## React Redux

官网：https://react-redux.js.org/

React Redux 是基于 Redux 的。Redux 是类似于 vuex 的状态管理工具，但是 vuex 只能用于 vue，Redux 可以用于绝大部分的框架，当然也可以用于 React。相对来说，Redux 比较不好上手，因此我们可以直接使用 React Redux。



React Redux 是基于[上下文空间 context](#jump3) 的思想设计的

![image-20230106182237539](https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1307/image-20230106182237539.png)

### 安装

React Redux 基于 Redux，因此安装时需要两者一起安装：

```bash
npm install @reduxjs/toolkit react-redux
```



### 仓库创建

在 src下，创建 store 目录，包含 reducer.js（**创建初始状态，并导出一个函数**）以及 index.js（**仓库的入口文件**）。

store/reducer.js

```js
// 创建仓库的初始状态
const defaultState = {
    // reducer 中可以创建一些默认的状态，初始数据
    num: 1
}

export default (state = defaultState) => {
    return state;
}
```



store/index.js

```js
// 仓库的入口文件
// 引入 reducer
import reducer from "./reducer";
// 创建仓库
import { configureStore } from "@reduxjs/toolkit";

// 仓库的数据来自于 reducer
// 注意，reducer 需要有一对 花括号 包裹
const store = configureStore({ reducer });

// 导出仓库
export default store;
```



### 仓库引入

在项目的全局入口文件 index.js 中，给顶级组件套一个 `Provider`，就可向组件共享数据

```jsx
import ReactDOM from 'react-dom/client';
import App from './App8';
// 引入一个提供器
import { Provider } from 'react-redux';
// 引入仓库
import store from './store';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
    // 使用标签包括顶级组件，并共享数据
    <Provider store={store}>
        <App />
    </Provider>
);
```



### 案例：在组件上获取到数据（state 映射）

要使用数据的组件（App8）

```jsx
import React from 'react'
// 引入连接器，连接提供器
import { connect } from 'react-redux';

// 使用 props 接收
function App8(props) {
  return (
    <>
      <div>数字为 {props.num}</div>
    </>
  )
}

// 状态映射：
  // 将 reducer 中的 state 映射成 props
  // 让开发者可以在组件中使用 props.num 去调用 state 中的 num
// 每一个映射都是一个函数
// 将 mapStateToProps 写在 connect 的第一个参数的位置，就会自动获取到一个参数 state
// 这个 state 就是 store/reducer.js 中 return 的 state 
const mapStateToProps = (state) => {
  return {
    num: state.num
  }
}

// export default connect(state映射，dispatch映射)(当前组件名称)
export default connect(mapStateToProps)(App8);
```



此时，页面上就会显示 num 的值



### 案例：点击按钮让 reducer.js 中的 num 加 1（dispatch 映射）

事件派发映射



组件：

```jsx
import React from 'react'
// 引入连接器，连接提供器
import { connect } from 'react-redux';

// 使用 props 接收
function App8(props) {
  return (
    <>
      <div>数字为 {props.num}</div>
      {/* 调用事件 */}
      <button onClick={() => props.addNum()}>累加</button>
    </>
  )
}

const mapStateToProps = (state) => {
  return {
    num: state.num
  }
}

// 事件派发映射：将 reducer 中的事件映射成 props
// 让开发者可以在组件中使用 props.addNum() 实现 num 的累加
// 和 mapStateToProps 同理，当 mapDispatchToProps 写在 connect 的第二个参数位置，
// 就会被分配一个形参 dispatch 
const mapDispatchToProps = (dispatch) => {
  return {
    addNum() {
      // type 这个名称也是约定俗成，官网也是这么写的
      const action = {type: 'leijia'}
      // 这里的 action 会传递到 reducer 的 action
      dispatch(action)
    }
  }
}

// export default connect(state映射，dispatch映射)(当前组件名称)
export default connect(mapStateToProps, mapDispatchToProps)(App8);
```



reducer.js 中，导出的函数还有第二个形参

```js
// 创建仓库的初始状态
const defaultState = {
    num: 1
}

// 这里还有第二个参数 action，是一个对象
export default (state = defaultState, action) => {
    // 深拷贝，直接修改 state 不生效
    let newState = JSON.parse(JSON.stringify(state))
    // 官网建议我们使用 switch-case 的形式，在 action 传值那里有例子
    if (action.type === 'leijia') {
        newState.num++;
    }
    return newState;
}
```



### 案例：action 传值

我们现在想要实现一个效果，num 每次增加一个固定值，这个值由组件来决定



组件需要修改的地方：

```jsx
const mapDispatchToProps = (dispatch) => {
  return {
    addNum() {
      // 这里可以再给 action 定义别的属性，比如 value
      const action = {type: 'leijia', value: 3}
      dispatch(action)
    }
  }
}
```

reducer.js

```js
const defaultState = {
    num: 1
}

// 这里还有第二个参数 action，是一个对象
export default (state = defaultState, action) => {
    let newState = JSON.parse(JSON.stringify(state))
    // 使用 switch-case
    switch(action.type) {
        case ('leijia'): {
            // 从 action 中获取值
            newState.num += action.value;
            break;
        }
    }
    return newState;
}
```



## 路由

路由的版本非常重要，一定要选对。假设你忘记了如何进行路由传参，在网上直接百度，可能由于路由版本不同，你试了也没效果。再过几个月，可能又换新版本了。

官网：https://reactrouter.com/en/main/start/tutorial

### 安装

```bash
npm install react-router-dom localforage match-sorter sort-by
```



### 快速上手

初始化几个组件

pages/Detail.jsx、pages/Home.jsx、pages/List.jsx

```jsx
import React from 'react'

export default function Home() {
  return (
    <h2>首页Home</h2>
  )
}
```

全局入口 index.js

```js
import ReactDOM from 'react-dom/client';
import App from './App9';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
    <App />
);
```



路由本质也是一个组件，可以写 jsx 后缀

新建 router/index.jsx，在其中引入刚才写好的组件

```jsx
import Home from '../pages/Home';
import Detail from '../pages/Detail';
import List from '../pages/List';
import App from '../App9'

import { BrowserRouter, Route, Routes } from 'react-router-dom';

const BaseRouter = () => /* {
    return ( */
        <BrowserRouter>
            <Routes>
                {/* 注意 element 中的 App 要写成标签形式，不是直接一个 App */}
                <Route path='/' element={<App />}>
                    <Route path='/home' element={<Home />}></Route>
                    <Route path='/list' element={<List />}></Route>
                    <Route path='/detail' element={<Detail />}></Route>
                </Route>
            </Routes>
        </BrowserRouter>
/*     )
} */

export default BaseRouter;
```

react-router-dom 中有两种模式：

- History 模式：地址栏不带 # 号，需要配置 nginx（**BrowserRouter**）

- Hash 模式：地址栏带 # 号，打包后可以直接放到线上（**HashRouter**）



全局入口文件 index.js 引入写好的 router/index.jsx

```js
import ReactDOM from 'react-dom/client';
// import App from './App9';
import Router from './router'

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
    // <App />
    <Router />
);
```



在 顶级组件中，指定子路由显示的位置：`<Outlet />` （vue 指定路由显示的位置：`router-view` 标签）



```jsx
import React from 'react'
import { Outlet } from 'react-router-dom'

export default function App9() {
  return (
    <>
      <div>App9</div>
      <Outlet />
    </>
  )
}
```



使用 `Link` 标签替代 `a` 标签（vue 中是 `router-link`）

```jsx
import React from 'react'
import { Outlet, Link } from 'react-router-dom'

export default function App9() {
  return (
    <>
      <ul>
        {/* to='/home' 没有花括号，也是可以的 */}
        <li><Link to={'/home'}>主页</Link></li>
        <li><Link to={'/List'}>列表页</Link></li>
        <li><Link to={'/Detail'}>详情页</Link></li>
      </ul>
      <hr />
      <div>App9</div>
      <Outlet />
    </>
  )
}
```



<img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1307/image-20230107150728914.png" alt="image-20230107150728914" style="zoom:67%;" />



### 路由跳转携带参数

前两种见 [useParams](#jump4) 和 [useSearchParams](#jump5) 这两个钩子



#### 使用事件跳转

当我们想要携带多个参数跳转，前两种方式就比较麻烦。



App 组件

```jsx
import React from 'react'
import { Outlet, useNavigate } from 'react-router-dom'

export default function App9() {
  const navigate = useNavigate();
  const toDetail = () => {
    navigate('/detail', {
      // 使用 state 携带数据
      state: {
        username: 'songxiao'
      }
    })
  }

  return (
    <>
      <button onClick={toDetail}>跳转到详情页</button>
      <div>App9</div>
      <Outlet />
    </>
  )
}
```



Detail 组件

**使用的是 useLocation 钩子**

```jsx
import React from 'react'
import { useLocation } from 'react-router-dom'

export default function Detail() {
  const location = useLocation();
  return (
    <>
      <h2>用户名：{location.state?.username}</h2>
    </>
  )
}
```

当我们点击按钮，就可以跳转到 Detail 组件，并携带数据



### 404 路由匹配

Error 组件

```jsx
import React from 'react'
import ErrorImg from '../assets/404.png'

export default function Error() {
  return (
    <img src={ErrorImg} alt="" />
  )
}
```



router/index.jsx 中，如果想要整个页面显示 404 组件，那么就需要与 App 同级

```jsx
<Routes>
    <Route path='/' element={<App />}>
        <Route path='/home' element={<Home />}></Route>
        <Route path='/list/:id' element={<List />}></Route>
        <Route path='/detail' element={<Detail />}></Route>
    </Route>
    {/* 与 App 组件同级 */}
    <Route path='*' element={<Error />}></Route>
</Routes>
```



当我们访问一个不存在的 URL，比如 `localhost:3000/hello` ，就会跳转到 404 页面



## React Router Hooks

### useLocation

在 App 组件中获取当前页面的路径

```jsx
import React from 'react'
// 引入 useLocation
import { Outlet, Link, useLocation } from 'react-router-dom'

export default function App9() {
  const location = useLocation();
  console.log(location);// {pathname: '/home', search: '', hash: '', state: null, key: 'default'}
  console.log(location.pathname);

  return (
    <>
      <li><Link to={'/home'}>主页</Link></li>
      <hr />
      <div>App9</div>
      <Outlet />
    </>
  )
}
```



### useNavigate

如果我们**使用事件的形式**来跳转页面，可以使用 useNavigate 这个钩子。

案例：点击按钮，跳转到详情页

```jsx
import React from 'react'
import { Outlet, Link, useNavigate } from 'react-router-dom'

export default function App9() {
  const navigate = useNavigate();
  // 点击事件
  const toDetail = () => navigate('/detail')

  return (
    <>
      <button onClick={toDetail}>跳转到详情页</button>
      <hr />
      <div>App9</div>
      <Outlet />
    </>
  )
}
```



### <span id='jump4'>useParams</span>

当我们想要跳转到某个页面并**携带参数**时，可以使用 useParams 钩子。

App 组件

```jsx
import React from 'react'
import { Outlet, Link, useNavigate } from 'react-router-dom'

export default function App9() {
  return (
    <>
        {/* 携带参数 */}
        <Link to={'/List/123'}>列表页</Link>
      <hr />
      <div>App9</div>
      <Outlet />
    </>
  )
}
```



router/index.jsx 中，需要修改一下

```jsx
<Route path='/list/:id' element={<List />}></Route>
```



在 List 组件中获取传递的参数

```jsx
import React from 'react'
import {useParams} from 'react-router-dom'

export default function List() {
  // useParams() 返回一个对象，解构出来
  const { id } = useParams();
  return (
    <h2>列表页List - {id}</h2>
  )
}
```



### <span id='jump5'>useSearchParams</span>

使用问号的形式**携带参数**（我们产生了疑问（问号），就要搜索（Search）=> 使用的是 useSearchParams 而不是 useParams）

```jsx
<li><Link to={'/Detail/?id=456&tag=唱&tag=跳&tag=rap&tag=篮球'}>详情页</Link></li>
```



Detail 组件中

如果传递的是单个，使用 `get()` 获取

如果传递的是多个值（例如多选框），使用 `getAll()` 获取

```jsx
import React from 'react'
import { useSearchParams } from 'react-router-dom'

export default function Detail() {
  const [searchParams] = useSearchParams();
  // 获取单个
  let id = searchParams.get('id');
  // 获取多个
  let tag = searchParams.getAll('tag');
  return (
    <>
      <h2>详情页Detail - {id}</h2>
      <h2>爱好</h2>
      <ul>
        {
          tag.map((item, index) => <li key={index}>{item}</li>)
        }
      </ul>
    </>
  )
}
```



<img src="https://xingqiu-tuchuang-1256524210.cos.ap-shanghai.myqcloud.com/1307/image-20230107161037033.png" alt="image-20230107161037033" style="zoom: 67%;" />



