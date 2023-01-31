# [iOS/Android] APP HTTP 접근 허용하기


## APP HTTP 접근 허용 설정하기

### iOS
`ios/Runner/Info.plist`
```xml
	<key>NSAppTransportSecurity</key>
    <dict>
        <key>NSAllowsLocalNetworking</key>
        <true/>
        <key>NSAllowsArbitraryLoadsInWebContent</key>
        <true/>
    </dict>
</dict>
</plist>
```

### Android
`android/app/src/main/AndroidManifest.xml`

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.web_view">
    <uses-permission android:name="android.permission.INTERNET"/> <!-- 추가 -->
   <application
        android:label="web_view"
        android:name="${applicationName}"
        android:icon="@mipmap/ic_launcher"
        android:usesCleartextTraffic="true"> <!-- 추가 -->

```


