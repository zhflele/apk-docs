# 233联运广告对接文档

| 文档版本 | 修订日期   | 说明                                                         |
| -------- | ---------- | ------------------------------------------------------------ |
| v1.0.1   | 2020.08.09 | 创建文档                                                     |
| v1.0.2   | 2020.10.18 | 更新联运广告接入                                             |
| v1.0.3   | 2020.04.08 | 优化广告展示                                                 |
| v1.0.4   | 2021.07.31 | 添加插屏广告的支持                                           |
| v1.0.4.1 | 2021.08.05 | 优化初始化接口、新增unity插件                                |
| v1.0.4.2 | 2021.08.18 | 优化播放视频广告接口                                         |
| v1.0.5   | 2021.08.25 | 添加Banner广告的支持                                         |
| v1.0.6   | 2022.01.11 | 1、添加关闭Banner广告的功能 2、添加开启或关闭个性化广告推荐的开关 |
| v1.0.7   | 2022.07.12 | 修复部分bug提升稳定性                                        |

SDK说明

SDK开发者：北京龙威互动科技有限公司

SDK名称：233乐园广告SDK

SDK主要功能：用于广告投放活动，包括广告展示、监测、归因及投放效果分析与优化功能，提供个性化推荐服务。

**用户信息与隐私策略**

