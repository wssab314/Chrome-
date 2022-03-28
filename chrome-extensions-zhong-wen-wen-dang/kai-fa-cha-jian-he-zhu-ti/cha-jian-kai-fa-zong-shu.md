# 🐨 插件开发综述

阅读完第一次的指导和综述之后，可以使用这篇文章作为梗概，使用Manifest v3所提供的支持来进行插件的开发了。我们鼓励开发者扩展和探索插件的无限可能。

| 用户界面接口         | 描述               |
| -------------- | ---------------- |
| Action         | 控制插件在工具栏中的展示样式   |
| Commands       | 添加键盘快捷键来触发功能     |
| Context Menus  | 为右键菜单添加功能        |
| Omnibox        | 给地址栏中添加关键字功能     |
| Override Pages | 创建新选项卡、书签页面或历史页面 |
| Page Actions   | 动态的在工具栏中展示图标     |

| 功能性接口                | 描述                 |
| -------------------- | ------------------ |
| Accessibility (a11y) | 为残障人士提供无障碍功能的接口    |
| Service Workers      | 有事件发生时，检测它并作出反映    |
| Internationalization | 使用特定的语言工作          |
| Identity             | 获取OAuth2访问token    |
| Management           | 管理已安装插件和正在运行的插件    |
| Message Passing      | content脚本和父插件的双向通信 |
| Options Pages        | 选项页，让用户对他们的插件进行设置  |
| Permissions          | 确认插件的权限            |
| Storage              | 存储和检索数据            |

| 修改和监视Chrome浏览器 | 描述                           |
| -------------- | ---------------------------- |
| Bookmarks      | 创建、组织和操作书签                   |
| Browsing Data  | 从用户的本地数据中删除浏览记录              |
| Downloads      | 以编程方式启动下载、监视下载、进行西在操作和搜索下载结果 |
| Font Settings  | 管理chrome的字体设置                |
|                |                              |
