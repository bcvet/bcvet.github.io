# 现代化前端开发模式的构建
现代化前端开发,应该具备 `模块化`,`组件化`,`工程化` 三个特点,我们目标,就是通过组合各种开源技术,构建出符合上述特点,又适合自己团队的前端开发技术栈.

新版本的开发中,不再考虑对 `ie8` 浏览器的支持,为开发带来了很大的方便.
新版本开发中,能够全面使用基于`HTML5`的技术进行开发,预期使用的主要技术有: `HTML5`,`CSS3`,`React`,`webpack`,`es6`.

---
## 用什么技术开发?

### HTML5/CSS3
HTML5是w3c制定的新一代html标准,是对HTML4标准的一种扩充,内置了很多使用更为方便的现代解决方案.

HTML5的重大特性有:
- 语义化标签,对原有标签的扩充,使代码可读性更好,语义化更强.
- 音频/视频.`<video>`与`<audio>`.
- canvas与svg,提供更好的图形图像操作支持,用于富媒体展示,替代flash.
- 对web app的支持.离线缓存,本地存储,拖放,地理特性.
- WebSocket.可以方便构建实时web应用.

`ie9`对于HTML5的支持并不全面,只支持部分特性.如果使用了不支持的特性,可以使用`html5shiv`使ie9支持.
	<!--[if lt IE 9]>
  		<script src="http://html5shiv.googlecode.com/svn/trunk/html5.js"></script>
	<![endif]-->

### React(体系)
`React`是当前诸多前端框架中的一种.与其他前端框架相比,`React`最大的特点就是`组件化`开发模式.(另外一个组件化实现比较好的框架是`Vue`).

以`React`技术为基础,形成了一套完整的**开源技术体系**.

`React` + `React-Router` + `redux`是当前最为主流的组合.

`React`的优点(使用React的原因)
- 代表了最先进思路的发展方向,开发思路较传统前端开发发生革命性变化.能够使用较长时间.
- 创造了`Virtual DOM`的概念,让开发人员不再直接操作DOM,由框架来接管处理DOM变化.
	- 不用再关注DOM的变化,大大简化了代码逻辑.只需定义好各种状态下的展示效果,逻辑由**改变DOM**转换为**改变组件状态**.
	- 规避了部分低效处理DOM的操作,提高界面渲染效率.
	- 实现了UI层的`跨平台`:一个`Virtual DOM`可以转变为多种UI(`Web`,`iOS`,`Android`,`Canvas`).
- 第一个真正实现了组件化的前台开发框架.
- 文档与配套工具完善,既有大公司支持,又有发展速度惊人的开源社区.
- 设计完善,思路超前.
	- 到目前的0.14版本后已趋于稳定,基本没有太大的’坑’.
	- 随着`React`的组件化思路提出,新的前端框架全部采用组件化设计(`Angular2`,`Vue`).
	- `FLUX`思想的提出,简化了数据流,所有的前端主流框架都开始提供`FLUX`实现.
- 在APP开发时,后续还可以逐步过渡到react-native,实现原生组件.

`React`的缺点
- 与传统开发相比,思路转变较大,需要摒弃旧的思想,**thinking in react**,需要适应.
- 原有的以`jquery`为体系基础的各种前端库已无法方便使用(可以使用,但不建议使用),需要在`npm`与`github`上寻找尝试相应的替代品.
- 单独使用`React`而不搭配`flux-like`的库使用的话,开发复杂应用时,逻辑复杂度会比较高.

#### React
`React`技术本身,是一种使用组件化思路实现的**表现层**框架,主要用于页面渲染.`React`的入门比较简单,但是易学难精,需要多看官方文档,并慢慢总结一些最佳实践.

理解`React`的思想:**组件就是状态机. UI = f(state) **.

