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
1. AndroidManifest.xml > <intent-filter> (預設有google assistant喚醒) &&  android:resource="@xml/actions" /
2. android/app/src/main/res/values/strings.xml > <string name="app_name">小創</string>
3. Ok Google，開啟小創，新增訂單 > android/app/src/main/res/xml/actions.xml
<action intentName="actions.intent.CREATE_ORDER">
    <parameter name="customer" type="text" />
    <parameter name="amount" type="number" />

    <fulfillment>
        <intent
            action="android.intent.action.VIEW"
            targetPackage="your.package.name"
            targetClass="your.package.name.MainActivity">
        </intent>
    </fulfillment>
</action>
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

## Android, iOS 差異
|            | Google Assistant | Siri  |
| ---------- | ---------------- | ----- |
| 喚醒 App     | ✅ 穩定             | ❌ 不保證 |
| 中文解析       | 👍 很強            | 👍    |
| 不加捷徑       | ✅ 可以             | ❌ 不行  |
| Flutter 整合 | 😄 簡單            | 😅 較複 |
