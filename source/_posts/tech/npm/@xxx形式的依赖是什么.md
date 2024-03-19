---
title: 类似@xxx的依赖是什么？
date: 2024-03-19 13:48:35
tags: ['npm', '前端工程化']
categories: ['npm']
---
# 为啥会有@xxx格式的npm包

&ensp;&ensp;事情还得从一个开源的[聊天室项目](https://github.com/yinxin630/fiora)说起。
&ensp;&ensp;在拜读前辈的聊天应用源码时，发现源码本身根按照前后端、数据库、命令行工具、手机app等划分了几个项目。其中一部分又存在依赖关系，例如[server端](https://github.com/yinxin630/fiora/blob/master/packages/server/package.json#L11)的项目同时依赖于database、bin，但是在package.json中的依赖是以
```json
"dependencies": {
  "@fiora/bin": "^1.0.0"
}
```
形式引入。其实像上面提到的`@xxx/xx`这样诡异的依赖，工作中这种格式的包并不少见，比如`@babel/runtime`、`@rollup/plugin-terser`还有`@typescript-eslint/parser`，但是后者在我的狗脑子看来无非就是人家开发组织庞大，实力雄厚，取些标新立异的依赖名秀肌肉而已，很正常。但是凭直觉判断`@fiora/bin`这个包应该不会在npm仓库中，所以把这个问题抛给了Claude, 从它给的解释才知道依赖项名称为`@xxx`的包的含义。

## 有哪些情况会出现这种格式的包

废话少说，直接下结论：
对于以`@xxx/xx`形式命名的依赖，有以下几种情况：
- `@组织/用户名/包名`：一些组织/个人会在npm上发布自己开发的包，但是可能会重名，所以可以在npm注册一个自己名字的作用域防止取名冲突。
- `@项目名/模块名`：一些大型、支持扩展、社区活跃的项目通常会把重要的代码分模块发布出去，就像上面的babel、rollup那几个包，这些包的命名通常跟项目和所属模块相关。
- `@公司名/包名`：一些公司会自己维护一个内部的npm仓库，所以不同公司会有不同的包命名约定，其中一部分可能会以`@公司/团队名/包名`格式避免内部包名冲突。
- `本地连接的包`: 一些项目会以多包仓库的形式管理代码，当每个包之间需要互相依赖时为了不再重复下载依赖、不用重复配置、易于管理等，所以有了**Yarn Workspace** + **monorepo** + `@仓库名/项目名`方案。

## **monorepo** 和 **lerna** 和 **Yarn Workspace**

[一定一定要去看！！！](https://zhuanlan.zhihu.com/p/350317373?utm_id=0)

关于这一块，我自认为没有大神讲的好，大家可以**移步到上面的链接**看看这部分内容，深入浅出，受益匪浅！

下面是个人的理解。

经常维护多项目仓库的小伙伴们都知道，同一个包可能被多个项目依赖，但是在仓库没有脚手架的时候只能每个项目里分别单独安装这个包，很消耗磁盘空间。另外项目与项目间需要依赖时也只能打包到另一个本地项目里，非~ 常~ 难~ 受~，所以有了monorepo这个概念。
- 一个项目放一个仓库，同时管理多个仓库。这叫multi-repo
- **很多个项目放在一个仓库，只需要维护一个git仓库，里面的每个项目叫做工作空间/包。这叫mono-repo。**

multi-repo方式对于多模块项目不够友好。对于一般的项目/团队来说，multi-repo方式简单直白而且也够用了。可对于同一部门/同一业务/同一工具链/主体-扩展系列的项目集而言有很多弊端（调试、相互依赖等）。标题中提到的方法就提供了这种在一个库里构建项目的能力，让我们不用将项目发布到远程注册表来解决弊端。同时这种能力使得直接使用`npm link`方法变得过时。

lerna和yarn workspaces就是这样的方法，具体来说lerna比yarn workspaces还先出，yarn workspaces只是近几年才支持的，npm workspaces好像更靠后才支持（目前已支持）。

### 配置mono-repo的不同方法

1. npm init
这种方法已经被淘汰了，我也没有继续深入，古老低效并且有上位替代的技术可以不用特别关注~
2. lerna + npm
这里参考前面大佬的文章即可。我这里提一点注意事项：
lerna官方在7.0.0版本废弃了`lerna bootstrap`等指令，具体背景见[链接](https://lerna.js.org/docs/legacy-package-management)。所以原文中从设置npm脚本指令`bootstrap`这步(包括这步)开始按如下方法进行：
    - 确定自己安装的lerna版本高于7.0.0，低于7.0.0按原文来。
    - 在**根目录**下的`package.json`文件中设置`workspaces`字段，把你仓库下的所有项目的相对路径都加上：
```json
{
  "workspaces": ["packages/a", "packages/b"] // 子项目到根目录的相对路径
}
```
    - 如果要在子项目a中添加新的依赖，可用下面的指令：
```sh
npm install < 你的依赖名 > -w < 子项目名 >
# 举例：安装外部依赖core-js给子项目a
npm install core-js -w a
# 举例：安装内部的本地子项目server给另一个子项目web
npm install server -w web
```

3. lerna + yarn
这部分跟2一样，只是看你更喜欢npm还是yarn，我这里一开始接触的npm，所以不举例了。
4. yarn workspaces
yarn工作空间内置了mono-repo功能，可以按照原文进行配置。
5. lerna + yarn workspaces
可见原文。




