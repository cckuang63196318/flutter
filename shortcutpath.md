## iOS
```
1. Runner > Signing & Capabilities > +Capability > Siri
2. +AddOrderIntent.swift: (也可以讓siri處理參數, 新增訂單 給王小明 金額 3000)
NotificationCenter.default.post(
    name: NSNotification.Name("ADD_ORDER_FROM_SIRI"),
    object: nil
)
3. AppDelegate:
NotificationCenter.default.addObserver(
    self,
    selector: #selector(handleAddOrder),
    name: NSNotification.Name("ADD_ORDER_FROM_SIRI"),
    object: nil
)
@objc func handleAddOrder() {
    methodChannel?.invokeMethod("addOrder", arguments: nil)
}
4. flutter
final MethodChannel _siriChannel =
    const MethodChannel('siri_channel');

void initSiriListener(BuildContext context) {
  _siriChannel.setMethodCallHandler((call) async {
    if (call.method == 'addOrder') {
      Navigator.pushNamed(context, '/addOrder');
    }
  });
}
5. 打開 捷徑 App > 新增訂單 > 加入捷 > 小創 新增訂單
6. 「Hey Siri，小創 新增訂單」or「Hey Siri，使用小創 新增訂單」or 「Hey Siri，開啟小創 新增訂單」- 不穩定
```

## Android
```
1. AndroidManifest.xml > <intent-filter> (預設有google assistant喚醒)
2. android/app/src/main/res/values/strings.xml > <string name="app_name">小創</string>
3. Ok Google，開啟小創，新增訂單:
<intent-filter>
    <action android:name="android.intent.action.VIEW"/>
    <category android:name="android.intent.category.DEFAULT"/>
    <category android:name="android.intent.category.BROWSABLE"/>

    <data android:scheme="xiaochuang"
          android:host="add"/>
</intent-filter>
4. res/xml/shortcuts.xml:
<shortcuts>
    <shortcut
        android:shortcutId="add_item"
        android:enabled="true"
        android:shortcutShortLabel="新增">
        <intent
            android:action="android.intent.action.VIEW"
            android:data="xiaochuang://add" />
    </shortcut>
</shortcuts>
5. MainActivity 接收 Intent → 傳給 Flutter
override fun onNewIntent(intent: Intent) {
    super.onNewIntent(intent)
    val uri = intent.data
    if (uri?.host == "add") {
        methodChannel.invokeMethod("openAddPage", null)
    }
}
6. flutter:
MethodChannel('assistant_channel')
  .setMethodCallHandler((call) async {
    if (call.method == 'openAddPage') {
      Navigator.pushNamed(context, '/add');
    }
  });
```