## 取得目前裝置上的 App 版本
```
import 'package:package_info_plus/package_info_plus.dart';

Future<String> getInstalledVersion() async {
  final info = await PackageInfo.fromPlatform();
  return info.version; // e.g. "1.2.3"
}

```

## 取得 Google Play Store 最新版本
```
import 'package:new_version_plus/new_version_plus.dart';

final newVersion = NewVersionPlus(
  androidId: 'com.yourcompany.yourapp', // ← 改成你的 package name
);

Future<void> checkUpdate() async {
  final status = await newVersion.getVersionStatus();
  if (status != null) {
    print('Store version: ${status.storeVersion}');
    print('Local version: ${status.localVersion}');
    print('Can update: ${status.canUpdate}');
  }
}

newVersion.showUpdateDialog(
  context: context,
  versionStatus: status!,
  dialogTitle: '有新版本！',
  dialogText: '目前商店版本是 ${status.storeVersion}，請更新以獲得最佳體驗。',
  updateButtonText: '前往更新',
);
```

## iOS 也支援
```
NewVersionPlus(
  androidId: 'com.example.myapp',
  iOSId: '1234567890', // Apple Store app id
);
```