# 🐼 开始啦

插件是多样化的，但同样也是内聚的组件化的。组件包括background脚本、content脚本、选项页面、UI组件和各种各样的逻辑文件。插件的组件主要通过HTML、CSS和JavaScript来编写。插件的组件选择取决于功能，并不一定包含全部的组件。

这篇教学将会带你编写一个可以改变用户当前页面背景颜色的插件。我们会用到插件平台所给予的很多组件，并展示他们的用法和联系。

首先新建一个根目录，随便起个名字用来装这些插件相关的文件。

或者你可以直接下载我们写好的代码：[在这里哦](https://storage.googleapis.com/web-dev-uploads/file/WlD8wC6g8khYWPJUsQceQkhXSlv1/SVxMBoc5P3f6YV3O7Xbu.zip)

## 创建一个manifest

盘古开天辟地，想写插件先建manifest。在刚才新建的目录下新建一个文件`manifest.js`其中包含如下代码。

{% code title="manifest.json" %}
```json
{  
  "name": "Getting Started Example",
  "description": "Build an Extension!",
  "version": "1.0",
  "manifest_version": 3
}
```
{% endcode %}

### 加载一个已解压的插件

在Chrome的开发者模式下，我们可以加载一个包含manifest.json的文件夹作为一个插件。要想加载的话，跟随如下步骤：

1. 打开Chrome，在地址栏中输入`chrome://extensions` &#x20;
   * 或者你可以通过头像左边那个拼图按钮，点击后选择管理扩展程序打开
   * 或者你可以通过点击头像右边那三个点，点击后选择更多工具 -> 扩展程序打开
2. 打开开发者模式开关
3. 点击加载已解压程序

![](../.gitbook/assets/image.png)

好的兄弟们，这个插件咱就装完了。因为插件的manifest.json里没写头像路径，所以就给咱用首字母自动生成了一个头像。

## 添加功能

插件是装好了，但是他啥也干不了，因为咱没写功能呢。下面咱加一个存储背景颜色值的功能吧。

### 在manifest中注册background脚本

background脚本和其他重要的组件一样，需要被注册到manifest中。注册意味着告诉插件需要引用哪个文件，这些文件怎么操作。

{% code title="manifest.json" %}
```json
{
  "name": "Getting Started Example",
  "description": "Build an Extension!",
  "version": "1.0",
  "manifest_version": 3,
  "background": {
    "service_worker": "background.js"
  }
}n
```
{% endcode %}

现在Chrome知道了咱的插件有一个服务了，当我们重新载入插件时，Chrome会扫描必要的文件作为额外指令，就像是一个需要遵从的很重要的指令。

### 创建background脚本文件

插件需要在被安装的时候获取到一些永久变量的信息。我们通过`runtime.onInstalled`这个方法来开始获取。在`onInstalled`监听内部，插件会设置一个变量来使用存储的API。这些变量将会在其他的插件组件中使用和更新。下面我们在插件目录下新建一个`backgroud.js`文件并添加如下代码。

{% code title="background.js" %}
```javascript
let color = '#3aa757';

chrome.runtime.onInstalled.addListener(() => {
  chrome.storage.sync.set({ color });
  console.log('Default background color set to %cgreen', `color: ${color}`);
});
```
{% endcode %}

