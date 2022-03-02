# 🐵 accessibilityFeatures

#### 描述

使用`chrome.accessibilityFeatures`API来管理chrome的无障碍功能。这个API需要依赖chrome.typeAPI的ChromeSetting属性，从而获取或设置独立的无障碍功能。为了去获取功能的状态，插件必须申请accessibilityFeatures.read权限。需要注意的是accessibilityFeatures.modify并不包含accessibilityFeatures.read权限。这里的功能都需要在ChromeOS下获得，一般的插件开发基本用不到。所以不翻译了。贴个链接，大家有需要的可以自己去看看。

#### 权限

`accessibilityFeatures.modify`

`accessibilityFeatures.read` &#x20;

## 总结

#### 属性

[animationPolicy](https://developer.chrome.com/docs/extensions/reference/accessibilityFeatures/#property-animationPolicy)

[autoclick](https://developer.chrome.com/docs/extensions/reference/accessibilityFeatures/#property-autoclick)

[caretHighlight](https://developer.chrome.com/docs/extensions/reference/accessibilityFeatures/#property-caretHighlight)

[cursorColor](https://developer.chrome.com/docs/extensions/reference/accessibilityFeatures/#property-cursorColor)

[cursorHighlight](https://developer.chrome.com/docs/extensions/reference/accessibilityFeatures/#property-cursorHighlight)

[dictation](https://developer.chrome.com/docs/extensions/reference/accessibilityFeatures/#property-dictation)

[dockedMagnifier](https://developer.chrome.com/docs/extensions/reference/accessibilityFeatures/#property-dockedMagnifier)

[focusHighlight](https://developer.chrome.com/docs/extensions/reference/accessibilityFeatures/#property-focusHighlight)

[highContrast](https://developer.chrome.com/docs/extensions/reference/accessibilityFeatures/#property-highContrast)

[largeCursor](https://developer.chrome.com/docs/extensions/reference/accessibilityFeatures/#property-largeCursor)

[screenMagnifier](https://developer.chrome.com/docs/extensions/reference/accessibilityFeatures/#property-screenMagnifier)

[selectToSpeak](https://developer.chrome.com/docs/extensions/reference/accessibilityFeatures/#property-selectToSpeak)

[spokenFeedback](https://developer.chrome.com/docs/extensions/reference/accessibilityFeatures/#property-spokenFeedback)

[stickyKeys](https://developer.chrome.com/docs/extensions/reference/accessibilityFeatures/#property-stickyKeys)

[switchAccess](https://developer.chrome.com/docs/extensions/reference/accessibilityFeatures/#property-switchAccess)

[virtualKeyboard](https://developer.chrome.com/docs/extensions/reference/accessibilityFeatures/#property-virtualKeyboard)

