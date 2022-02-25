# 🐼 Chrome 插件新特性

这里会展示Chrome插件平台的改动、文档变化和相关的协议或变更，咱要及时查看（但我不一定会及时更新）

### Chrome 100：本地消息端口保持服务健康

2022.2.9

咱们就是说可以通过`chrome.runtime.connectNative()`这个方法，链接到一个本地服务端口，端口不关闭，我们不罢工（也不知道翻译的对不对，大概就是这么个意思）

### Chrome 100：omnibox.setDefaultSuggestion()将支持promise和callbacks

2022.2.8

就是说`omnibox.setDefaultSuggestion()`这个方法之前不支持，现在支持promise和callback了

### Chrome 99：match\_origin\_as\_fallback 发灰度咯

2022.1.5

content.js现在可以识别到`match_origin_as_fallback`这个key了，有啥用呢？就是注入页面时候可以识别到一些特殊的frame包括：`about:`, `data:`, `blob:`, `filesystem:`

### Chrome 99：promise将支持消息API也发灰度咯

2021.12.28

promise支持在V3中已经被添加到了`tabs.sendMessage`，`runtime.sendMessage`和`runtime.sendNativeMessage`

### 文档更新我就不翻译了

大概意思就是优化了一下Web Store

### Chrome 96：给27个API加上了promise支持

2021.10.1

带善人！

<details>

<summary>插件 API</summary>

* `chrome.browsingData`
* `chrome.commands`&#x20;
* `chrome.contentSettings`&#x20;
* `chrome.debugger`
* `chrome.downloads`&#x20;
* `chrome.enterprise.hardwarePlatform`&#x20;
* `chrome.fontSettings`&#x20;
* `chrome.history`&#x20;
* `chrome.instanceID`&#x20;
* `chrome.permissions`&#x20;
* `chrome.processes`&#x20;
* `chrome.search`&#x20;
* `chrome.sessions`&#x20;
* `chrome.signedInDevices`&#x20;
* `chrome.topSites`

使用了ChromeSetting原型的API也都支持promise了，下面这几个API收到影响了

* `chrome.privacy`
* `chrome.accessibilityFeatures`
* `chrome.proxy`

PS：原文这里有个链接可以点击到对应API，兄弟们别着急，慢慢加

</details>

<details>

<summary>Chrome OS API</summary>

* `chrome.certificateProvider`&#x20;
* `chrome.documentScan`&#x20;
* `chrome.enterprise.deviceAttributes`&#x20;
* `chrome.enterprise.networkingAttributes`&#x20;
* `chrome.fileBrowserHandler`&#x20;
* `chrome.fileSystemProvider`&#x20;
* `chrome.loginState`&#x20;
* `chrome.printingMetrics`&#x20;
* `chrome.wallpaper`

</details>

### Chrome 96：动态的content.js

2021.9.24

咱们就是说`chrome.scripting`这个API现在可以支持content脚本的注册、更新、注销和获取列表的操作了。特别说明，要使用content脚本时，要么在manifest里声明，要么用`chrome.scripting.executeScript()`方法在运行时注入



{% hint style="info" %}
TNND，为什么这么多，这个先不翻译了，先把重要的翻译了
{% endhint %}

