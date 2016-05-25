* YaokanSDK For iOS has been fully release the first version 1.0!

## YaokanSDK
* YaokanSDK is the most popular remote control SDK for apps in global world ! We've already support over 100,000 devices until now.
YaokanSDK could easily support 10+ device platforms in the world for third-party remote controller. Only few minutes, this small package will make your app fully remote control!

## 新手入门
* [注册应用](#create_app_id)
* [添加 SDK](#add_sdk)
* [配置项目](#configuration)
* [功能集成](#integration)
* [示例项目](#demo)

### <a id="create_app_id"></a>注册应用
暂无

### <a id="add_sdk"></a>添加 SDK

* **使用 CocoaPods 添加：**
  ```objc
  # YaokanSDK
  pod "YaokanSDK"
  ```

* **手动添加：**

  1. 下载最新的 YaokanSDK, 拖进您的项目

     * 1) 从官网下载最新版本的 YaokanSDK，解压得到 `YaokanSDK.framework` 和 `YaokanSDK.bundle`。

     * 2) 将这两个文件拖进项目 (或者按下键盘的 `control` 键，点击项目名称，选择 `Add Files to ...`)，在 `Choose options for adding these files:` 窗口中选中 `Copy items if needed`，然后点击 `Finish` 按钮。

  2. 添加依赖框架

    添加系统依赖框架，*<TARGET NAME>* -> Build Phases -> Link Binary With Libraries -> + (Add items)：
    ```objc
    SystemConfiguration.framework
    MobileCoreServices.framework
    MediaPlayer.framework
    AudioToolbox.framework
    AVFoundation.framework
    ```

  3. 添加编译选项

    在 Build Settings 中找到 `Other Linker Flags`，添加 `-all_load`。

### <a id="configuration"></a>配置项目

  仅适用于 `iOS 9` 及以上版本，将 Yaokan 服务器添加到白名单：
  ```objc
  <key>NSAppTransportSecurity</key>
	<dict>
		<key>NSExceptionDomains</key>
		<dict>
			<key>www.yaokantv.com</key>
			<dict>
				<key>NSExceptionAllowsInsecureHTTPLoads</key>
				<true/>
				<key>NSExceptionRequiresForwardSecrecy</key>
				<false/>
				<key>NSIncludesSubdomains</key>
				<true/>
				<key>NSThirdPartyExceptionRequiresForwardSecrecy</key>
				<false/>
			</dict>
		</dict>
	</dict>
  ```

### <a id="integration"></a>功能集成
1. 导入头文件
  ```objc
  #import <YaokanSDK/YaokanSDK.h>
  ```

2. 使用 [注册应用](#create_app_id) 中注册到的 `AppKey` 和 `Secret` 向服务器注册：

  ```objc
  -	(BOOL)application:(UIApplication )application didFinishLaunchingWithOptions:(NSDictionary )launchOptions
  {
         [YaokanSDK registerApp:@"xxx" secret:@"xxx"];; // *** is the AppKey and Secret that you just got
         //……
         return YES;
  }
  ```

3. 创建遥控器
  ```objc
  [YaokanSDK startCreateDeviceIn:self.navigationController
                      withCompletion:^(NSArray *obj, NSError *error) {
                          self.devices = [NSMutableArray arrayWithArray:obj];
                          [self.tableView reloadData];
                      }];
  ```

4. 获取创建的遥控器列表
  ```objc
  [YaokanSDK fetchDeviceList:^(NSArray *obj, NSError *error) {
          self.devices = [NSMutableArray arrayWithArray:obj];
          [self.tableView reloadData];
      }];
  ```

5. 进入遥控器界面
  ```objc
  // self 的父类容器必须有 UINavigationController
  [YaokanSDK openDevicePannelIn:self withDevice:device];
  ```

6. 删除遥控器
  ```objc
  RemoteDevice *device = self.devices[indexPath.row];

  if ([YaokanSDK deleteDevice:device]) {
      // Delete the row from the data source.
      [self.devices removeObjectAtIndex:indexPath.row];
      [tableView deleteRowsAtIndexPaths:[NSArray arrayWithObject:indexPath]
                       withRowAnimation:UITableViewRowAnimationFade];
  }
  ```

### <a id="demo"></a>示例项目
* [YaokanSDK Demo](http://github.com/yaokantv/YaokanSDK)
