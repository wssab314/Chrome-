# 🐱 什么是插件？

这个页面将会对Chrome插件做一个简单的介绍，然后做一个hello world玩玩。

## 关于插件

插件是一个可以用来客制化浏览器体验的小程序。插件允许用户分离浏览器的功能和表现，用以不同的地方，譬如：

* 生产力工具
* 浓缩网页内容
* 信息聚合
* 娱乐和游戏

这些都是冰山一角，如果没有想法，可以去商店看看别人都做了些什么。

### 插件是如何运行的？

插件是基于HTML、CSS和JavaScript等web技术来实现的。不同的插件会独立运行在不同的沙盒环境中，不同的沙盒之间通过浏览器来进行联系。

插件的本质是允许我们使用chrome的api来增加一些浏览器的功能或直接访问web内容。插件是基于端用户的UI，即你所见的页面，和开发者接口来工作的：

#### 插件用户接口

这里为用户提供了统一管理插件的接口

#### 拓展API

插件接口可以被用来访问浏览器本身的功能：新建Tab、编辑网络请求等等。

去创建一个插件，你需要组装如下的一些资源信息 -- manifest文件、JavaScript、HTML、图像等等 -- 这些组成了插件。你可以以文件夹的形式加载到Chrome的开发者模式中来调试。如果你已经对自己的插件满意了，你可以将它打包发布到商店中去分发他。

### 用户如何获取一个插件？

大部分的用户是从插件商店中获取插件的。有世界各地的开发者上传自己的插件到商店中向公众开放。

{% hint style="info" %}
当你安装一些组织的插件时，他们使用的是企业协议。这些插件既可以从组织官网下载，也可以从Chrome商店下载。
{% endhint %}

### 一个关于插件协议的提示

在web应用商店的插件必须包含Chrome Web Store policies。在你开始之前你需要牢记如下几点：

* 一个插件必须要实现一个简单易懂的功能。一个插件可以包含多个组件和各种各样的功能，这些功能都是为了一个共同的目的。

![](<../../.gitbook/assets/image (4).png>)

* 用户交互页面需要做到小且有目的性。可以是一个小的图标，比如上面的，点击之后就会出现下面这样一个带有列表的新窗口。

![](<../../.gitbook/assets/image (5).png>)

## Hello World

这样一个小实例可以帮助你快速上手。首先创建一个空文件夹。

添加一个manifest.json文件，加入如下代码：

```json
{
  "name": "Hello Extensions",
  "description": "Base Level Extension",
  "version": "1.0",
  "manifest_version": 3
}
```

每个插件都需要一个manifest，尽管大多数插件不会对manifest做太多修改。在本次实例中，manifest中有一个popup页面和一些icon的声明。

```json
{
  "name": "Hello Extensions",
  "description": "Base Level Extension",
  "version": "1.0",
  "manifest_version": 3,
  "action": {
    "default_popup": "hello.html",
    "default_icon": "hello_extensions.png"
  }
}
```

从链接下载[hello\_extension.png](https://storage.googleapis.com/web-dev-uploads/image/WlD8wC6g8khYWPJUsQceQkhXSlv1/gmKIT88Ha1z8VBMJFOOH.png)文件，并在根目录下创建hello.html文件：

```html
<html>
  <body>
    <h1>Hello Extensions</h1>
  </body>
</html>
```

现在点击插件icon我们就可以看到弹出的页面了。下面的步骤是用来增加键盘快捷方式的配置，不需要的可以不用。

```json
{
  "name": "Hello Extensions",
  "description": "Base Level Extension",
  "version": "1.0",
  "manifest_version": 3,
  "action": {
    "default_popup": "hello.html",
    "default_icon": "hello_extensions.png"
  },
  "commands": {
    "_execute_action": {
      "suggested_key": {
        "default": "Ctrl+Shift+F",
        "mac": "MacCtrl+Shift+F"
      },
      "description": "Opens hello.html"
    }
  }
}
```

最后一步就是将插件安装到本地Chrome

1. 地址栏中输入`chrome://extensions`&#x20;
2. 勾选开发者模式
3. 点击加载已解压的拓展程序，选择刚才的目录

恭喜你，现在点击插件图片或键盘输入`Ctrl+Shift+F`，页面就会跳出一个popup页面
