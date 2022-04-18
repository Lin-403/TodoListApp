# TodoListApp
# 一、基本流程打通（前端-后端-electron）

## 1.gittee创建仓库

https://gitee.com/poppyLin_123/todo-workbench.git

通过git bash clone到本地

## 2.创建umi框架

> 乌米，一个可插拔的企业级react框架。（蚂蚁金服底层前端框架）
>
> umi以路由为基础，支持类nect.js的约定式路由，支持路由级按需加载
>
> - **开箱即用**，内置 react、react-router 等
> - **类 next.js 且[功能完备](https://www.wenjiangs.com/doc/umijs-guide-router)的路由约定**，同时支持配置的路由方式
> - **完善的插件体系**，覆盖从源码到构建产物的每个生命周期
> - **高性能**，通过插件支持 PWA、以路由为单元的 code splitting 等
> - **支持静态页面导出**，适配各种环境，比如中台业务、无线业务、[egg](https://github.com/eggjs/egg)、支付宝钱包、云凤蝶等
> - **开发启动快**，支持一键开启 [dll](https://umijs.org/zh/plugin/umi-plugin-react.html) 等
> - **一键兼容到 IE9**，基于 [umi-plugin-polyfills](https://umijs.org/zh/plugin/umi-plugin-react.html)
> - **完善的 TypeScript 支持**，包括 d.ts 定义和 umi test
> - **与 [dva](https://dvajs.com/) 数据流的深入融合**，支持 duck directory、model 的自动加载、code splitting 等等

进入main文件夹下，输入下面命令：

```
yarn create @umijs/umi-app
```

创建umi框架

## 3.vsCode项目初始化+git项目存储

安装依赖

```
yarn
```

项目启动

```
yarn start
```

存储到远程仓库

```
git add .
git commit -m 'umi项目初始化'
git push

```

## 4.electron项目创建

> electron 可以使用纯js调用丰富的原生APIs创建桌面应用
>
> 其使用web页面作为他的GUI
>
> 可以被看成是一个被js控制的，精简版的Chromium浏览器
>
> 

新建app文件夹，cd进入

初始化项目

```
yarn init
//question name (app): todo-workbench-app
```

electron安装

```
yarn add --dev electron
```

在其package.json下添加，便于在开发模式下打开应用

```
 "scripts": {
    "start": "electron ."
  }
```

创建入口文件index.js

创建页面index.html，复制官方文档文件内容：

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <!-- https://developer.mozilla.org/en-US/docs/Web/HTTP/CSP -->
    <meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self'">
    <meta http-equiv="X-Content-Security-Policy" content="default-src 'self'; script-src 'self'">
    <title>Hello World!</title>
  </head>
  <body>
    <h1>Hello World!</h1>
    We are using Node.js <span id="node-version"></span>,
    Chromium <span id="chrome-version"></span>,
    and Electron <span id="electron-version"></span>.
  </body>
</html>
```

按照官方文档使用

## 5.express创建

> 基于 [Node.js](https://nodejs.org/en/) 平台，快速、开放、极简的 Web 开发框架(node.js的框架)
>
> 提供了一系列强大的特性帮助创建各种web应用，和丰富的http工具
>
> 使用express可以快速搭建一个完整的网站
>
> 核心特性：
>
> 1. 可以设置中间件响应HTTP请求
> 2. 定义了路由表用于执行不同的http请求动作
> 3. 可以通过向模块传递参数来动态渲染HTML页面

app文件夹下创建server文件夹（本质上相当于一个后端服务器）

进入server文件夹下

运行express应用程序生成器

```
npx express-generator
```

安装依赖：

```
yarn
```

运行命令：

```
yarn start
```

## 6.编译

在main目录下输入：

```
yarn build
```

打包在main下生成了一个dist文件夹，文件夹下的文件拿到后端使用，即打通了前后端，然后是electron和后端之间，我们要在electron上把后端跑起来，然后electron直接访问我们的后端资源，从而实现前后端都集成在electron

## 7.express和electron配合

将express中创建HTTP Server的部分封装成函数导出，将其引入到electron中调用，在electron创建窗口前，调用该函数，即开启由express创建的服务器

这里我们要做的是将express创建的服务器使用electron就可以进行开启并使用。

之前我们是在server文件夹下，创建express，然后也是在server文件夹下运行`yarn start`进行创建http服务器并开启然后我们就可以访问3000端口内容，现在我们将开启服务器的代码部分封装成一个函数，将函数导出，再导入到electron的index.js文件下，这样我们即可在开启electron的同时开启服务器，即在app的目录下运行`yarn start`即可同时开启服务器和electron窗口，达到了两者的配合。

然后我们可以通过在app下的（electron）index.html中使用iframe形式导入3000端口界面。

这时可能会报错，原因在于，electron的html文件下：

```html
  <meta http-equiv="Content-Security-Policy" content="default-src 'self'; script-src 'self'">
    <meta http-equiv="X-Content-Security-Policy" content="default-src 'self'; script-src 'self'">
```

规定了安全策略，然后重启成功显示在electron界面上

然后初始化样式：

```css
    <style>
      *{
        margin: 0;
        padding: 0;
      }
      iframe{
        width: 100vw;
        height: 100vh;
        border: none;
      }
    </style>
```



到目前为止，前端将编译成功的静态资源放到后端，后端再由electron进行运行





# 二、基本UI设计（参照飞书+钉钉）

## 1.less和tsx文件创建

## 2.less文件新的写法

当前缀一样时可简写成：&+(不一样的部分)

这里的  "&"  就相当于  .menu-item

```js
.menu-item{
    border: 0;
    background: white;
    box-shadow: 0 0 1px 1px #dddadaad;
    padding: 2px;
    cursor: pointer;
    &:hover{
        
    }
    &_name{

    }
    &_count
}
```

## 3.tsx编写

定义接口规范某一参数的类型，如：

```js
//定义一个接口规范props的类型
interface IProps{
    name:string 
    count:number 
    active:boolean
    icon?:ReactNode  //？的使用可以用来判断icon是否有值，有值的时候才会进行下面操作，也就是说我们的                         //icon不一定能传过来值，有可能是undefined
    onClick:()=>void
}
    
传入参数是对象数组[{},{},{}]
const getConfig=(data:{name:string,value:number}[])=>{}
    
    
传入参数是：
const CARDS_LOCAL_KEY= [
  { i: "allRemain", title:'总剩余任务量' },
  { i: "todaysRemain",title: '今日截止任务量'},
  { i: "todaysFinished", title: '今日已完成任务量' },
  { i: "allFinished",  title: '累计已完成任务量' },
  { i: "outdatedCount",title: '已截止任务量' },

  { i: "finishedRadio",title: '任务完成比' },
  { i: "finishedProgress", title: '任务完成进度' },

];

 参数传递的类型：{i:string,title:string}[]
 const [visibleCardKeys, setVisibleCardKeys] = useState<{i:string,title:string}[]>(
    getLocal(CARDS_LOCAL_KEY, DEFAULT_CARD_KEYS)
  );
```

## 4.antd

**（1）引入调用时间选择器**

main路径下命令行；

```
yarn add antd
```

引入相关API

```js
import {DatePicker} from 'antd'
```

点击进入antd官网选择要使用API，点击查看代码，复制（引入代码）

```js
 <DatePicker showTime onChange={onChange} onOk={onOk} />
```

**（2）自定义表单控件**

> 自定义或第三方的表单控件，也可以与 Form 组件一起使用。只要该组件遵循以下的约定：
>
> > - 提供受控属性 `value` 或其它与 [`valuePropName`](https://ant.design/components/form-cn/#Form.Item) 的值同名的属性。
> > - 提供 `onChange` 事件或 [`trigger`](https://ant.design/components/form-cn/#Form.Item) 的值同名的事件。
>
> 

## 5.iconPark图标引入

进入官网，点击图标库，搜索想要的图标，点击"..."然后复制svg。在main下创建文件夹创建全局组件，创建tsx文件编写，引入刚复制的svg

> SVG是一种基于XML的矢量图形格式，用于在Web和其他环境中显示各种图形；它允许我们编写可缩放的二维图形，并可通过CSS或JavaScript进行操作。
>
> SVG最能够响应当前Web开发对可伸缩性，响应性，交互性，可编程性，性能和可访问性的要求。
>
> 因为SVG是基于矢量的，所有在放大图形时不会出现任何降低或丢失保真度的情况。它们只是重新绘制以适应更大的尺寸，这使得它非常适合多语境场景，例如响应式Web设计。

```
export const PlusIcon=()=>(<svg width="24" height="24" viewBox="0 0 48 48" fill="none" xmlns="http://www.w3.org/2000/svg"><rect width="48" height="48" fill="white" fillOpacity="0.01"/><path d="M24.0607 10L24.024 38" stroke="#333" strokeWidth="4" strokeLinecap="round" strokeLinejoin="round"/><path d="M10 24L38 24" stroke="#333" strokeWidth="4" strokeLinecap="round" strokeLinejoin="round"/></svg>)
```

***注意，所有带下划线 “-” 的都要转成驼峰式命名法，然后xml文件头去掉***

## 6.Hook钩子的使用

```js
import { useState } from 'react';
const [isCreate,setIsCreate]=useState(false)


利用钩子，创建一个状态title和一个改变这个状态setTitle的方法，监听input标签的值的改变并使用这个setTitle同步这个改变到title状态上，然后将这个值进一步处理提交最后渲染到页面上。
const [title, setTitle] = useState('')   //创建钩子

 // 如果useMemo(fn, arr)第二个参数匹配，并且其值发生改变，才会多次执行执行，
 // 否则只执行一次，如果为空数组[]，fn只执行一次
 // 在方法函数，由于不能使用shouldComponentUpdate处理性能问题，react hooks新增了useMemo
const realTitle = useMemo(() => {   //使用useMemo无用渲染的性能问题
    return title ? title : (task?.title || '')
}, [title, task])

const handleTitleChange = (e: any) => {   //监测input改变并改变状态title
    setTitle(e.target.value)
}

const renderTitle = () => {      //页面渲染
    return (
         <Input
         onChange={handleTitleChange}
         name="title"
         value={realTitle} />
     )
}

const handleSubmit = (values: any) => {    //提交本次状态改变结果
	console.log('form', values, realTitle)
}


useEffect副作用钩子，相当于componentDidMount和componentDidUpdate以及componentWillUnMount等周期函数。
```

## 7.calc运算符前后加空格

## 8.onFocus onBlur使用

当我们想点击input并出发什么效果时

onFocus挂载一个函数，（这个函数是由useState()钩子产生，即可以改变某一状态的方法）然后界面的css样式可以使用es6形式进行判断渲染

```
 onFocus={()=>{setIsCreate(true)}}   //onFocus挂载一个改变状态的函数
 
 <div className={`add-task-container ${isCreate?'add-task-container--active':''}`}>                                            //es6字符串形式判断渲染
```





# 三、后端编写

后端数据处理基本都放到electron上，前端会将处理过的静态资源交给后端，然后交给electron处理

## 1.fetch封装请求

百度 fetch mdn

```
https://developer.mozilla.org/zh-CN/docs/Web/API/Fetch_API/Using_Fetch
```



> Fetch API  自身提供一个js接口，用于访问和操作HTTP管道的一些具体部分（请求，响应）
>
> 本身还提供一种全局 fetch()方法，用于跨网络异步获取资源。
>
> 使用全局fetch()规范和JQuery.ajax()的不同之处：
>
> 1. 当接收一个代表错误的HTTP状态码时，fetch放回的Promise不会被标记为reject（相应的http状态码为404或者500），相反他会返回resolve（如果响应的 HTTP 状态码不在 200 - 299 的范围内，则设置 resolve 返回值的 [`ok`](https://developer.mozilla.org/zh-CN/docs/Web/API/Response/ok) 属性为 false ）***仅当网络故障或者请求被组织时，才会标记为reject***
> 2. fetch不会发送跨域cookies，除非使用了 *credentials* 的[初始化选项](https://developer.mozilla.org/zh-CN/docs/Web/API/fetch#参数)
>
> 一个基本fetch请求设置：
>
> ```
> fetch('http://example.com/movies.json')
>   .then(response => response.json())
>   .then(data => console.log(data));
> ```
>
> 上述代码解释：
>
> 我们通过使用fetch传入一个资源路径（提供一个参数用来指明想fetch到的资源路径），此次fetch会返回一个包含相应信息结果（一个response对象）的promise。但这只是一个http响应，并不是我们**想要通过网络获取的一个JSON文件**，所以我们需要对结果进行.json（）方法处理（***该方法返回一个将响应body解析成JSON文件的promise***）。
>
> 

## 2.跨域问题

https://blog.csdn.net/success400/article/details/107208164

app.js文件夹下：

```
app.all("*",function(req,res,next){
  //设置允许跨域的域名，*代表允许任意域名跨域
  res.header("Access-Control-Allow-Origin","http://localhost:8081");
  //允许的header类型
  res.header("Access-Control-Allow-Headers","content-type");
  //跨域允许的请求方式
  res.header("Access-Control-Allow-Methods","DELETE,PUT,POST,GET,OPTIONS");
  if (req.method.toLowerCase() == 'options')
    res.send(200);  //让options尝试请求快速结束
  else
    next();
});

```

## 3.使用json文件存储相关信息

涉及node的文件读取，（文件相关操作）

```js
 const fs = require('fs')
 const path = require('path')
 
 const dbPath=path.join(__dirname,'..','db') //__dirname可以获取当前文件路径，通过join拼接路径

  fs.readFile(`${dbPath}\\DOING.json`, 'utf8' , (err, data) => { //读取文件
    if (err) {
      console.error(err)
      return
    }
    console.log(data,'json')
  })
```

## 4.ToastUI calendar使用

> https://ui.toast.com/tui-calendar
>
> 

## 5.防止重复渲染，操作dom元素

```

```

# 三、动态布局

## 1.Echarts 数据统计

```
npm install echarts --save
。。。。
```

## 2.颜色生成工具

```
http://www.hepou.com/peise/jianbian.html
```

## 3.想在自定义组件中嵌套其他组件

```js
interface IProps {
    // ...略
    children:JSX.Element   //<-------定义一个children
}
export default function CardContainer(props: IProps) {
    const {children, width = 280, height = 200, title ,theme='default'} = props
    return (
        <div>
            <div></div>
            {children}     //<-------放入参数占位
        </div>
    )
}
```

## 4.[react-grid-layout](https://github.com/react-grid-layout) gitHub网格布局，动态卡片

```js
https://github.com/react-grid-layout/react-grid-layout

具体步骤及用法，参照其readMe.md文件

//在statistic组件下引入(这里就是想要应用网格布局，动态卡片的位置)
import GridLayout from "react-grid-layout";

//定义网格布局
const DEFAULT_LAYOUT= [
  { i: "allRemain", x: 2, y: 1, w: 4, h: 3,minW: 4, minH: 3 },
  { i: "todaysRemain", x: 0, y: 0, w: 4, h: 3,minW: 4, minH: 3 },
  { i: "todaysFinished", x: 1, y: 0, w: 4, h: 3,minW: 4, minH: 3 },
  { i: "allFinished", x: 2, y: 1, w: 4, h: 3,minW: 4, minH: 3 },

  { i: "finishedRadio", x: 4, y: 0, w: 5, h: 6, minW: 4, minH: 6 },
  { i: "finishedProgress", x: 4, y: 0, w: 6, h: 7,minW: 6, minH: 7  },

];


//示例中用法：
import GridLayout from "react-grid-layout";

class MyFirstGrid extends React.Component {
  render() {
    // layout is an array of objects, see the demo for more complete usage
    const layout = [
      { i: "a", x: 0, y: 0, w: 1, h: 2, static: true },
      { i: "b", x: 1, y: 0, w: 3, h: 2, minW: 2, maxW: 4 },
      { i: "c", x: 4, y: 0, w: 1, h: 2 }
    ];
    return (
      <GridLayout
        className="layout"
        layout={layout}
        cols={12}        //一行12格
        rowHeight={30}   //行高30
        width={1200}     //行宽1200
      >
        <div key="a">a</div>
        <div key="b">b</div>
        <div key="c">c</div>
      </GridLayout>
    );
  }
}
```

## 5.动态卡片位置记忆

```js
组件中的onLayoutChange可以监听卡片布局的变化

可以通过传入函数形式获得这种变化，然后存入localStorge,之后每次打开，位置保留上一次的布局
onLayoutChange={handleLayoutChange}
```

## 6.鸿蒙字体使用

```js
//官网
https://developer.harmonyos.com/     

//字体下载地址
https://developer.harmonyos.com/cn/docs/design/des-guides/font-0000001157868583


选择相应字体文件后
@font-face{               //自定义字体
  font-family: HarmonyOS;
  src: url('../resource/HarmonyOS_Sans_Regular.ttf');
}
```

## 7.将数据文件上传到gitignore

> 充当数据库存储的db下两个JSON文件属于数据文件，每次同步上传到gitee时我们需要将数据文件一同上传，所以可以将其添加到gitignore上。
>
> gitignore:
>
> 一般来说每个Git项目中都需要一个`.gitignore文`件，这个文件的作用就是告诉Git哪些文件不需要添加到版本管理中。意思就是本地修改完项目后，上传到github等版本管理服务中，本地哪些文件不上传过去.
>
> 例如数据文件可以不上传。
>
> （可以右键添加到gitignore）
>
> ```js
> 先清理缓存
> git rm -r --cached .
> 然后三条指令同步gitee
> 
> //文件添加
> # JSON 数据文件
> app/server/db/DOING.json
> app/server/db/DONE.json
> ```
>
> 

## 8.修改全局css变量

```js
Object.entries(values).forEach(e=>{   // entries可以取出对象中的键值对
    const [key,value]=e;
    document.documentElement.style.setProperty(`--${key}`,String(value))  //改变全局css变量
})
```

## 9.强制修改

> update((pre) => pre + 1)
>
> 为了让全局进行重新渲染，可以尝试强制渲染，即强制改变某一状态而导致页面整体渲染同步。

# 四、项目打包

> ```
> 在main目录下 yarn bulid打包前端资源
> 
> 将dist文件夹下umi.css和umi.js文件以及statix文件夹复制到app文件夹下server/public下，在app目录下执行yarn make
> ```
>
> 
>
> 安装脚手架
>
> yarn add --dev @electron-forge/cli
>
> npx electron-forge import 
>
> 
>
> 这里可能出现问题：
>
> 添加：
>
> "description": "待办工作台",
>
> "author": "XuMin",
>
> 
>
> yarn make
