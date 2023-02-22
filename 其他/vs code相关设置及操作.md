# vs code 相关设置及操作

## 插件

### 优化外观
| 名称                  | 作用                              |
| :-----------------------| :----------------------------- |
| Chinese                 | 中文                           |
| Bracket Pair Colorizer（内置） | 彩色括号                           |
| indent-rainbow                 | 彩色缩进                             |
| One Dark Pro                   | 主题                                 |
| vscode-icons                   | 文件类型图标                         |
| Indenticator                   | 出现白线展示缩进层级               |
| Better Comments                | 美化注释，根据种类显示不同颜色      |
| Highlight Matching Tag         | 高亮对应标签                     |
| Color Highlight                | 颜色高亮（默认是以背景色展示，设置里marker type可以更改展示方式） |
| Material Icon Theme            | 设置文件图标                |
| Error Gutters                  | 大红波浪线提示报错              |

### 功能扩展
|名称					|作用					|
|:-------------------|:--------------------|
|会了吧 			   |可以分析英语单词		|
|Translation 		 |翻译  				  |
|Image preview 		 |查看图片				 |
|open in browser 	 |浏览器中打开			|
|Live Server		 | 实时预览				 |
|Easy LESS 			 |编译 less 文件		 |
|Svg Preview 		 |查看 svg 图片          |
|Code Runner 		 |可以在编辑器查看运行结果  |
|REST Client |可以在编辑器里轻量化发送网络请求（新建一个.http文件，然后 GET http://127.0.0.1/test，点击上方发送请求，携带数据的话空一行，然后直接写对象{name:'zs'}，请求头也可以添加）不同请求之间使用###分隔 |
|Intellicode API Usage snippets|为原生方法提供示例教学（类似菜鸟教程）|
| Nested Comments   |嵌套注释    |
| Template String Converter  | 字符串输入$直接转为模板字符串|
| es6-string-html | js中模板字符串高 （前面需加上/*html*/） |


### 提升效率

|名称						 |作用								|
|:------------------------|------------------------------------|
|Auto Rename Tag 		  |自动重命名标签		  				  |
|Auto Import	 		  | 自动导入							|
|Auto Close Tag（内置）  		  |自动闭合标签名		  				  |
|CSS Peek 		 		  |点击类名迅速定位到样式(有时会失效)		|
|change-case 	 		  |快速切换变量格式（大驼峰、小驼峰、下划线）|
|Path Intellisense 		  |自动补全引入文件路径					|
|Npm Intellisense  		  |导入 npm 包时智能提示				 |
|px to rem & rpx (cssrem) |自动换算单位						   |

### 代码片段类
**(使用几个字符的简写，就可以敲出整段代码)**
|名称								  |语言或者框架			|
|:---------------------------------|:--------------------|
|JavaScript (ES6) code snippets    |js(ES6)				 |
|HTML CSS Support 				   |css					 |
|HTML Snippets（vscode内置） 					   |html				 |
|Vue VSCode Snippets               |vue  				 |

### 代码格式化
|名称		 |说明			   |
|:-------:|:-----------------:|
|Prettier |代码格式化(具体要配置)|
|ESLint   |具体要配置		   |
|vetur    |vue 格式化			|
|vue language features |vue格式化（vue3）	|


## 自动保存

 文件—自动保存 点击

## 自带格式化

 设置—文本编辑器—格式化—Format On Paste 和 Format On Save 打上对勾

## 快捷键
**（部分快捷键可能和其他软件冲突）**
|快捷键					 |作用					|
|:------------------------:|:---------------------:|
|shift + alt +↑ (↓) 	   |快速复制一行			   |
|ctrl + d 				   |选中多个相同单词		  |
|ctrl + alt + ↑ (↓) 	   |添加多个光标（在相同的位置）|
|alt + 鼠标左键点击 		   |可以在想要的地方添加光标    |
|鼠标中键按住滑动 			  |可以在相同的位置添加光标   |
|ctrl + g 				   |快速跳转到某一行（指定行号）|
|shift + alt + 鼠标左键拖动  |选中某一块区域			   |
|ctrl + +/- 			   |放大缩小整个编辑器界面		|
|ctrl + / 				   |单行注释			     |
|shift + alt + a 		   |多行注释			     |
|alt + ↑ (↓) 			   |该行文本位置上移或下移	    |
|ctrl + [ (]) 			   |减少（增加）缩进	      |
|shift + alt + f 		   |格式化文档				|
|ctrl + shift + k 		   |删除该行				|
|ctrl + alt + ← (→) 	   |将编辑器移动到上（下）一组 |
|ctrl + k ctrl + ← (→)     |聚焦到左（右）侧编辑器组   |
|shift + alt + r 		   |在文件管理器中显示文件	    |
|ctrl + shift + [ (]) 	   |折叠（展开）该块代码		|
|ctrl + k ctrl + 0 		   |全部折叠				|
|ctrl + k ctrl + shift + \ |在该组内拆分（合并）成 2 个|