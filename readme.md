# debug [![explain]][source] [![translate-svg]][translate-list]

[explain]: http://llever.com/explain.svg
[source]: https://github.com/chinanf-boy/Source-Explain
[translate-svg]: http://llever.com/translate.svg
[translate-list]: https://github.com/chinanf-boy/chinese-translate-list

「 一个微小的JavaScript调试工具,以Node.js核心的调试技术为模型. 适用于Node.js和Web浏览器.  」

---

## 校对 ✅

<!-- doc-templite START generated -->
<!-- repo = 'visionmedia/debug' -->
<!-- commit = '207a6a2d53507ec9dd57c94c46cc7d3dd272306d' -->
<!-- time = '2018 7.26' -->
翻译的原文 | 与日期 | 最新更新 | 更多
---|---|---|---
[commit] | ⏰ 2018 7.26 | ![last] | [中文翻译][translate-list]

[last]: https://img.shields.io/github/last-commit/visionmedia/debug.svg
[commit]: https://github.com/visionmedia/debug/tree/207a6a2d53507ec9dd57c94c46cc7d3dd272306d

<!-- doc-templite END generated -->

### 贡献

欢迎 👏 勘误/校对/更新贡献 😊 [具体贡献请看](https://github.com/chinanf-boy/chinese-translate-list#贡献)

## 生活

[help me live , live need money 💰](https://github.com/chinanf-boy/live-need-money)

---

### 目录

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [调试](#%E8%B0%83%E8%AF%95)
  - [安装](#%E5%AE%89%E8%A3%85)
  - [用法](#%E7%94%A8%E6%B3%95)
      - [Windows命令提示符说明](#windows%E5%91%BD%E4%BB%A4%E6%8F%90%E7%A4%BA%E7%AC%A6%E8%AF%B4%E6%98%8E)
        - [CMD](#cmd)
        - [PowerShell (VS Code 默认)](#powershell-vs-code-%E9%BB%98%E8%AE%A4)
  - [命名空间颜色](#%E5%91%BD%E5%90%8D%E7%A9%BA%E9%97%B4%E9%A2%9C%E8%89%B2)
      - [Node.js](#nodejs)
      - [网页浏览器](#%E7%BD%91%E9%A1%B5%E6%B5%8F%E8%A7%88%E5%99%A8)
  - [毫秒差异](#%E6%AF%AB%E7%A7%92%E5%B7%AE%E5%BC%82)
  - [约定](#%E7%BA%A6%E5%AE%9A)
  - [通配符](#%E9%80%9A%E9%85%8D%E7%AC%A6)
  - [环境变量](#%E7%8E%AF%E5%A2%83%E5%8F%98%E9%87%8F)
  - [格式化程序](#%E6%A0%BC%E5%BC%8F%E5%8C%96%E7%A8%8B%E5%BA%8F)
    - [自定义格式化程序](#%E8%87%AA%E5%AE%9A%E4%B9%89%E6%A0%BC%E5%BC%8F%E5%8C%96%E7%A8%8B%E5%BA%8F)
  - [浏览器支持](#%E6%B5%8F%E8%A7%88%E5%99%A8%E6%94%AF%E6%8C%81)
  - [输出流](#%E8%BE%93%E5%87%BA%E6%B5%81)
  - [动态设置](#%E5%8A%A8%E6%80%81%E8%AE%BE%E7%BD%AE)
  - [检查debug目标是否可用](#%E6%A3%80%E6%9F%A5debug%E7%9B%AE%E6%A0%87%E6%98%AF%E5%90%A6%E5%8F%AF%E7%94%A8)
  - [作者](#%E4%BD%9C%E8%80%85)
  - [支持者](#%E6%94%AF%E6%8C%81%E8%80%85)
  - [赞助商](#%E8%B5%9E%E5%8A%A9%E5%95%86)
  - [License](#license)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->


# 调试

[![Build Status](https://travis-ci.org/visionmedia/debug.svg?branch=master)](https://travis-ci.org/visionmedia/debug)  [![Coverage Status](https://coveralls.io/repos/github/visionmedia/debug/badge.svg?branch=master)](https://coveralls.io/github/visionmedia/debug?branch=master)  [![Slack](https://visionmedia-community-slackin.now.sh/badge.svg)](https://visionmedia-community-slackin.now.sh/) [![OpenCollective](https://opencollective.com/debug/backers/badge.svg)](#backers)
[![OpenCollective](https://opencollective.com/debug/sponsors/badge.svg)](#sponsors)

<img width="647" src="https://user-images.githubusercontent.com/71256/29091486-fa38524c-7c37-11e7-895f-e7ec8e1039b6.png">

一个微小的JavaScript调试工具,以Node.js核心的调试技术为模型. 适用于Node.js和Web浏览器. 

## 安装

```bash
$ npm install debug
```

## 用法

`debug`导出一个函数; 只需传递模块的名称给函数,它将返回一个装饰了的`console.error`, 来为您传递调试语句. 这将允许您看出与调试 模块的不同部分 或者 整个模块的调试输出. 

例[*app.js*](./examples/node/app.js): 

```js
var debug = require('debug')('http')
  , http = require('http')
  , name = 'My App';

// 一个假的示例

debug('booting %o', name);

http.createServer(function(req, res){
  debug(req.method + ' ' + req.url);
  res.end('hello\n');
}).listen(3000, function(){
  debug('listening');
});

// 一个假的工作中不同流程

require('./worker');
```

例[*worker.js*](./examples/node/worker.js): 

```js
var a = require('debug')('worker:a')
  , b = require('debug')('worker:b');

function work() {
  a('doing lots of uninteresting work');
  setTimeout(work, Math.random() * 1000);
}

work();

function workb() {
  b('doing some work');
  setTimeout(workb, Math.random() * 2000);
}

workb();
```

喔 然后用`DEBUG`环境变量启用它们. `根据空格或逗号分隔的名称`

这里有些例子: 

<img width="647" alt="screen shot 2017-08-08 at 12 53 04 pm" src="https://user-images.githubusercontent.com/71256/29091703-a6302cdc-7c38-11e7-8304-7c0b3bc600cd.png">
<img width="647" alt="screen shot 2017-08-08 at 12 53 38 pm" src="https://user-images.githubusercontent.com/71256/29091700-a62a6888-7c38-11e7-800b-db911291ca2b.png">
<img width="647" alt="screen shot 2017-08-08 at 12 53 25 pm" src="https://user-images.githubusercontent.com/71256/29091701-a62ea114-7c38-11e7-826a-2692bedca740.png">

#### Windows命令提示符说明

##### CMD

在Windows上,使用. 设置环境变量`set`命令. 

```cmd
set DEBUG=*,-not_this
```

例: 

```cmd
set DEBUG=* & node app.js
```

##### PowerShell (VS Code 默认) 

PowerShell使用 以下的语法 来设置环境变量. 

```cmd
$env:DEBUG = "*,-not_this"
```

例: 

```cmd
$env:DEBUG='app';node app.js
```

然后,像往常一样运行要调试的程序. 

npm脚本示例: 

```js
  "windowsDebug": "@powershell -Command $env:DEBUG='*';node app.js",
```

## 命名空间颜色

每个调试实例都根据 其空间名称生成颜色. 这有助于可视化解析,可以在调试输出时,识别调试行属于哪个调试实例. 

#### Node.js

在Node.js中,当stderr是TTY时启用颜色. 你也*能*安装[`supports-color`](https://npmjs.org/supports-color)模块帮助调试,否则调试只会使用 少量的基本颜色. 

<img width="521" src="https://user-images.githubusercontent.com/71256/29092181-47f6a9e6-7c3a-11e7-9a14-1928d8a711cd.png">

#### 网页浏览器

"Web 调试器"也可以使用颜色来理解`%c`格式化选项. 这些是WebKit 调试,Firefox ([从版本31开始](https://hacks.mozilla.org/2014/05/editable-box-model-multiple-selection-sublime-text-keys-much-more-firefox-developer-tools-episode-31/)) 和 Firefox的插件 (任何版本) . 

<img width="524" src="https://user-images.githubusercontent.com/71256/29092033-b65f9f2e-7c39-11e7-8e32-f6f0d8e865c1.png">

## 毫秒差异

在积极开发应用程序时,查看在一个应用程序运行`debug()`和下一个之间花费的时间可能很有用. 例如,假设您在请求资源之前调用`debug()`,"+ NNNms"将显示两次调用之间花费了多少时间. 

<img width="647" src="https://user-images.githubusercontent.com/71256/29091486-fa38524c-7c37-11e7-895f-e7ec8e1039b6.png">

当 stdout 不是 TTY 时, `Date#toISOString()`使用,使其更有用于记录调试信息,如下所示: 

<img width="647" src="https://user-images.githubusercontent.com/71256/29091956-6bd78372-7c39-11e7-8c55-c948396d6edd.png">

## 约定

如果您在一个或多个库中使用它,那么您*应该*使用库的名称,以便开发人员可以根据需要切换调试,而无需猜测名称. 如果你有多个debug,那么*应该*使用您的库名称作为前缀,并使用":"分隔不同调试. 例如,来自 Connect 的"bodyParser"将是"connect:bodyParser". 如果在名称的末尾添加"\*",则无论 DEBUG环境变量 的设置如何,都将始终启用它. 然后,您可以将其用于 正常输出以及调试输出. 

## 通配符

该`*`字符可以用作通配符. 假设您的库具有名为`connect:bodyParser`,`connect:compress`,`connect:session`的调试器,而不是列出所有三个`DEBUG=connect:bodyParser,connect:compress,connect:session`,你可能只是这样做`DEBUG=connect:*`,或只需使用`DEBUG=*`运行一切. 

您还可以通过在前面加上 `-` 字符来排除特定的调试器. 例如,`DEBUG=*,-connect:*`将包括除`connect:`之外的所有调试器. 

## 环境变量

在运行Node.js时,您可以设置一些环境变量来改变调试日志记录的行为: 

| 名称                  | 目的                   |
| ------------------- | -------------------- |
| `DEBUG`             | 启用/禁用特定的调试命名空间.      |
| `DEBUG_HIDE_DATE`   | 从调试输出中隐藏日期 (非TTY) .  |
| `DEBUG_COLORS`      | 是否在调试输出中使用颜色.        |
| `DEBUG_DEPTH`       | 调试深度.              |
| `DEBUG_SHOW_HIDDEN` | 显示已检查对象的隐藏属性.        |

**注意:**环境变量以`DEBUG_`开头,最终被转换为Options对象，经过`%o`/`%O`格式化使用. 请参阅Node.js文档[`util.inspect()`](https://nodejs.org/api/util.html#util_util_inspect_object_options)完整列表. 

## 格式化程序

调试使用[printf-style](https://wikipedia.org/wiki/Printf_format_string)格式. 以下是官方支持的格式化程序: 

| 格式化  | 表示                            |
| ---- | ----------------------------- |
| `%O` | 在多行上打印一个对象.                   |
| `%o` | 在一行上打印一个对象.                   |
| `%s` | 字符串.                            |
| `%d` | 数字 (整数和浮点数) .                 |
| `%j` | JSON. 替换为字符串'[圆]'如果参数包含循环引用.  |
| `%%` | 单个百分号 ('%') . 这不会消耗参数.        |

### 自定义格式化程序

您可以扩展自定义格式化程序, 通过添加`debug.formatters`. 例如,如果您想添加`%h`来支持  Buffer 作为 hex 进行渲染,你可以这样做: 

```js
const createDebug = require('debug')
createDebug.formatters.h = (v) => {
  return v.toString('hex')
}

// …elsewhere
const debug = createDebug('foo')
debug('this is hex: %h', new Buffer('hello world'))
//   foo this is hex: 68656c6c6f20776f726c6421 +0ms
```

## 浏览器支持

您可以使用构建 用于浏览器的脚本,通过使用[browserify](https://github.com/substack/node-browserify),或者只是使用[browserify-as-a-service](https://wzrd.in/) [build](https://wzrd.in/standalone/debug@latest),如果你不想自己建造它. 

Debug的启用状态目前已经`localStorage`持久化. 考虑下面显示的情况`worker:a`和`worker:b`,并希望调试两者. 您可以使用此函数`localStorage.debug`: 

```js
localStorage.debug = 'worker:*'
```

然后刷新页面. 

```js
a = debug('worker:a');
b = debug('worker:b');

setInterval(function(){
  a('doing some work');
}, 1000);

setInterval(function(){
  b('doing some work');
}, 1200);
```

## 输出流

默认`debug`将记录日志在stderr,但是可以通过覆盖它,来为每个命名空间配置`log`方法: 

例[*stdout.js*](./examples/node/stdout.js): 

```js
var debug = require('debug');
var error = debug('app:error');

// 默认 使用 stderr 
error('goes to stderr!');

var log = debug('app:log');
// 设置 命名空间 记录日志 通过 console.log
log.log = console.log.bind(console); // 不要忘了绑定 console!
log('goes to stdout');
error('still goes to stderr!');

// 设置所有都通过 console.info 记录
// overrides all per-namespace log settings
debug.log = console.info.bind(console);
error('now goes to stdout via console.info');
log('still goes to stdout, but via console.info now');
```

## 动态设置

您也可以通过调用`enable()`方法来自我启用调试 : 

```js
let debug = require('debug');

console.log(1, debug.enabled('test')); // 没启动

debug.enable('test'); // 启动
console.log(2, debug.enabled('test')); // ok 

debug.disable(); // 关了
console.log(3, debug.enabled('test')); // no
```

打印: 

    1 false
    2 true
    3 false

用法: \
`enable(namespaces)`\
`namespaces`可以包括由冒号和通配符分隔的模式. 

注意`enable()`会完全覆盖 DEBUG 变量 :

    $ DEBUG=foo node -e 'var dbg = require("debug"); dbg.enable("bar"); console.log(dbg.enabled("foo"))'
    => false

## 检查debug目标是否可用

在你创建了debug实例, 你可以通过`enabled`属性, 来确定调试是否运行了:

```javascript
const debug = require('debug')('http');

if (debug.enabled) {
  // do stuff...
}
```

当然,您还可以手动切换此属性以强制启用或禁用调试实例.

## 作者

-   TJ Holowaychuk
-   Nathan Rajlich
-   Andrew Rhyne

## 支持者

Support us with a monthly donation and help us continue our activities🥄 \[[Become a backer](https://opencollective.com/debug#backer)]

<a href="https://opencollective.com/debug/backer/0/website" target="_blank"><img src="https://opencollective.com/debug/backer/0/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/1/website" target="_blank"><img src="https://opencollective.com/debug/backer/1/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/2/website" target="_blank"><img src="https://opencollective.com/debug/backer/2/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/3/website" target="_blank"><img src="https://opencollective.com/debug/backer/3/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/4/website" target="_blank"><img src="https://opencollective.com/debug/backer/4/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/5/website" target="_blank"><img src="https://opencollective.com/debug/backer/5/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/6/website" target="_blank"><img src="https://opencollective.com/debug/backer/6/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/7/website" target="_blank"><img src="https://opencollective.com/debug/backer/7/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/8/website" target="_blank"><img src="https://opencollective.com/debug/backer/8/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/9/website" target="_blank"><img src="https://opencollective.com/debug/backer/9/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/10/website" target="_blank"><img src="https://opencollective.com/debug/backer/10/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/11/website" target="_blank"><img src="https://opencollective.com/debug/backer/11/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/12/website" target="_blank"><img src="https://opencollective.com/debug/backer/12/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/13/website" target="_blank"><img src="https://opencollective.com/debug/backer/13/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/14/website" target="_blank"><img src="https://opencollective.com/debug/backer/14/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/15/website" target="_blank"><img src="https://opencollective.com/debug/backer/15/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/16/website" target="_blank"><img src="https://opencollective.com/debug/backer/16/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/17/website" target="_blank"><img src="https://opencollective.com/debug/backer/17/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/18/website" target="_blank"><img src="https://opencollective.com/debug/backer/18/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/19/website" target="_blank"><img src="https://opencollective.com/debug/backer/19/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/20/website" target="_blank"><img src="https://opencollective.com/debug/backer/20/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/21/website" target="_blank"><img src="https://opencollective.com/debug/backer/21/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/22/website" target="_blank"><img src="https://opencollective.com/debug/backer/22/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/23/website" target="_blank"><img src="https://opencollective.com/debug/backer/23/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/24/website" target="_blank"><img src="https://opencollective.com/debug/backer/24/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/25/website" target="_blank"><img src="https://opencollective.com/debug/backer/25/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/26/website" target="_blank"><img src="https://opencollective.com/debug/backer/26/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/27/website" target="_blank"><img src="https://opencollective.com/debug/backer/27/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/28/website" target="_blank"><img src="https://opencollective.com/debug/backer/28/avatar.svg"></a>
<a href="https://opencollective.com/debug/backer/29/website" target="_blank"><img src="https://opencollective.com/debug/backer/29/avatar.svg"></a>

## 赞助商

Become a sponsor and get your logo on our README on Github with a link to your site🥄 \[[Become a sponsor](https://opencollective.com/debug#sponsor)]

<a href="https://opencollective.com/debug/sponsor/0/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/0/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/1/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/1/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/2/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/2/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/3/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/3/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/4/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/4/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/5/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/5/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/6/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/6/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/7/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/7/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/8/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/8/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/9/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/9/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/10/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/10/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/11/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/11/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/12/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/12/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/13/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/13/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/14/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/14/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/15/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/15/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/16/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/16/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/17/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/17/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/18/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/18/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/19/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/19/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/20/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/20/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/21/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/21/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/22/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/22/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/23/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/23/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/24/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/24/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/25/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/25/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/26/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/26/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/27/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/27/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/28/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/28/avatar.svg"></a>
<a href="https://opencollective.com/debug/sponsor/29/website" target="_blank"><img src="https://opencollective.com/debug/sponsor/29/avatar.svg"></a>

## License

<details>

<summary> MIT > 详细🔎 </summary>


(The MIT License)

Copyright (c) 2014-2017 TJ Holowaychuk\<tj@vision-media.ca>

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the 'Software'), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT🥄 IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

</details>