**如果您是开发者，在为用户提供服务前请阅读****[《233乐园广告SDK隐私政策》](https://dev.233leyuan.com/#/doc/50138)****与****[《233乐园广告SDK合规使用说明》](https://dev.233leyuan.com/#/doc/50139)****，了解SDK对个人信息收集范围、处理目的以及权限使用情况。请您向用户提供服务时，告知相关信息并取得用户同意。**

**如果您是用户，请在使用我们的服务前阅读****[《233乐园广告SDK隐私政策》](https://dev.233leyuan.com/#/doc/50138)****与****[《233乐园广告SDK合规使用说明》](https://dev.233leyuan.com/#/doc/50139)****，了解SDK对个人信息收集范围、处理目的以及权限使用情况。充分理解后再开始使用我们的服务。**

1. **联运****广告申请**

请登录开发者后台，申请联运广告合作。 我们会返回给您以下参数：

| 参数   | 说明          |
| ------ | ------------- |
| appKey | 应用的联运key |

备注：关于联运合作，还可以接入登录和支付哦。

1. **联运****广告****SDK****接入**

### **2.1** **SDK****引用**

下载[mpg-cm-v1.0.7.zip](https://cdn.233xyx.com/online/KXoWJSfSm9BH1705906632580.zip)解压并将mpg-cm-v1.0.7.aar文件复制到您项目Project/app/libs文件夹下。

在您app的build.gradle中添加：

```Bash
dependencies {
    ...
    implementation files('libs/mpg-cm-v1.0.7.aar')
}
```

unity游戏可直接使用压缩文件中的unity插件导入接口，无需单独拷贝aar

### **2.2** **SDK****使用**

#### **2.2.1 初始化**

示例代码

```Bash
MetaAdApi.get().init(application, APP_KEY, new InitCallback() {
            @Override
            public void onInitSuccess() {
                Log.d(TAG, "onInitSuccess");
            }
            @Override
            public void onInitFailed(int errorCode, String errorMsg) {
              Log.d(TAG, String.format("onInitFailed: code: %1d,  msg: %2s", errorCode, errorMsg));
            }
        });
```

#### **2.2.2 播放视频广告（包含****激励视频****&全屏视频 两种广告样式）**

示例代码

pos: int值；平台申请的激励视频或全屏视频广告位ID

```Bash
 MetaAdApi.get().showVideoAd(pos, new IAdCallback.IVideoIAdCallback() {
                @Override
                public void onAdShow() {
                    // 播放成功
                    Log.d("MetaAdApi", "onAdShow");
                }
                @Override
                 public void onAdShowFailed(int errCode, String errMsg) {
                    // 播放失败
                    Log.d("MetaAdApi", "onAdShowFailed： " + errMsg);
                }
                @Override
                public void onAdClick() {
                    //点击广告
                    Log.d("MetaAdApi", "onAdClick");
                }
                @Override
                public void onAdClose() {
                    //  广告关闭
                }
              
                @Override
                public void onAdClickSkip() {
                    // 播放点击跳过
                    Log.d("MetaAdApi", "onAdClickSkip");
                }
                @Override
                public void onAdReward() {
                    //发放激励
                    Log.d("MetaAdApi", "onAdReward");
                }
                @Override
                public void onAdClose(Boolean aBoolean) {
                    // 广告关闭, 废弃 ,建议使用onAdClose 和 onAdReward
                    Log.d("MetaAdApi", "onAdClose");
                }
            });
```

#### **2.2.3播放插屏广告**

示例代码：

pos: int值；平台申请的插屏类型广告位ID

```Bash
MetaAdApi.get().showInterstitialAd(pos, new IAdCallback() {
            @Override
            public void onAdShow() {
                //广告展示
            }
            @Override
            public void onAdShowFailed(int errCode, String errMsg) {
                //展示失败
            }
            @Override
            public void onAdClick() {
                //广告被点击
            }
            @Override
            public void onAdClose() {
                //广告被关闭
            }
        });
```

#### **2.2.4播放****Banner广告****（该广告类型暂停使用，不进行展示）**

#### 示例代码：

#### pos: int值；平台申请的Banner类型广告位ID

```Bash
MetaAdApi.get().showBannerAd(pos, new IAdCallback() {
            @Override
            public void onAdShow() {
                //广告展示
            }
            @Override
            public void onAdShowFailed(String s) {
                //展示失败
            }
            @Override
            public void onAdClick() {
                //广告被点击
            }
            @Override
            public void onAdClose() {
                //广告被关闭
            }
        });
```

#### **2.2.5 关闭****Banner广告**

示例代码：

```Bash
MetaAdApi.get().closeBannerAd();
```

#### **2.2.6 广告个性化推荐开关**

示例代码：

isOpen: boolean值；true: 打开个性化开关；false: 关闭个性化开关；

```Bash
MetaAdApi.get().setPersonalRecommendAd(true);
```

### **2.3特殊说明**

关于版本是否支持联运广告，除了在播放广告失败回调信息“version not support”外，还可以通过接口获取：

```Bash
if (MetaAdApi.get().isInSupportVersion(int type)) {
    // 版本支持联运广告
}
type ： 测试的广告类型；
    AdType.AD_TYPE_VIDEO  : 视频（激励&全屏）
    AdType.AD_TYPE_INTERSTITIAL ：插屏    Banner不支持使用该方法判断
```

备注：

插屏广告只能在3.6.0.3以上的233乐园版本中才能展示，铺量持续进行中。

Banner广告只能在3.15.0.0以上的233乐园版本中才能展示（目前因为单价过低，已关闭展示）。

请重新下载“自测工具”，使用最新的自测工具来测试新类型广告。

#### **附录**

[mpg-cm-v1.0.7.zip](https://cdn.233xyx.com/online/s8IlNxxTtBDN1710760260537.zip)

| 错误码 | 错误信息                          | 处理方案                                                     |
| ------ | --------------------------------- | ------------------------------------------------------------ |
| 200    | 请求成功                          |                                                              |
| 10001  | 网络异常                          | 未清除自测工具缓存                                           |
| 20002  | 游戏未验证通过 未收录在游戏目录中 |                                                              |
| 20003  | 未初始化成功                      | 包名或appkey有误，请检查参数                                 |
| 10004  | 参数无效                          | 包名、appkey有误，请确认参数                                 |
| 30007  | 233乐园不支持此类型广告           | 未使用最新自测工具打开游戏，自测工具见SDK压缩包内            |
| 30008  | 第三方Sdk引发的错误               | onAdShowFailed 30008一般有以下情况： ①广告位没有关联； ②广告pos值错了，检查下插屏/全屏的pos是不是正确的。 ③banner请求过于频繁会报错，需要调整间隔时间为60s |