![用组件化思考](https://raw.githubusercontent.com/bcvet/bcvet.github.io/master/img/morden-frontend/thinking-in-react-components.png)

用好`React`,主要在于三个问题的思考:
- 如何(以什么粒度)拆分组件?
- 组件与组件之间的关系是什么?(父子?兄弟?无关联?)
- 组件之间如何通信?

![复杂的层级关系](https://raw.githubusercontent.com/bcvet/bcvet.github.io/master/img/morden-frontend/react-tree.png)

#### React-Router
`React-Router`为`React`提供了路由功能,可以方便的实现页面内路由功能,是构建**单页面应用(SPA)**的基础.(具体由程序实例演示)

#### FLUX与flux/redux
`FLUX`是facebook公司提供的一种数据流模型.与传统的`MVC`数据流模型不同的是,`FLUX`为单向数据流,在复杂状态下,不会出现未知情况.从现实的角度来说,`FLUX`的出现就是为了解决上文中出现的复杂组件的通信问题.

`flux`为facebook官方的实现,较为复杂.`redux`更为简化,是目前被广泛采用的`flux-like`库.

### Webpack
`webpack`是前台`工程化`的基础.随着`webpack`插件集的完善,已经基本可以用`webpack`搞定所有工作,不再需要`grunt`/`gulp`.

`webpack`的核心功能:
- 为基于`CommonJS`和`ES6`的模块化程序提供编译支持.
- 为`JSX`提供编译支持.

`webpack`的扩展功能:
- 在JavaScript中按需加载`css/less/sass/img`等资源.
- 使用`JSLint`/`ESLint`进行静态代码检测.
- 代码压缩/合并.
- 代码按需提取(CommonsChunkPlugin).
- 代码按需复制(CopyWebpackPlugin).
- HTML模板化(HtmlWebpackPlugin).
- 支持liveload与代理的`webpack-dev server`.
- 全局注入(ProvidePlugin).
- 支持基于`jQuery`的库.

### ES6/ES2015
`ES6`为最新版的`ECMA-Script标准`.与`ES3` - `ES5`的变化相比,`ES6`的变化是巨大且有实质性的的,因此广受欢迎并被快速推广.

目前主流浏览器对`ES6`的支持还非常有限.基于这种现状,出现了`babel`,**用于将ES6代码编译为ES5代码并在浏览器中执行**,为使用es6编写代码提供了可能.

但本质上来说,`ES6`并没有改变原有的代码编写思想,只是在原来的JavaScript语言基础上提供了更多的**语法糖**,属于锦上添花,不强制使用,可以选择性使用,提高开发效率与代码可读性.在我们的开发中,`ES6`并非核心技术,只是基于`ES6`的`import`与`export`实现`模块化`.

目前的工程中,已经成功将`babel`集成至`webpack`中,提供了`ES6`的使用环境,可以使用`ES6`进行开发.

建议开发时使用的`ES6`特性:
- 模块化(必须使用).
- 使用类语法创建React组件(有待商榷).
- 模板字符串.
- `let` & `const`.
- 解构赋值.

不建议开发时使用的`ES6`特性:
- 块级作用域(由于语法特性在浏览器端不支持,无法模拟).
- `Promise`(Promise不再使用xhr进行通信,可能存在潜在的兼容问题).
- 箭头表达式(为`babel6`中的stage-0特性,目前编译环境不支持).

---
## 如何开发?

### 工具
- `Webstrom/Intellij IDEA`
	- 最为完善的解决方案,提供了对最新技术的支持,最好的代码编辑与补全,最全的工具集.
	- 占用内存较多.
- `SublimeText/Atom/VisualStudioCode + CLI`
	- 现代文本编辑器,通过插件的支持与更新,提供了不输IDE的功能.
	- 轻量,打开/关闭/反应迅速,使用无负担.
	- 高级功能方面与专业IDE仍有较大差距.自动补全较弱,代码分析检查能力薄弱,需要与其他工具(`SVN客户端`,`CLI`,`nginx`)结合进行开发.
- `Eclipse`
	- 不建议使用Eclipse进行前端开发.

### 前后台交互
采用前后台分离的开发模式,前台与后台通过`ajax`技术通信,用json数据交互.`RESTFUL`规范已确定,按照规范编写接口与符合格式的数据.

前端使用独立的项目,与后端项目分离,需要使用单独的服务器在本地运行调试.针对可能存在的跨域问题,可以使用的几种方案有:
	- 将前后端工程都部署到`Tomcat`中,使前后端应用都具有相同的域,只是上下文不同,前端可直接调用后端.
	- 本地搭建`nginx`环境,使用`nginx`配置,允许跨域访问;或者直接使用`本地测试环境(212服务器)`上部署的后端API(已配置好`nginx`).
	- 使用`webpack-devserver`运行前端应用,可通过配置代理的方式解决跨域问题.最为推荐.

### 目录结构与资源目录的划分
原来的状态为将所有资源都放到同一文件夹下,然后先按种类分类,再按业务模块分类.

![按模块分层的结构](https://raw.githubusercontent.com/bcvet/bcvet.github.io/master/img/dir-struct.png)

理想状态参见[前端工程-基础篇](https://github.com/fouber/blog/issues/10)一文.

当前状态详见`手机版目录结构`.

未来状态取决于css是否可以按组件抽取.

### 开发规范
开发规范使用`Airbnb的JavaScript代码规范`(此规范目前基本为通用规范),使用`es-lint`插件进行规范自动检测.

具体使用规则参见[公司技术网站](http://bcvet.github.io/).

### 关于jQuery
在传统的以操作DOM为基础的开发模式中,`jQuery`为开发的核心技术.

主要提供了如下几大功能:
	1. DOM查询
	2. DOM操作
	3. AJAX操作
	4. 工具库
	5. 第三方库(插件)
上述`1`,`2`两点,也是`jQuery`技术的根本.

基于`React`构建的技术体系中,**已经不再需要DOM操作**.所以`jQuery`存在的必要性大大降低.去除`jQuery`的好处有:
	- 破除对`jQuery`的习惯性依赖,在每次需要操作DOM时,都思考这种操作是否必要.
	- 减少JavaScript代码库体积.

对于AJAX操作,可以使用专门的AJAX库.

对于工具方法,以`lodash`库为基础,积累自己的工具库.

第三方插件,在`npm`和`github`中寻找相应的替代工具.

总的原则是,逐步减少对`jQuery`依赖,直到完全抛弃`jQuery`.

### 回顾并检测生态系统的闭环
试图使用当前技术体系,满足[前端开发体系建设日记 #2](https://github.com/fouber/blog/issues/2)中提出的场景需求.

![工程化实践](https://raw.githubusercontent.com/bcvet/bcvet.github.io/master/img/morden-frontend/production-practice.png)

### 可能遇到的问题
- 组件如何划分?
- css如何做?组件化做到什么程度?
 
---
## 参考资料
### HTML5
[w3school的HTML5教程](http://www.w3school.com.cn/html5/)
[Is IE9 a modern browser?](https://people.mozilla.org/~prouget/ie9/)
### React
[React官方文档中文版](http://reactjs.cn/react/docs/getting-started.html)
### Webpack
[Webpack傻瓜式指南（一）](http://zhuanlan.zhihu.com/FrontendMagazine/20367175)
[Webpack傻瓜式指南（二）](http://zhuanlan.zhihu.com/FrontendMagazine/20397902)
### ES6
[ECMAScript 6入门](http://es6.ruanyifeng.com/)
### 前端工程化
[前端工程-基础篇](https://github.com/fouber/blog/issues/10)
[浅谈前端集成解决方案 #1](https://github.com/fouber/blog/issues/1)
[前端开发体系建设日记 #2](https://github.com/fouber/blog/issues/2)
[基于webpack搭建前端工程解决方案探索 #10](	https://github.com/chemdemo/chemdemo.github.io/issues/10)
### 扔掉jQuery
[You don’t need jQuery](https://github.com/oneuijs/You-Dont-Need-jQuery/blob/master/README.zh-CN.md)


