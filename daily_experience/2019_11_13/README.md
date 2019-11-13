# Amazon 内购充值记录



[官方文档说明](https://developer.amazon.com/zh/docs/in-app-purchasing/iap-overview.html)

[说明资料](./Amazon_IAP_eBook.pdf)

[官方例子+SDK](https://developer.amazon.com/zh/apps-and-games/sdk-download)

[测试流程](,/Amazon App Tester工具的操作流程.docx)

[api说明]( https://developer.amazon.com/zh/docs/in-app-purchasing/iap-implement-iap.html )







可购买项目的类型
IAP 包括三种不同类别的可购买项目：

消费品： 先进行购买，然后在应用中使用，如额外生命、额外移动或游戏内货币。可多次购买。
权利： 一次性购买，用于允许访问应用或游戏中的功能或内容。
订阅： 允许访问一组高级内容或功能有限的一段时间。





## 关于 Android IAP 程序包

`com.amazon.device.iap` 程序包提供了用于在 Android 应用中实现应用内购买的类和界面。

此程序包包含以下接口和类：

- **ResponseReceiver**： 从 Amazon Appstore 接收广播意图的类。
- **PurchasingService**： 通过 Amazon Appstore 发出请求的类。
- **PurchasingListener**： 接收 `PurchasingService` 发出的请求的异步响应的接口。

下表显示了 `PurchasingService` 的请求方法和相关联的 `PurchasingListener` 响应回调。这些是您在实现 IAP API 时最常用的方法、回调和响应对象：

| PurchasingService 方法 | PurchasingListener 回调     | 响应对象                |
| :--------------------- | :-------------------------- | :---------------------- |
| getUserData()          | onUserDataResponse()        | UserDataResponse        |
| getPurchaseUpdates()   | onPurchaseUpdatesResponse() | PurchaseUpdatesResponse |
| getProductData()       | onProductDataResponse()     | ProductDataResponse     |
| purchase()             | onPurchaseResponse()        | PurchaseResponse        |
| notifyFulfillment()    | 无                          | 无                      |

有关这些类和方法的更多信息，请参阅 [IAP v2.0 API 参考文档](https://s3-us-west-1.amazonaws.com/devportal-reference-docs/iap/API-Reference/index.html)。

### ResponseReceiver

应用内购买 API 以异步方式执行其所有活动。您的应用需要通过 `ResponseReceiver` 类从 Amazon Appstore 接收广播意图。此类从不在您的应用中直接使用，但要使您的应用接收意图，您必须将 `ResponseReceiver` 条目添加到 `AndroidManifest.xml` 文件中。以下代码段显示了如何将 ResponseReceiver 添加到 IAP v2.0 的 AndroidManifest.xml 文件中：

```
 <application>
  ...
  <receiver android:name = "com.amazon.device.iap.ResponseReceiver"
      android:permission = "com.amazon.inapp.purchasing.Permission.NOTIFY" >
    <intent-filter>
      <action android:name = "com.amazon.inapp.purchasing.NOTIFY" />
    </intent-filter>
  </receiver>
  ...
 </application>
```

### PurchasingService

使用 `PurchasingService` 类来检索各类信息，执行购买，并将购买的履行情况通知亚马逊。`PurchasingService` 实现以下方法：

- `registerListener(PurchasingListener purchasingListener)`： 在 `PurchasingService` 类中调用其他方法之前先调用此方法。
- `getUserData()`： 调用此方法来检索当前登录的用户的特定于应用的 ID 和市场。例如，如果用户切换账户或者多个用户在同一设备上访问您的应用，则此调用将帮助您确保您检索的收据针对当前用户账户。
- `getPurchaseUpdates(boolean reset)`： GetPurchaseUpdates 跨所有设备检索所有订阅和权利购买。只能从在其中购买消费品的设备检索消费品购买。GetPurchaseUpdates 只检索未履行的和已取消的消费品购买。亚马逊建议您保留返回的 `PurchaseUpdatesResponse` 数据并仅向系统查询更新。响应会分页。
- `getProductData(java.util.Set skus)`： 调用此方法来检索一组 SKU 的项目数据以显示在您的应用中。
- `purchase(java.lang.String sku)`： 调用此方法以发起特定 SKU 的购买。
- `notifyFulfillment(java.lang.String receiptId, FulfillmentResult fulfillmentResult)`： 调用此方法以发送指定 `receiptId` 的 `FulfillmentResult`。`FulfillmentResult` 的可能值为 FULFILLED 或 UNAVAILABLE。

### PurchasingListener

实现 `PurchasingListener` 接口来处理异步回调。因为您的 UI 线程调用这些回调，所以不要在 UI 线程中处理长时间运行的任务。您的 `PurchasingListener` 实例应实现以下方法：

- `onUserDataResponse(UserDataResponse userDataResponse)`： 在调用 getUserData() 后调用。确定当前登录用户的 `UserId` 和 `marketplace`。
- `onPurchaseUpdatesResponse(PurchaseUpdatesResponse purchaseUpdatesResponse)`： 在调用 `getPurchaseUpdates(boolean reset)` 后调用。检索购买历史记录。亚马逊建议您保留返回的 `PurchaseUpdatesResponse` 数据并仅向系统查询更新。
- `onProductDataResponse(ProductDataResponse productDataResponse)`： 在调用 `getProductDataRequest(java.util.Set skus)` 后调用。检索有关要从您的应用销售的 SKU 的信息。在 `onPurchaseResponse`() 中使用有效的 SKU。
- `onPurchaseResponse(PurchaseResponse purchaseResponse)`： 在调用 `purchase(String sku)` 后调用。用于确定购买的状态。

### ResponseObjects

您通过 `PurchasingService` 发出的每个调用都会使 `PurchasingListener` 收到相应响应。其中每个响应都使用响应对象：

- `UserDataResponse` 提供当前登录用户的特定于应用的 `UserId` 和 `marketplace`。
- `PurchaseUpdatesResponse`： 提供收据的分页列表。收据是无序的。
- `ProductDataResponse`： 提供以 SKU 为键的项目数据。`getUnavailableSkus()` 方法列出不可用的任何 SKU。
- `PurchaseResponse`： 提供在您的应用内发起的购买的状态。请注意，`PurchaseResponse.RequestStatus` 结果为 FAILED 可能只表示用户在完成之前取消了购买。







adb push D:/work/Amazon-Android-SDKs/AmazonInAppPurchasing/examples/SampleIAPEntitlementsApp/amazon.sdktester.json /mnt/sdcard/





adb push D:/work/Amazon-Android-SDKs/AmazonInAppPurchasing/examples/SampleIAPSubscriptionsApp/amazon.sdktester.json /mnt/sdcard/































开源链接

[url_github] : ( https://github.com/EmmyLua )

[url_doc] : ( https://emmylua.github.io/ )





















[url_github]: 