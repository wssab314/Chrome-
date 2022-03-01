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

1. 打开Chrome，在地址栏中输入`chrome://extensions`
   * 或者你可以通过头像左边那个拼图按钮，点击后选择管理扩展程序打开
   * 或者你可以通过点击头像右边那三个点，点击后选择更多工具 -> 扩展程序打开
2. 打开开发者模式开关
3. 点击加载已解压程序

![](<../.gitbook/assets/image (1).png>)

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

### 添加存储权限

包括存储API在内的大多数API，都需要先在manifest文件中的`"permissions"`字段下进行声明，才可以正常使用哦。

{% code title="manifest.json" %}
```json
{
  "name": "Getting Started Example",
  "description": "Build an Extension!",
  "version": "1.0",
  "manifest_version": 3,
  "background": {
    "service_worker": "background.js"
  },
  "permissions": ["storage"]
}
```
{% endcode %}

### 检查background脚本执行情况

我们回到插件管理页面，重新载入插件后（每次对插件的修改都需要重新载入一下），下面就出现了一个查看视图的链接，点击之后会打开一个控制台（如果你的插件没有background页面的话），里面打印了一行log`Default background color set to green`。

![](<../.gitbook/assets/image (3) (1).png>)

## 介绍一个用户界面

插件的用户交互界面可以有很多种形式，在这里咱们用popup来示范。首先在插件目录中创建一个名叫`popup.html`的文件。咱们的插件需要用一个按钮去修改背景颜色，按钮就在`popup.html`中。

{% code title="popup.html" %}
```html
<!DOCTYPE html>
<html>
  <head>
    <link rel="stylesheet" href="button.css">
  </head>
  <body>
    <button id="changeColor"></button>
  </body>
</html>
```
{% endcode %}

和background脚本一样，这个脚本也需要在manifest.json中声明。我们可以在manifest中添加一个`action`字段来设置`popup.html`为插件的`default_popup`。

{% code title="manifest.json" %}
```json
{
  "name": "Getting Started Example",
  "description": "Build an Extension!",
  "version": "1.0",
  "manifest_version": 3,
  "background": {
    "service_worker": "background.js"
  },
  "permissions": ["storage"],
  "action": {
    "default_popup": "popup.html"
  }
}
```
{% endcode %}

这个弹出的HTML会引用一个外部的css文件，我们需要把他加到目录里，命名为button.css。

{% code title="button.css" %}
```css
button {
  height: 30px;
  width: 30px;
  outline: none;
  margin: 10px;
  border: none;
  border-radius: 2px;
}

button.current {
  box-shadow: 0 0 0 2px white,
              0 0 0 4px black;
              
}
```
{% endcode %}

