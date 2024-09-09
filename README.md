# uniapp方式实现视频播放加密统计

在移动应用开发中，视频播放功能是常见的需求之一，而视频内容的加密保护和统计则成为了开发者需要关注的问题。本文将分享如何使用uniapp结合视频云点播插件，实现视频播放的加密和统计功能。对方案有任何疑问，可V：wjc24680525

## 环境准备

在开始之前，请确保你已经安装了HBuilderX，并创建了一个uniapp项目。接下来，你需要从[插件市场](https://ext.dcloud.net.cn/plugin?id=2802)购买并下载视频云点播插件。

## 集成插件

1. **购买插件**：在插件市场中选择视频云点播插件，并绑定到你的项目中。
2. **配置插件**：在项目的`manifest.json`文件中，找到`app原生插件配置`部分，勾选视频云点播插件。

## 视频播放加密

视频加密是保护视频内容不被非法获取的重要手段。插件提供了强大的加密功能，我们可以利用这一功能来保护我们的视频内容。

### 配置加密参数

首先，你需要在视频云平台获取`userid`、`readtoken`、`writetoken`和`secretkey`，这些参数将用于视频的加密和解密。

```javascript
var configModule = uni.requireNativePlugin("PLV-VodUniPlugin-ConfigModule");
configModule.setToken({
    'userid': '你的userid',
    'readtoken': '你的readtoken',
    'writetoken': '你的writetoken',
    'secretkey': '你的secretkey'
}, (ret) => {
    if (ret.isSuccess) {
        console.log('设置token成功');
    } else {
        console.error('设置token失败：', ret.errMsg);
    }
});
```

### 视频播放

使用视频云点播插件提供的播放器组件`plv-player`来播放视频。

```html
<template>
    <plv-player
        ref="vod"
        class="vod-player"
        seekType="0"
        autoPlay="true"
        disableScreenCAP="false"
        rememberLastPosition="false"
        @onPlayStatus="onPlayStatus"
        @onPlayError="onPlayError"
        @positionChange="positionChange">
    </plv-player>
</template>

<script>
export default {
    methods: {
        setVid() {
            this.$refs.vod.setVid({
                vid: '视频的vid',
                level: 0
            }, (ret) => {
                if (ret.errMsg) {
                    uni.showToast({
                        title: ret.errMsg,
                        icon: "none"
                    });
                }
            });
        },
        onPlayStatus(e) {
            console.log('播放状态：', e.playbackState);
        },
        onPlayError(e) {
            console.error('播放错误：', e.errCode, e.errEvent);
        },
        positionChange(e) {
            console.log('当前播放位置：', e.currentPosition);
        }
    },
    mounted() {
        this.setVid();
    }
}
</script>

<style>
.vod-player {
    width: 100%;
    height: 100%;
}
</style>
```

## 视频播放统计

视频播放统计对于了解用户行为和优化内容至关重要。插件对应的管理后台提供了丰富的统计字段和API，可根据实际需求调用
