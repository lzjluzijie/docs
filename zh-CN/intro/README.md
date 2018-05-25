---
name: 框架简介
---

# Macaron

Macaron 是一个具有高生产力和模块化设计的 Go Web 框架。框架秉承了 [Martini](https://github.com/go-martini/martini) 的基本思想，并在此基础上做出高级扩展。

[API 指南](https://gowalker.org/gopkg.in/macaron.v1)

Go 语言的最低版本要求为 **1.5**。

## 主要特性

- 支持子路由的强大路由设计
- 支持灵活多变的路由组合
- 支持无限路由组的无限嵌套
- 支持直接集成现有的服务
- 支持运行时动态设置需要渲染的模板集
- 支持使用内存文件作为静态资源和模板文件
- 支持对模块的轻松接入与解除
- 采用 [inject](https://github.com/codegangsta/inject) 提供的便利的依赖注入
- 采用更好的路由层和更少的反射来提升执行速度

## 使用案例

- [Gogs](http://gogs.io)：极易搭建的自助 Git 服务
- [Peach](http://peachdocs.org)：现代 Web 文档服务器
- [Go Walker](https://gowalker.org): Go 语言在线 API 文档
- [Switch](http://gopm.io)：Go 语言包管理
- [YouGam](http://yougam.com)：在线论坛
- [Critical Stack Intel](https://intel.criticalstack.com/)：A 100% free intel marketplace from Critical Stack, Inc.

## 快速导航

- 刚开始了解 Macaron 的话，不妨从 [开始使用](/docs/intro/getting_started) 看起。
- Macaron 已经拥有许多 [中间件](/docs/middlewares) 来简化您的工作。
- 如果您有任何问题，建议先从 [常见问题](/docs/faqs) 中寻找答案。
- 如果您觉得文档有描述得不够清楚之处，请通过 [提交工单](https://github.com/go-macaron/docs/issues) 告知我们。
