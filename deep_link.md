# Deep Link 
深層連結（Deep Link）是一種讓使用者能透過連結直接進入應用程式內的特定頁面，而不是僅開啟應用程式的主頁。
* URL Scheme
```
//send
import 'package:url_launcher/url_launcher.dart';
launchUrl(Uri.parse("myapp://open?data=hello"));

//receive
import 'package:uni_links/uni_links.dart';
Future<void> initDeepLink() async {
  // App 被其他 App 呼叫啟動時的 URL
  final initial = await getInitialLink();
  if (initial != null) {
    print("App 是透過外部傳入的：$initial");
    handleLink(initial);
  }
}

//在背景啟動
linkStream.listen((String? link) {
  if (link != null) {
    print("收到新的外部啟動參數：$link");
    handleLink(link);
  }
});

void handleLink(String link) {
  final uri = Uri.parse(link);

  // 路徑
  print(uri.path);

  // 取得參數
  print(uri.queryParameters['data']);  
}

[與go_router合作]
final router = GoRouter(
  routes: [
    GoRoute(
      path: '/',
      builder: (_, __) => const HomePage(),
    ),
    GoRoute(
      path: '/product/:id',
      builder: (_, state) {
        final id = state.params['id'];
        return ProductPage(productId: id!);
      },
    ),
  ],
);

import 'package:uni_links/uni_links.dart';
import 'package:go_router/go_router.dart';

Future<void> initDeepLinks(GoRouter router) async {
  // App cold start 時的 URL
  final initial = await getInitialLink();
  if (initial != null) {
    router.go(Uri.parse(initial).path);
  }

  // App 已啟動狀態下收到 Deep Link
  linkStream.listen((String? link) {
    if (link != null) {
      final uri = Uri.parse(link);
      router.go(uri.path);
    }
  });
}

void main() async {
  final router = GoRouter([...]);

  WidgetsFlutterBinding.ensureInitialized();
  await initDeepLinks(router);

  runApp(MyApp(router: router));
}


[schema]
iOS
<key>CFBundleURLTypes</key>
<array>
  <dict>
    <key>CFBundleURLSchemes</key>
    <array>
      <string>myapp</string>
    </array>
  </dict>
</array>

Android
<intent-filter>
    <action android:name="android.intent.action.VIEW"/>
    <category android:name="android.intent.category.DEFAULT"/>
    <category android:name="android.intent.category.BROWSABLE"/>
    <data android:scheme="myapp"/>
</intent-filter>


[canLaunchUrl]
iOS
<key>LSApplicationQueriesSchemes</key>
<array>
  <string>sms</string>
  <string>tel</string>
</array>

Android
<!-- Provide required visibility configuration for API level 30 and above -->
<queries>
  <!-- If your app checks for SMS support -->
  <intent>
    <action android:name="android.intent.action.VIEW" />
    <data android:scheme="sms" />
  </intent>
  <!-- If your app checks for call support -->
  <intent>
    <action android:name="android.intent.action.VIEW" />
    <data android:scheme="tel" />
  </intent>
  <!-- If your application checks for inAppBrowserView launch mode support -->
  <intent>
    <action android:name="android.support.customtabs.action.CustomTabsService" />
  </intent>
</queries>

URL
mailto:smith@example.org?subject=News&body=New%20plugin
https://flutter.dev
el:+1-555-010-999
sms:5550101234
file:/home
```
* App Link
* Universal Link