我们也可以通过manifest文件中`action`字段下的`default_icons`来设置插件在工具栏的icon显示。可以通过[这个链接](https://storage.googleapis.com/web-dev-uploads/file/WlD8wC6g8khYWPJUsQceQkhXSlv1/wy3lvPQdeJn4iqHmI0Rp.zip)下载icon文件，解压后放在插件目录即可。记得更新manifest文件。

{% code title="manifest.json" %}
```json
{
  "name": "Getting Started Example",
  "descriptnion": "Build an Extension!",
  "version": "1.0",
  "manifest_version": 3,
  "background": {
    "service_worker": "background.js"
  },
  "permissions": ["storage"],
  "action": {
    "default_popup": "popup.html",
    "default_icon": {
      "16": "/images/get_started16.png",
      "32": "/images/get_started32.png",
      "48": "/images/get_started48.png",
      "128": "/images/get_started128.png"
    }
  }
}
```
{% endcode %}

在插件管理页中，每个插件有一个展示的图片、权限警告和图标。这些图标都可以在manifest文件中的icons字段下设置。

{% code title="manifest.json" %}
```json
{
  "name": "Getting Started Example",
  "description": "Build an Extension!",
  "version": "1.0",
  "manifest_version": 3,
  "background": {
    "service_worker": "background.js"
  },
  "permissions": ["storage"],
  "action": {
    "default_popup": "popup.html",
    "default_icon": {
      "16": "/images/get_started16.png",
      "32": "/images/get_started32.png",
      "48": "/images/get_started48.png",
      "128": "/images/get_started128.png"
    }
  },
  "icons": {
    "16": "/images/get_started16.png",
    "32": "/images/get_started32.png",
    "48": "/images/get_started48.png",
    "128": "/images/get_started128.png"
  }
}
```
{% endcode %}

一般情况下，插件都展示在插件列表中，点击图钉可以让他展示在工具栏中。

![](<../.gitbook/assets/image (2) (1).png>)

进行到这步了，我们重新载入一下插件，就可以看到页面展示了我们的插件图标而非默认图标，点击图标后会有一个popup页面弹出，里面有一个按钮展示了默认情况下的颜色。

![](<../.gitbook/assets/image (1) (1).png>)

popup UI的最后一步就是添加颜色到按钮上去，在目录下创建一个名为`popup.js`的文件，并将如下代码添加到文件中。

{% code title="popup.js" %}
```javascript
// Initialize button with user's preferred color
let changeColor = document.getElementById("changeColor");

chrome.storage.sync.get("color", ({ color }) => {
  changeColor.style.backgroundColor = color;
});java
```
{% endcode %}

这块代码的意思就是从`popup.html`页面中获取到按钮元素，然后从本地存储中请求背景颜色的值，然后将他设置为按钮的背景颜色值。下面我们需要在popup.html中引用一下`popup.js`。

```markup
<!DOCTYPE html>
<html>
  <head>
    <link rel="stylesheet" href="button.css">
  </head>
  <body>
    <button id="changeColor"></button>
    <script src="popup.js"></script>
  </body>
</html>
```

重新载入一下插件，我们就可以看到一个绿色的按钮啦。

## 布局逻辑

现在咱们的插件有了客制化的图标于弹窗，并且弹窗中的颜色是基于插件存储中的颜色。下一步我们需要添加更多的用户交互逻辑。更新popup.js文件下的内容，添加如下代码。

{% code title="popup.js" %}
```javascript
// When the button is clicked, inject setPageBackgroundColor into current page
changeColor.addEventListener("click", async () => {
  let [tab] = await chrome.tabs.query({ active: true, currentWindow: true });

  chrome.scripting.executeScript({
    target: { tabId: tab.id },
    function: setPageBackgroundColor,
  });
});

// The body of this function will be executed as a content script inside the
// current page
function setPageBackgroundColor() {
  chrome.storage.sync.get("color", ({ color }) => {
    document.body.style.backgroundColor = color;
  });
}
```
{% endcode %}

上述代码为按钮添加了一个能够触发content脚本注入的监听函数。脚本会让页面的背景颜色修改为和button一样的颜色。使用编程式注入允许用户调用content脚本，而不是自动注入一些并不需要的代码到页面中。

mainfest需要添加`activeTab`权限来允许插件短暂的访问当前页面，同样的需要添加`scripting`权限来调用脚本API的`executeScript`方法。

{% code title="manifest.json" %}
```json
{
  "name": "Getting Started Example",
  ...
  "permissions": ["storage", "activeTab", "scripting"],
  ...
}
```
{% endcode %}

现在插件已经功能齐全啦，重新载入一下插件并刷新页面。点击脚本按钮打开popup页面并点击其中的按钮，页面就绿啦！你不喜欢绿色也可以换成其他的颜色。

{% hint style="danger" %}
陷阱

插件不能再Chrome原生页面进行注入脚本，所以你在类似chrome://extensions页面点击按钮是没有效果的。请确保你在一个真正的web页面上进行插件的触发。
{% endhint %}

## 添加用户选项

现在咱们的插件仅允许用户将页面北京改成绿色的。在这个基础上加一个选项页面，可以给用户更多的操控权限，更多的客制化他们的浏览体验。

那么我们可以先在目录下创建一个`options.html`的文件，包含如下代码。

{% code title="options.html" %}
```html
<!DOCTYPE html>
<html>
  <head>
    <link rel="stylesheet" href="button.css">
  </head>
  <body>
    <div id="buttonDiv">
    </div>
    <div>
      <p>Choose a different background color!</p>
    </div>
  </body>
  <script src="options.js"></script>
</html>
```
{% endcode %}

添加完后，记得在manifest文件中加上这个页面。

```json
{
  "name": "Getting Started Example",
  ...
  "options_page": "options.html"
}
```

重新载入程序后，右键咱们的插件图标选择**Options**选项。

或者你也可以在查看插件细节的页面底部找到**Extension options**选项。

最后一步就是加上可选择的选项，先在根目录创建一个options.js文件，加入如下代码。

```javascript
let page = document.getElementById("buttonDiv");
let selectedClassName = "current";
const presetButtonColors = ["#3aa757", "#e8453c", "#f9bb2d", "#4688f1"];

// Reacts to a button click by marking the selected button and saving
// the selection
function handleButtonClick(event) {
  // Remove styling from the previously selected color
  let current = event.target.parentElement.querySelector(
    `.${selectedClassName}`
  );
  if (current && current !== event.target) {
    current.classList.remove(selectedClassName);
  }

  // Mark the button as selected
  let color = event.target.dataset.color;
  event.target.classList.add(selectedClassName);
  chrome.storage.sync.set({ color });
}

// Add a button to the page for each supplied color
function constructOptions(buttonColors) {
  chrome.storage.sync.get("color", (data) => {
    let currentColor = data.color;
    // For each color we were provided…
    for (let buttonColor of buttonColors) {
      // …create a button with that color…
      let button = document.createElement("button");
      button.dataset.color = buttonColor;
      button.style.backgroundColor = buttonColor;

      // …mark the currently selected color…
      if (buttonColor === currentColor) {
        button.classList.add(selectedClassName);
      }

      // …and register a listener for when that button is clicked
      button.addEventListener("click", handleButtonClick);
      page.appendChild(button);
    }
  });
}

// Initialize the page by constructing the color options
constructOptions(presetButtonColors);
```

重新载入插件后，我们就有4种颜色可以选择啦。当我们点击按钮后，脚本会在插件存储中更新color的值。因为所有的插件文件都是从存储总获取颜色的，所以不用更新其他的值。

## 迈出下一步

恭喜你！现在这个目录下的有一个全功能的、非常简单的Chrome插件啦！

那么下一步要做什么呢？

* 你可以读一读**Chrome插件综述**，这里有插件的结构信息和一些开发者需要熟悉的具体概念。
* 在**调试之旅**中学习调试插件的方式。
* Chrome插件可以访问比打开一个页面更强力的API。**chrome.\*API文档**中将会讲解每一个API。
* **开发者指南**中有数十个关于高级插件创建的相关文档。

