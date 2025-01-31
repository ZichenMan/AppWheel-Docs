---
sidebar_position: 1
title: AppWheel SDK Integration
id: AppWheel-SDK-Integration
slug: /
---
# AppWheel SDK Integration
## 整体流程
![Overall Process](/img/integration/overallProcess-zh.jpg)

## iOS 对接
### 第一步：创建应用

![Overall Process](/img/integration/createAnApp.png)

- **App name:** 你的应用名称
- **Store:** 平台，有Android、iOS之分，此时选择iOS
- **App Bundle ID:** bundleID为您的应用的在苹果商店的标识，可以在App Store Connect中查看您的app获得
- **iTunesConnect App Specific Shared Secret:** 共享密钥，这个密钥在校验您的app产生的订单收据时需要用到。获取方法可以参照： [https://help.apple.com/app-store-connect/#/devf341c0f01](https://help.apple.com/app-store-connect/#/devf341c0f01)，有两种共享密钥可供选择：一种是：针对所有app的共享密钥；另外一种是针对单个app的共享密码。建议使用“针对单个app的共享密钥。

### Step 2: 应用配置
#### 通知配置(<font color="red"> **重要：如果有自己的接收服务器参考「配置通知接收服务器」说明"**</font>)
  - 把Apple服务器通知发送Appwheel，请将以下地址([https://msgserver-dot-subscription-saas.uc.r.appspot.com/subcenter/receive_ios_notification](https://msgserver-dot-subscription-saas.uc.r.appspot.com/subcenter/receive_ios_notification)) 


![Overall Process](/img/integration/notificationReceivingConfiguration1.png)

  - to Apple Store Connect: (Currently supports V1 type notifications)
  
![Overall Process](/img/integration/notificationReceivingConfiguration2.png)

#### 配置通知接收服务器
- 如果业务方有自己的通知接收服务器需要这部分推送数据可以再 AppWheel 主站里设置接收服务器地址，我们会按照消息到来的顺序将每条消息发送一次，但有时可能并不按顺序传送消息，或者会将消息传送多次，业务方需按照幂等处理的方式设计程序。

![Overall Process](/img/integration/configServerIOS.png)

- 配置好了之后，可以通过 「Test」按钮测试地址的可用性，需要和官方要求的配置一致，即接受 POST 方法的请求，在处理成功后返回 200 Http状态码，在处理失败时返回 400 或 500 状态码；另，因为测试请求这里没有实际的业务数据 body，所以在收到空数据的时候返回 200 的http 状态码即可。

#### 购买优惠商品的 P8 文件配置
当您的app在苹果商店设置了优惠，并且想要在app中使用此功能时，需要在AppWheel中设置p8文件。

![Overall Process](/img/integration/p8.png)

**Service Account credentials JSON:** 一个后缀为.p8的文件，通常称为p8文件。当您需要启用订阅的优惠功能时需要对当前的优惠有个签名，p8文件就是生成这个签名的必不可少的key。获取方式：[https://help.apple.com/app-store-connect/#/dev689c93225](https://help.apple.com/app-store-connect/#/dev689c93225])

#### 商品配置
- 对接前已有商品配置
- 对接前没有商品配置


### Step 3: 验收检查
- □App 配置检查
	- □通知是否配置成功：检查 AppStoreConnect 后台的配置

![Overall Process](/img/integration/appStoreConnectBackend.png)

	- □ App设置如下

![Overall Process](/img/integration/appConfiguration.png)

![Overall Process](/img/integration/addP8File.png)

- □是否在启动时调用了初始化接口
- □是否能通过 appwheel sdk 获取购买的商品信息
	- □订阅商品
		- □优惠商品（选填）
			- □新用户推介促销优惠
			- □促销优惠
			- □优惠码优惠
		- □消耗商品
		- □非消耗商品
		- □非续期订阅商品
- □是否能通过 appwheel sdk 恢复商品信息

### 其它步骤：对接高级功能
#### 发券机
##### 获取优惠券
说明：开发者应当在需要展示优惠券前尽可能早的请求优惠券的信息

```Objective-C
AWPurchaseKit queryCouponDetail:((^)(BOOL success,AWCouponModel * _Nullable model, AWError * _Nullable error))completion
```


返回的AWCouponModel数据结构，请见文档： AppWheel SDK Integration Document - Introduction to iOS Classes 

##### 消耗优惠码
说明：开发者应该在展示过优惠券之后主动把taskId上报给AW，才能完成统计。

```Objective-C
AWPurchaseKit updateConponStateWithTaskId:(long)taskId
           withCompletion:((^)(BOOL success, AWError * _Nullable error))completion
```

参数:taskId,由获取优惠券的接口得到的AWCouponModel.taskId得到

## Android对接
### 第一步：创建应用

![createAnAppAndroid](/img/integration/createAnAppAndroid.png)
- **App name**: 你的应用名称
- **Store**: 平台，有Android、iOS之分，此时选择iOS
- **Google Play package**: 应用的包名，包名为您的应用在谷歌商店的唯一标识，可在[Google Play Console](https://play.google.com/console)中获取
- **Service Account credentials JSON**: 服务器校验订单需要的json文件。拥有这个文件文件之前请确认您已经在谷歌后台完成了谷歌支付必要的配置， AppWheel 必要的前期准备 - Android ，当您完成了文档中的配置您已经获取到了一个json文件，请把这个文件上传到我们的后台来，依此创建您的Android版本的app

### 第二步：应用配置
#### 通知配置（<font color="red"> **重要**</font>）可直接将Google 服务器通知发送Appwheel，请将以下地址（[https://msgserver-dot-subscription-saas.uc.r.appspot.com/subcenter/receive_android_notification](https://msgserver-dot-subscription-saas.uc.r.appspot.com/subcenter/receive_android_notification)）配置到Google Cloud Platform。

![notificationReceivingConfigurationAndroid](/img/integration/notificationReceivingConfigurationAndroid.png)

#### 商品配置
##### 对接前已有商品配置
##### 对接前没有商品配置


### 第四步：验收检查
- □App 配置检查
	- □通知是否配置成功：检查 Google Cloud Pub/Sub 队列是否有推送设置


![pubSub](/img/integration/pubSub.png)

	- □ T  [ ] 基础配置如下
![androidSetting](/img/integration/androidSetting.png)

- □ 是否在启动时调用了初始化接口
- □ 是否能通过 appwheel sdk 获取购买的商品信息
	- □ 订阅商品
	- □ 应用内商品
	- □ 优惠商品（选填）
		- □ SinglePayment商品
		- □ Recurring Payment商品
- □ 是否能通过 appwheel sdk 恢复商品信息
### 其它步骤：对接高级功能

