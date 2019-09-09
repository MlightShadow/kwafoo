# kwafoo

> An electron-vue project

一个不需要太多计算机知识就可以使用的爬虫工具

以下为近阶段开发目标

* [ ] 单页下载: 获取单个网页中的所有静态资源, 供用户选择下载
* [ ] 多页下载: 根据xpath获取资源, 并指定下一页的xpath继续执行, 直到某个深度结束
* [ ] 用户注册: 轻度联网功能前置
* [ ] 配置脚本: 可以保存自己对某个网页爬取方式的配置脚本信息
* [ ] 脚本市场: 可以查看其它人发布的网页爬取配置脚本

## Build Setup

``` bash
# install dependencies
npm install

# serve with hot reload at localhost:9080
npm run dev

# build electron application for production
npm run build


# lint all JS/Vue component files in `src/`
npm run lint

```

---

This project was generated with [electron-vue](https://github.com/SimulatedGREG/electron-vue)@[8fae476](https://github.com/SimulatedGREG/electron-vue/tree/8fae4763e9d225d3691b627e83b9e09b56f6c935) using [vue-cli](https://github.com/vuejs/vue-cli). Documentation about the original structure can be found [here](https://simulatedgreg.gitbooks.io/electron-vue/content/index.html).
