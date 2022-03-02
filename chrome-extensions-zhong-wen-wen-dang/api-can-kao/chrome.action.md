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

