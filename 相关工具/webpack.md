# webpack

## webpack打包流程

Webpack的运行流程是一个串行的过程，从启动到结束依次执行以下流程：

1. 初始化：启动构建，读取与合并配置参数，加载 Plugin，实例化 Compiler。
2. 编译：从 Entry 发出，针对每个 Module 串行调用对应的 Loader 去翻译文件内容，再找到该 Module 依赖的 Module，递归地进行编译处理。
3. 输出：对编译后的 Module 组合成 Chunk，把 Chunk 转换成文件，输出到文件系统。

如果只执行一次构建，以上阶段将会按照顺序各执行一次。但在开启监听模式下，流程将变为如下：

![webpack打包流程](image/webpack打包流程.png)

下面具体介绍一下webpack的三个大阶段具体的小步。

初始化阶段

初始化阶段大致分为：

- 合并shell和**配置文件文件**的参数并且**实例化Complier对象**。
- **加载插件**
- **处理入口**

| 事件名          | 解释                                                         |
| --------------- | ------------------------------------------------------------ |
| 初始化参数      | 从配置文件和 Shell 语句中读取与合并参数，得出最终的参数。 这个过程中还会执行配置文件中的插件实例化语句 new Plugin()。 |
| 实例化 Compiler | 用上一步得到的参数初始化Compiler实例，Compiler负责文件监听和启动编译。Compiler实例中包含了完整的Webpack配置，全局只有一个Compiler实例。 |
| 加载插件        | 依次调用插件的apply方法，让插件可以监听后续的所有事件节点。同时给插件传入compiler实例的引用，以方便插件通过compiler调用Webpack提供的API。 |
| environment     | 开始应用Node.js风格的文件系统到compiler对象，以方便后续的文件寻找和读取。 |
| entry-option    | 读取配置的Entrys，为每个Entry实例化一个对应的EntryPlugin，为后面该Entry的递归解析工作做准备。 |
| after-plugins   | 调用完所有内置的和配置的插件的apply方法。                    |
| after-resolvers | 根据配置初始化完resolver，resolver负责在文件系统中寻找指定路径的文件。 |

编译阶段

| 事件名        | 解释                                                         |
| ------------- | ------------------------------------------------------------ |
| before-run    | 清除缓存                                                     |
| run           | 启动一次新的编译。                                           |
| watch-run     | 和run类似，区别在于它是在监听模式下启动的编译，在这个事件中可以获取到是哪些文件发生了变化导致**重新启动**一次新的编译。 |
| compile       | 该事件是为了告诉插件一次**新的**编译将要启动，同时会给插件带上compiler对象。 |
| compilation   | 当Webpack以开发模式运行时，每当检测到文件变化，一次新的Compilation将被创建。一个Compilation对象包含了当前的模块资源、编译生成资源、变化的文件等。Compilation对象也提供了很多事件回调供插件做扩展。 |
| make          | 一个新的Compilation创建完毕，即将从Entry开始读取文件，根据文件类型和配置的Loader对文件进行编译，编译完后再找出该文件依赖的文件，递归的编译和解析。 |
| after-compile | 一次Compilation执行完成。这里会根据编译结果 合并出我们最终生成的文件名和文件内容。 |
| invalid       | 当遇到文件不存在、文件编译错误等异常时会触发该事件，该事件不会导致Webpack退出。 |

这里主要最重要的就是compilation过程，compilation实际上就是调用相应的loader处理文件生成chunks并对这些chunks做优化的过程。几个关键的事件（Compilation对象this.hooks中）：

| 事件名               | 解释                                                         |
| -------------------- | ------------------------------------------------------------ |
| build-module         | 使用对应的Loader去转换一个模块。                             |
| normal-module-loader | 在用Loader对一个模块转换完后，使用acorn解析转换后的内容，输出对应的抽象语法树（AST），以方便Webpack后面对代码的分析。 |
| program              | 从配置的入口模块开始，分析其AST，当遇到require等导入其它模块语句时，便将其加入到**依赖的模块列表**，同时对新找出的**依赖模块递归分析**，最终搞清所有模块的**依赖关系**。 |
| seal                 | 所有模块及其**依赖**的模块都通过Loader转换完成后，根据依赖关系开始生成Chunk。 |

输出阶段

