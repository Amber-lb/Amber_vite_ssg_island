# 框架初体验：环境搭建和功能概览

[toc]

这节课我们来进行一些编码操作，体验一下 Island.js 框架。本节课的目标主要有 2 个：

1. 安装好必要的工具和软件
2. 对框架功能进行概览，看看最后需要完成哪些功能

## 环境搭建

首先是环境搭建。你需要安装如下的软件/工具:

- Chrome 浏览器：[下载地址](https://www.google.com/chrome/)
- VSCode 编辑器：[下载地址](https://code.visualstudio.com/)
- Node.js 16 及以上版本：[下载地址](https://nodejs.org/en/)

> 在安装完 Node.js 之后，npm 命令也会被自动安装

当然，我也推荐你使用 pnpm 作为项目的包管理工具，相比于传统的 npm 和 yarn，pnpm 不仅安装速度明显更快，而且解决了依赖提升等安全问题（[戳链接了解详情](https://juejin.cn/post/6932046455733485575)）。

你可以直接通过 npm 来全局安装 pnpm：

> npm i -g pnpm

## 框架体验

首先，由于 npm 默认的镜像源在国外，安装速度可能会有些慢，我推荐你换成国内的镜像源，有两种方式可以用来切换。

> 镜像源：https://registry.npmmirror.com/

1. 项目目录新建 `.npmrc` 文件，写入如下内容：

```shell
registry=https://registry.npmmirror.com/
```

2. 终端执行命令：

```shell
npm config set registry https://registry.npmmirror.com/
```

前者的配置只会在当前项目生效，而后者会在全局生效，也就是在所有项目默认情况下生效。然后我们安装必要的依赖:

```shell
npnm i islandjs -S
```

在 package.json 中添加命令：

```json
{
    "scripts": {
        "dev": "island dev docs",
        "build": "island build docs",
        "preview": "island start docs"
    },
}
```

接着可以新建 `docs/.island/config.ts` 配置文件：

```ts
import { defineConfig } from "islandjs";

export default defineConfig({
    themeConfig: {
        nav: [
            {
                text: "Home",
                link: "/",
            },
        ],
        sidebar: {
            "/": [
                {
                    text: "文章列表",
                    items: [
                        {
                            text: "Fresh",
                            link: "/article/fresh",
                        },
                        {
                            text: "Astro",
                            link: "/article/astro",
                        },
                    ],
                },
            ],
        },
    },
});
```
这样就配置好了站点的导航栏和侧边栏，当然你需要**添加对应的文件**，你可以在 [这里](https://github.com/sanyuan0704/island-template/tree/master/docs/article) 获取文件内容。

接着通过 `pnpm dev` 启动项目，访问 `http://localhost:5173/article/fresh`，可以看到如下的效果：

![](/assets/amber-4.webp)

滑动页面后你可以发现右边目录的锚点也跟着变化：

![](/assets/amber-5.webp)

点击不同的锚点也能够跳转到对应的文章区域。

同时点击搜索框，或者按下`Command + K`，搜索框会进入输入状态，输入内容后自动出现搜索结果:

![](/assets/amber-6.webp)

尝试修改文件内容，可以发现热更新是可以正常生效的。然后我们来添加首页内容，新建docs/index.md:

```md
---
pageType: home

hero:
  name: Island.js 模板
  text: React SSG 文档站
  tagline: 提供 React SSG 文档站模板
  image:
    src: /logo.png
    alt: Note
  actions:
    - theme: brand
      text: 点击查看
      link: /article/fresh
    - theme: alt
      text: GitHub
      link: https://github.com/sanyuan0704/island.js
features:
  - title: Feature 1
    details: Feature 1 的详细内容
    icon: 🪐
  - title: Feature 2
    details: Feature 2 的详细内容
    icon: 🧑🏻‍💻
  - title: Feature 3
    details: Feature 3 的详细内容
    icon: 🏃‍♂️
---
```

然后直接访问根路由(`/`)，就可以看到首页的效果出现了，并且点击按钮也能够正常地进行页面跳转，但还存在一个问题：图片资源并不能显示。

这是因为当前的图片路径 `/logo.png` 并不存在，我们可以新建 `docs/public` 目录，在里面加入 `logo.png` 图片，接着框架会自动在 `public` 目录寻找 `logo.png`，然后进行展示。首页效果如下所示：

![](/assets/amber-7.webp)

最后，我们来打包这个项目，你可以执行 `pnpm build` 来进行打包，产物会默认放到 `docs/.island/dist` 中。打包完后，你可以使用 `pnpm preview` 命令预览产物，这样你可以看到如下的命令行信息：

```shell
> island start docs

Built site served at http://localhost:4173/
```
进入 `http://localhost:4173/` 后，你就可以查看生产环境下的页面内容了。那到目前为止，一个最基础的项目模板就被我们搭建起来，你可以根据当前的配置进行扩展，添加更丰富的内容。

你应该也注意到了，从头到尾我们并没有编写一行前端组件或者样式的代码，仅仅通过少量的配置 + Markdown 内容，就把完整的页面呈现出来了，非常简单，相信你也能从中感受到框架的强大之处。

