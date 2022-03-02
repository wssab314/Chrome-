---
description: 因能力有限，仅包含Stable API
---

# 🐼 API 参考

Chrome给插件提供了很多具有特殊目的的API比如`chrome.runtime`和`chrome.alarms`。

## API协议

除非文档特别说明，以chrome.\*开头的API是异步的：这些方法都是即时返回，不需要等待其他操作完成。如果你需要直到某一个操作的执行结果，那你就需要给他加上一个回调函数。还不理解的话，看[这个视频](https://www.youtube.com/watch?v=bmxr75CV36A)。

## Stable APIs

以下信息均不适配于2015年发布的Chrome42以前的版本。



{% swagger method="get" path="" baseUrl="" summary="" %}
{% swagger-description %}

{% endswagger-description %}
{% endswagger %}

<details>

<summary></summary>



</details>

| 名称                                                | 描述                                                                                                                                                                                                               |
| ------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| [accessibilityFeatures](accessibilityfeatures.md) | `chrome.accessibilityFeatures` API是用来管理Chrome的辅助功能的，使用前需要请求`accessibilityFeatures.read`权限，需要注意的是`accessibilityFeatures.modify`权限并不包含`accessibilityFeatures.read`权限，需要特别申请。这里的API基本都需要在Chrome OS下运行，一般的插件开发基本用不到。 |
| [chrome.action](chrome.action.md)                 | 使用chrome.actionAPI来控制插件在Google Chrome工具栏中的图标                                                                                                                                                                     |
|                                                   |                                                                                                                                                                                                                  |