| 事件名      | 解释                                                         |
| ----------- | ------------------------------------------------------------ |
| should-emit | 所有需要输出的文件已经生成好，询问插件哪些文件需要输出，哪些不需要。 |
| emit        | 确定好要输出哪些文件后，执行文件输出，可以在这里获取和修改输出内容。 |
| after-emit  | 文件输出完毕。                                               |
| done        | 成功完成一次完成的编译和输出流程。                           |
| failed      | 如果在编译和输出流程中遇到异常导致Webpack退出时，就会直接跳转到本步骤，插件可以在本事件中获取到具体的错误原因。 |

## webpack有哪些阶段

1. webpack的准备阶段
2. modules和chunks的生成阶段
3. 文件生成阶段

## loader和plugin的不同

- loader能让webpack处理不同的文件，然后对文件进行一些处理，编译，压缩等，最终一起打包到指定文件中（比如loader可以将sass，less文件写法转换成css，而不需要在使用其他转换工具），loader本身是一个函数，接收源文件作为参数，返回转换的结果

   loader能把源文件经过转化后输出新的结果，一个loader遵循单一职责原则，只完成一种转换，然后链式的顺序去依次经过多个loader转换，直到得到最终结果并返回，所以在写loader时要保持其职责的单一性，同时webpack还提供了一些API供loader调用 

```js
// build/webpack.base.conf.js
module: {
    rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: vueLoaderConfig
      },
    ]
  },
```

- plugins则用于执行广泛的任务，从打包，优化，压缩，一直到重新定义环境中的变量，接口很强大，主要用来扩展webpack的功能，可以实现loader不能实现的更复杂的功能

```js
// build/webpack.prod.conf.js
plugins: [
    new webpack.DefinePlugin({
      'process.env': env
    }),
    new UglifyJsPlugin({
      uglifyOptions: {
        compress: {
          warnings: false
        }
      },
      sourceMap: config.build.productionSourceMap,
      parallel: true
    }),
]
```

## webpack如何配置单页和多页应用

单页应用即为webpack的标准模式，直接在entry中指定单页面应用的入口即可：

```
module.exports = {
  entry: {
    app: './src/main.js'
  },
}
```

多页面应用可以考虑使用webpack的`AutoWebPlugin`来完成简单的自动化构建，前提是项目目录结构要符合预先设定的规范

```
├── pages
│   ├── index
│   │   ├── index.css // 该页面单独需要的 CSS 样式
│   │   └── index.js // 该页面的入口文件
│   └── login
│       ├── index.css
│       └── index.js
├── common.css // 所有页面都需要的公共 CSS 样式
├── google_analytics.js
├── template.html
└── webpack.config.js
```

## webpack如何做到热更新

 webpack热更新（Hot Module Replacement），缩写为HMR，实现了不用刷新浏览器而将新变更的模块替换掉旧的模块，原理如下： 

![webpack热更新](image/webpack热更新.jpg)

server端和client端都做了处理：

- webpack监听到文件变化，重新编译打包，webpack-dev-server和webpack之间接口交互（主要是webpack-dev-middleware调用webpack暴露的API对代码进行监控，并告诉webpack将打包后代码保存到内存中）
- 通过sockjs（webpack-dev-server的依赖）在浏览器和服务器之间建立 一个`websocket`长连接，将webpack编译打包各阶段的信息告知浏览器端
- webpack根据 webpack-dev-server/client 传给它的信息以及 dev-server的配置决定是刷新浏览器还是进⾏模块热更新，如果是模块热更新继续执行，否者刷新浏览器
- HotModuleReplacement.runtime 是客户端 HMR 的中枢，它接收到上⼀步传递给他的新模块的 hash 值，它通过JsonpMainTemplate.runtime 向 server 端发送 Ajax 请求，服务端返回⼀个 json，该 json 包含了所有要更新的模块的 hash 值，获取到更新列表后，该模块再次通过 jsonp 请求，获取到最新的模块代码
- HotModulePlugin 将会对新旧模块进⾏对⽐，决定是否更新模块
- 当 HMR 失败后，回退到 live reload 操作，也就是进⾏浏览器刷新来获取最新打包代码

## 如何用webpack优化前端性能

- 压缩代码：比如利用UglifyJsPlugin来对js文件压缩
- CDN加速：将引用的静态资源修改为CDN上的路径。比如可以抽离出静态js，在index利用CDN引入；利⽤webpack对于 output 参数和各loader的publicPath参数来修改资源路径
- 删除Tree Shaking：将代码中永远不会走到的片段删除。可以通过在启动webpack时追加参数 `--optimize-minimize` 来实现
- 按照路由拆分代码，实现按需加载，提取公共代码
- 优化图片，对于小图可以使用 base64 的方式写入文件中

