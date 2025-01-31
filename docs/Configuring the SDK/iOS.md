
 The SDK initialization method is as follows. Please initialize the SDK in your app as soon as possible according to your own situation.


import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';

<Tabs>
  <TabItem value="Objective-C" label="Objective-C">

```Objective-C 
///Please use the following method for versions older than 2.0.1.3.  
AWPurchaseKit configureWithAppId:appid 
                             uid:userId     
                      completion:^(BOOL success, AWError * _Nonnull error) {
    if (success) {
      //init success ,do something
    } else {
      // init failed,check error
    }
  }
  
  ///Use the following method for version 2.0.1.3 and above.  
[AWPurchaseKit configureWithAppId:appId 
                           secret:appSecret
                              uid:userId
                       completion:^(BOOL success, AWError * _Nonnull error) {
                 if (success) {
                  //init success ,do something
                } else {
                  // init failed,check error
                }
  }];
```
  </TabItem>
  <TabItem value="Swift" label="Swift">

```Swift
///Please use the following method for versions older than 2.0.1.3.  
AWPurchaseKit.configure(withAppId: appId, 
                              uid: uid) { [weak self](success, error) in
      if success == false {
        // init failed,check error
      } else {
        //init success ,do something
      }
    }
    
 ///Use the following method for version 2.0.1.3 and above.
 AWPurchaseKit.configure(withAppId: appId, 
                            secret:appSecret 
                               uid: uid) { [weak self](success, error) in
      if success == false {
        // init failed,check error
      } else {
        //init success ,do something
      }
    }
```
  </TabItem>
</Tabs>

### Parameters:
- appId: Generated by AW server. For the generation steps, please refer to the document: New Application (External Use)
- secret: Added in V2.0.1.3 and is generated by AW server. For the generation steps, please refer to the document: New Application (External Use)
- uid: userId should not be empty value.
- completion: Initialization result's block. Returns 'true' if initialization is successful; returns 'false' if not. This component can not be used in the case of configuration failure. 