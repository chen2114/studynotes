---
title: uniapp之微信小程序适配
tags: [学习笔记, uniapp, 适配, 微信, 微信小程序]
categories: [适配]
---
## uni.getUserProfile
授权获取用户信息，由于该方法每次都会弹出授权弹窗而且只能在按钮点击后调用该方法，无法先请求判断用户是否已授权过，只能通过本地缓存判断来决定是否调用该方法
``` js
uni.getUserProfile({
  desc: '登录',
  success: info => {
    console.log(info.encryptedData, info.iv)
  },
  fail: err => {
    console.log(err)
  }
})
```
## uni.getFileSystemManager().readFile()
上传图片转base64
``` js
uni.chooseImage({
  sourceType: ["camera", "album"],
  success: (res) => {
    res.tempFilePaths.map(item => {
      uni.getFileSystemManager().readFile({ //文件管理系统按照base64方式读取生成的图片
        filePath: item, //选择图片返回的相对路径
        encoding: 'base64', //编码格式
        success: file => { //成功的回调
          console.log(file.data)
        }
      })
    })
  }
})
```
## uni.canvasToTempFilePath()
在生成海报时 canvas 中的 drawImage() 无法使用 qrcode 生成的 base64 格式二维码，就需要先将 qrcode 生成的二维码直接放到 canvas 上，再用 uni.canvasToTempFilePath() 转成文件路径
``` vue
<template>
  <canvas class="my-qrcode" style="width: 500px;height: 500px;" width="500px" height="500px" canvas-id="myQrcode" id="myQrcode"></canvas>
</template>
<script>
import QRCode from '@/utils/weapp-qrcode.js'
export default {
  methods: {
    shortUrlToQr(shortUrl) {
      new QRCode('myQrcode',{
        text: shortUrl, // 生成二维码的文本
        width: 500,
        height: 500,
        padding: 12, // 生成二维码四周自动留边宽度，不传入默认为0
        correctLevel: QRCode.CorrectLevel.L, // 二维码可辨识度
        callback: setTimeout(() => {
          // 接下来就可以直接调用微信小程序的api保存到本地或者将这张二维码直接画在海报上面去，看各自需求
          uni.canvasToTempFilePath({
            canvasId: 'myQrcode',
            x: 0,
            y: 0,
            width: 500,
            height: 500,
            success(e) {
              console.log(e.tempFilePath)
            }
          })
        }, 500)
      })
    }
  }
}
</script>
```
## Android、iOS 的适配
### 时间格式
IOS系统只识别 " / " 不识别 " - "
``` js
var date = "2019-8-14 10:03:45"

//解决
var newDate = new Date(date.replace(/-/g, '/'));
```