# 🐵 chrome.action

#### 描述

使用chrome.actionAPI来控制插件在Google Chrome工具栏中的图标。

#### Manifest 字段名

需要在manifest中声明`action`字段才可以使用chrome.action。

#### 适用版本

chrome 88+&#x20;

Manifest V3+

你可以使用chrome.action来设置你的插件在chrome工具栏中的按钮UI。这个工具栏在地址栏的右边。默认情况下这里展示的是插件的菜单。用户可以选择将他们的插件固定在这个位置。

注意即时没有在声明manifest中声明`action`，每一个chrome插件也都有一个默认的icon。

## Manifest

注意提升到V3，才可以使用

```json
{
  "name": "Action Extension",
  ...
  "action": {
    "default_icon": {              // optional
      "16": "images/icon16.png",   // optional
      "24": "images/icon24.png",   // optional
      "32": "images/icon32.png"    // optional
    },
    "default_title": "Click Me",   // optional, shown in tooltip
    "default_popup": "popup.html"  // optional
  },
  ...
}
```

下面的选项都是可选的，为空也没关系。

下面有属性的详细描述。

## UI部分

### Icon

Icon是被使用在工具栏中的图片，默认展示16\*16像素。在manifest中使用`default_icon`这个key来设置。value部分使用图片的相对地址，chrome会从地址寻找图片。如果没有找到匹配的图片，chrome会寻找最近可用的图片作为icon。然而缩放也会让图片看起来模糊。

当然你也可以为不同的设置预设置不同的icon，详情看上面的manifest。

icon也可以使用编码的方式来设置，使用action.setIcon()方法。这个方法会使用HTML canvas element来根据不同突变动态生成icon，或者使用offscreen canvas API从插件service worker来设置。

```javascript
const canvas = new OffscreenCanvas(16, 16);
const context = canvas.getContext('2d');
context.clearRect(0, 0, 16, 16);
context.fillStyle = '#00FF00';  // Green
context.fillRect(0, 0, 16, 16);
const imageData = context.getImageData(0, 0, 16, 16);
chrome.action.setIcon({imageData: imageData}, () => { /* ... */ });
```

{% hint style="info" %}
action.setIcon()API是用来设置固定图片的，不应该被用来设置模拟动画。
{% endhint %}

#### 格式

在一个被打包的插件中，图片可以已很多种形式来展示，包括PNG, JPEG, BMP, ICO, 和其他的（SVG除外）。没有打包的插件仅支持PNG格式的图片。

### 工具提示（标题）

当用户将鼠标移动到插件按钮上方时，会展示一个弹出的提示，里面的内容是插件的名称。

默认的提示内容是使用manifest中的`default_title`来设置的。你也可以动态的使用`action.setTitle()`方法来设置。

### 标志

Action可以可选择的展示一个徽章，是一个icon上方的贴图。这个可以用来展示一些少量的有关插件状态的信息比如计算xx的数量。徽章有一个文本组件和一个背景颜色。

需要注意的是，徽章只有有点的空间，并且仅能使用4个或更少的文字。

徽章在manifest中并没有默认值，你可以使用action.setBadgeBackgroundColor()和action.setBadgeText()来动态的设置文本和背景颜色。当在设置颜色时，需要使用4个0\~255的值（RGBA）来设置背景颜色，文案颜色使用的是CSS颜色。

```javascript
chrome.action.setBadgeBackgroundColor(
  {color: [0, 255, 0, 0]},  // Green
  () => { /* ... */ },
);

chrome.action.setBadgeBackgroundColor(
  {color: '#00FF00'},  // Also green
  () => { /* ... */ },
);

chrome.action.setBadgeBackgroundColor(
  {color: 'green'},  // Also, also green
  () => { /* ... */ },
);
```

### Popup

当用户点击工具栏的插件按钮时，会弹出一个页面。页面包含一个预置的HTML页面，并且会自动的适应内容大小。弹窗最小是25x25像素，最大是800x600像素。

popup页面最初在manifest中的`default_popup`属性下设置，这个属性的父属性也是`action`。如果这个属性存在，那么他应该指向一个存在的路径。当然这个页面也同样可以通过`action.setPopup()`方法来设置。

{% hint style="info" %}
action.onClicked事件并不会因为在页面上点击popup的内容而触发
{% endhint %}

## Per-tab state <a href="#per-tab-state" id="per-tab-state"></a>

插件对于每个页面可以有不同的操作，你可以为不同的页面设置不同的徽章标记，来表示不同页面的状态。在action API中你可以为一个页面独立的tabId来设置页面的标记。举个例子，为一个特定的页面设置徽章的文案，可以像这样写：

