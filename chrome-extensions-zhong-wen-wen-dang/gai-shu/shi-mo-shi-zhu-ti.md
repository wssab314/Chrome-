# 🐱 什么是主题？

主题是一种特殊形式的插件，可以修改浏览器的样式。主题的打包方式和插件一样，但是他们不包含JavaScript和HTML文件。

![](<../../.gitbook/assets/image (3).png>)

## Manifest

这有一个主题的manifest文件内容：

```json
{
  "version": "2.6",
  "name": "camo theme",
  "theme": {
    "images" : {
      "theme_frame" : "images/theme_frame_camo.png",
      "theme_frame_overlay" : "images/theme_frame_stripe.png",
      "theme_toolbar" : "images/theme_toolbar_camo.png",
      "theme_ntp_background" : "images/theme_ntp_background_norepeat.png",
      "theme_ntp_attribution" : "images/attribution.png"
    },
    "colors" : {
      "frame" : [71, 105, 91],
      "toolbar" : [207, 221, 192],
      "ntp_text" : [20, 40, 0],
      "ntp_link" : [36, 70, 0],
      "ntp_section" : [207, 221, 192],
      "button_background" : [255, 255, 255]
    },
    "tints" : {
      "buttons" : [0.33, 0.5, 0.47]
    },
    "properties" : {
      "ntp_background_alignment" : "bottom"
    }
  }
}
```

### 颜色 colors

颜色是RGB格式的，我们也有一些字符形式的颜色，可以在这里查到：[`kOverwritableColorTable`](https://cs.chromium.org/search/?q=file:chrome/browser/themes%20symbol:kOverwritableColorTable)

### 图片 images

图片的路径是相对于根目录的相对路径。你可以重写任意的图片。所有的图片都必须要求png格式。
