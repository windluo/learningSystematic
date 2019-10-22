## Node.js进阶话题

本章节一共16页，预计30分钟。

### 模块加载机制

加载模块直接使用 `require`即可。

Node.js 的模块分为两大类：核心模块和文件模块。

核心模块是 Node.js 标准 API 中提供的模块，如`fs`、`http`等。 我们可以直接通过 require 获取核心模块，例如 require('fs')。核心模块拥有最高的加载优先级，换言之如果有模块与其命名冲突， Node.js 总是会加载核心模块。 

文件模块则是存储为单独的文件（或文件夹）的模块，可能是 JavaScript 代码、JSON 或 编译好的 C/C++ 代码。

> JavaScript 文件，*.js
>
> Json 文件，*.json
>
> C/C++ 文件，*.node

`require`加载时，遇到`require('fs')`这种，会优先去` node_modules `文件夹查找。在找不到的情况下，会依次往上查找到根目录。

 Node.js 模块不会被重复加载， 是因为 Node.js 通过文件名缓存所有加载过的文件模块。

>  即使你分别通过 require('express') 和 require('./node_modules/express') 加载两次，也不会重复加载，因为尽管两次参数不同，解析到的文件却是同一个。 

### 控制流



Node.js 应用部署

Node.js 的不足之处