```javascript
function getTabId() { /* ... */}
function getTabBadge() { /* ... */}

chrome.action.setBadgeText(
  {
    text: getTabBadge(tabId),
    tabId: getTabId(),
  },
  () => { ... }
);
```

如果TableId被省略，那么这个操作将会被应用到全局。因为独立的设置优先级是凌驾于全局之上的。

## Enabled state

默认情况下，工具栏的icon点击在每个页面都是允许的。你可以使用`action.enable()`和`action.disable()`方法来控制它。这仅仅影响到action.onClicked事件是否被派遣到你的插件中。

## 实例

下方的一些范例展示了action在插件中的一般用法。如果你想看更强的用法，可以在[这里看](https://github.com/GoogleChrome/chrome-extensions-samples/tree/main/api/action)。

### 展示弹窗

当用户点击icon时弹出一个页面是很常见的。如何在你的插件中实现这样的效果呢？首先在`manifest.json`中声明popup文件并且明确需要展示在popup中的内容。

```json
{
  "name": "Action popup demo",
  "version": "1.0",
  "manifest_version": 3,
  "action": {
    "default_title": "Click to view a popup",
    "default_popup": "popup.html"
  }
}
```

{% code title="popup.html" %}
```html
<!DOCTYPE html>
<html>
<head>
  <style>
    html {
      min-height: 5em;
      min-width: 10em;
      background: salmon;
    }
  </style>
</head>
<body>
  <p>Hello, world!</p>
</body>
</html>
```
{% endcode %}

### 点击后向页面注入content脚本

一般的模式就是使用插件的action来暴露插件的主要功能。下方的实例证明了这个模式。当用户点击action之后，插件会向当前页面注入一个content脚本。content脚本注入之后会警告一下用户来确认所有的步骤都按照预期进行。

{% code title="manifest.json" %}
```json

{
  "name": "Action script injection demo",
  "version": "1.0",
  "manifest_version": 3,
  "action": {
    "default_title": "Click to show an alert"
  },
  "permissions": ["activeTab", "scripting"],
  "background": {
    "service_worker": "background.js"
  }
}
```
{% endcode %}

{% code title="background.js" %}
```javascript
chrome.action.onClicked.addListener((tab) => {
  chrome.scripting.executeScript({
    target: {tabId: tab.id},
    files: ['content.js']
  });
});jav
```
{% endcode %}

{% code title="content.js" %}
```javascript
alert('Hello, world!');
```
{% endcode %}

### 使用声明的内容模拟页面操作

Chrome的actionAPI在manifest V3版本中将browserAction和pageAction替换了。默认情况下，actions都和浏览器操作相似，但是使用V3使得页面操作也变成了可能。

下方的例子展示了插件background逻辑可以：

* 默认禁用action
* 使用declarativeContent来在特定页面打开action操作

{% code title="background.js" %}
```javascript
// Wrap in an onInstalled callback in order to avoid unnecessary work
// every time the background script is run
chrome.runtime.onInstalled.addListener(() => {
  // Page actions are disabled by default and enabled on select tabs
  chrome.action.disable()

  // Clear all rules to ensure only our expected rules are set
  chrome.declarativeContent.onPageChanged.removeRules(undefined, () => {
    // Declare a rule to enable the action on example.com pages
    let exampleRule = {
      conditions: [
        new chrome.declarativeContent.PageStateMatcher({
          pageUrl: {hostSuffix: '.example.com'},
        })
      ],
      actions: [new chrome.declarativeContent.ShowAction()],
    };

    // Finally, apply our new array of rules
    let rules = [exampleRule];
    chrome.declarativeContent.onPageChanged.addRules(rules);
  });
});
```
{% endcode %}

## Types

### OpenPopupOptions

Chrome 99+

#### 属性：

windowId：窗口的id来确认打开action popup的页面，如果没有特别注明的话默认使用当前正在使用的页面。

类型：number

### TabDetails <a href="#type-tabdetails" id="type-tabdetails"></a>

#### 属性：

tabId：需要去查询的tabId，如果没有指定，会返回一个non-tab-specific的状态。

类型：number

### UserSettings <a href="#type-usersettings" id="type-usersettings"></a>

chrome 91+

用户对于插件action的设置集合

isOnToolbar：判断用户是否把插件固定在页面顶部的工具栏

类型：boolean

## 方法

### disable

```javascript
chrome.action.disable(
  tabId?: number,
  callback?: function,
}
```

支持Promise，作用是为当前页面禁用action

参数：

* tabId
  * 类型：number
  * 可选
  * 你想操作的对应的TabId
* callback
  * 类型：function
  * 可选
  * 回调函数

返回：

当回调函数未定义时，仅返回一个Promise。

### enable <a href="#method-enable" id="method-enable"></a>

```javascript
chrome.action.enable(
  tabId?: number,
  callback?: function,
)
```

支持Promise，作用是为当前页面启用action

参数：

* tabId
  * 类型：number
  * 可选
  * 你想操作的对应的TabId
* callback
  * 类型：function
  * 可选
  * 回调函数

返回：

当回调函数未定义时，仅返回一个Promise。

### getBadgeBackgroundColor

```javascript
chrome.action.getBadgeBackgroundColor(
  details: TabDetails,
  callback?: function,
)ja
```

支持Promise，作用是获取背景颜色

参数：

* details
  * 类型：TabDetails类
  * 可选
  * 你想操作的对应的TabId
* callback
  * 类型：function
  * 可选
  * 回调函数

返回：

返回一个颜色数组\[number, number, number, number] RGBA格式。

### getBadgeText

```javascript
chrome.action.getBadgeText(
  details: TabDetails,
  callback?: function,
)
```

支持promise，获取到action的徽章文案，如果没有选择tab，会返回一个non-tab-specific badge。如果displayActionCountAsBadgeText被开启了，会返回一个默认的填充文案，除非declarativeNetRequestFeedback权限存在或tab特定的徽章文案已经提供过。

参数：

* details：
  * 类型：TabDetails类

返回：

* 回调函数

当未声明回调时，会返回一个promise。

### getPopup

```javascript
chrome.action.getPopup(
  details: TabDetails,
  callback?: function,
)
```

获取html格式的popup

参数：

* details：
  * 类型：TabDetails

返回：

* String类型的文本

### getTitle

```javascript
chrome.action.getTitle(
  details: TabDetails,
  callback?: function,
)
```

支持promise，获取应用title

参数：

* details：
  * 类型：TabDetails

返回：

* String类型的文本

### getUserSetting

```javascript
chrome.action.getUserSettings(
  callback?: function,
)
```

支持promise，仅支持Chrome 91+。返回用户对于插件的相关设置。

返回：

* UserSettings类

### openPopup

```javascript
chrome.action.openPopup(
  options?: OpenPopupOptions,
  callback?: function,
)
```

支持promise，仅支持chrome 99+。打开插件的popup页面。

参数：

* options：
  * 见上方的openPopupOptions

返回：

* 返回一个回调函数

### setBadgeBackgroudColor

```javascript
chrome.action.setBadgeBackgroundColor(
  details: object,
  callback?: function,
)
```

设置徽章的背景颜色，传入一个object，包含需要修改的颜色（RGBA格式）以及目标页面的tabId，重启页面后重置。

参数：

* details：
  * colorArray：\[255, 0, 0, 255]类似的
* tabId：
  * number

返回：返回一个回调函数或promise

### setBadgeText

```javascript
chrome.action.setBadgeText(
  details: object,
  callback?: function,
)
```

支持promise，为徽章设置文案，徽章会被设置在icon上方。

参数：

* details：
  * 类型：object，包含一个number和一个string
  * number表示需要被修改的tabId
  * string表示修改后的文案

返回：返回一个回调函数，当没有回调时返回一个promise

### setIcon

```javascript
chrome.action.setIcon(
  details: object,
  callback?: function,
)
```

支持promise，设置当前页面的icon样式，可以被设置为一个路径也可以被设置为一个canvas格式的像素数据。无论是路径还是图片数据，都需要在参数中添加属性。

参数：

* details：
  * 类型：object包含一个imageData或path以及需要修改的tabId

返回：当没有设置回调时，返回一个promise

### setPopup

```javascript
chrome.action.setPopup(
  details: object,
  callback?: function,
)
```

支持promise，用来将一个页面的触发效果设置为点击按钮并弹出popup的形式。

参数：

* details：
  * 类型：object，对象包含一个string类型的popup，内容是html文件。和一个tabId，类型是number，用来指向特定的tab使用。

返回：若没有设置回调函数，则返回一个promise

### setTitle

```javascript
chrome.action.setTitle(
  details: object,
  callback?: function,
)
```

支持promise，用来设置工具栏中显示的名称

参数：

* details：object，对象包含一个tabId指定作用的页面，和一个string类型的title用来替换文案

返回：若无回调函数声明，则返回一个promise

## Events

### onClicked

```javascript
chrome.action.onClicked.addListener(
  callback: function,
)
```

当插件的icon被点击时触发，如果插件有popup页面，则不会触发。

返回：

```
(tab: tabs.Tab) => void
```

