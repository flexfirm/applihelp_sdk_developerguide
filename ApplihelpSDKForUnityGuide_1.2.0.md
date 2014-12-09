Applihelp SDK for Unity（Android and iOS）
==================================================
[Applihelp](http://www.applihelp.com) |
[Facebook](http://facebook.com/applihelp) |
[Twitter](http://twitter.com/applihelp)

---

<a name="TOC">Table of Contents</a>
--------------------------------------------------
1. [Introduction](#Introduction)
1. [Requirements](#Requirements)
1. [Versions](#Versions)
1. [Installtion](#Installtion)
1. [Usage](#Usage)
1. [Changelogs](#Changelogs)
1. [License](#License)

<a name="Introduction">Introduction</a>
--------------------------------------------------
ApplihelpはあなたのUnity（Android, iOS）アプリケーションにヘルプサポート機能を提供します。  
Applihelp SDKはお問い合わせメッセージの送信と回答の受信をサポートします。

**[[⬆]](#TOC)**

<a name="Requirements">Requirements</a>
--------------------------------------------------

### Unity
- 4.5.5f1

### Android
- Android 2.3.3(API Level10) or later
- JDK 6 or later

### iOS
- iOS5.1.1 or later
- Xcode 6.0 or later
- iOS SDK8.0 or later

**[[⬆]](#TOC)**

<a name="Versions">Versions</a>
--------------------------------------------------
Applihelp for Unityの内部で使用しているApplihelp SDKのバージョンはそれぞれ以下の通りです。

- Applihelp for Android 1.4.0
- Applihelp for iOS 1.3.1

**[[⬆]](#TOC)**

<a name="Installtion">Installation</a>
--------------------------------------------------  

### SDKについて
SDKを利用するためには事前に[Applihelp](http://console.applihelp.com)のアカウントを取得し、以下の情報を登録している必要があります。

<small>[Android]</small>

- Androidアプリケーションパッケージ名(例：com.example.sample)  
- Google API Key(PUSH通知を利用する場合)  

<small>[iOS]</small>

- iOSアプリケーションAppID(例：333903271)  
- Apple Push Certificate(.p12)(PUSH通知を利用する場合)  

### SDKのインポート
SDKを利用したいUnityプロジェクトにおいて、以下の手順でapplihelp1.2.0.unitypackageをインポートします。  
Unityの統合開発環境で、Assets > Import Package > Custom Package...を選択し、applihelp1.2.0.unitypackageを選択します。  
インポートするファイルの確認が済んだらImportボタンをクリックします。  

**Androidで使用する際の注意**

Applihelpでは以下の外部ライブラリを利用しています。  

+ android-async-http-1.4.3.jar
+ android-support-v4.jar
+ google-play-services_lib

**既に利用しているライブラリがある場合は、インポートする際にチェックをはずしてください。**  
※ google-play-services_libのチェックをはずす場合は、チェックの箇所が複数に分かれる場合がありますのでご注意ください。  
※ google-play-services.jar を直接アプリケーション内にコピーするとクラッシュする場合があります。  
Androidライブラリプロジェクトとして取り込み、アプリと一緒にビルドする必要があります。

applihelp1.2.0.unitypackage展開後のフォルダ構成は下記の通りです。

<pre>
/
└─Assets	// UnityのAssetsフォルダです。
   └─Plugins
  　　 ├─Android		//Applihelp SDKを含む、Android関連プログラム一式
 　　  |　├─libs
  　　 |　|　├─apphelp_sdk.jar		//Applihelp SDK本体
   　　|　|  ├─android-async-http-1.4.3.jar //Android 外部ライブラリ
   　　|　|  └─android-support-v4.jar //Android 外部ライブラリ
   　　|　|
   　　|　├─res		//Applihelp SDKが利用するresファイル群
   　　|　├─Applihelp_AndroidManifest_sample.xml
   　　|　├─applihelp_plugin.jar		//Applihelp SDKのメソッドを呼び出す関数群
   　　|  └─google-play-services_lib //Android 外部ライブラリ
   　　|　
   　　├─AppliHelp
   　　|　└─Scripts
   　　|　　　├─AppHelpPlugin.cs　//※このクラスの各メソッドでAppliHelpの各機能を利用します
   　　|　　　├─AppHelpPluginAndroid.cs　　//Androidのapplihelp_plugin.jarのメソッドを呼び出す関数群
   　　|　　　├─AppHelpPluginIOS.cs　　//iOSのApplihelpライブラリを呼び出す関数群
   　　|　　　└─AppHelpUsingSample.cs　　　//AppHelpPlugin.csを利用するサンプルプログラム
   　　|　
  　　 └─iOS		//Applihelp SDKを含む、iOS関連プログラム一式
　　　　  ├─ApplihelpAppController.mm  //iOSでプッシュ通知を受け取るためのコード
　　　　  ├─ApplihelpSDK.h  //ApplihelpSDK ヘッダーファイル
　　　　  ├─libapphelp.a  //ApplihelpSDK 本体
　　　　  └─libapplihelpUnityPlugin.a //ApplihelpSDK Unityプラグイン
</pre>

Androidの外部ライブラリの詳細についてはApplihelp SDK for Androidの[Installation](https://github.com/flexfirm/applihelp_android_sdk/blob/master/DeveloperGuide_1.4.0.md#installation)「外部ライブラリ追加」をご参照ください。

### Android固有の設定

#### AndroidManifest.xmlの編集
SDKを利用するためにあなたのUnityプロジェクトの`Assets/Plugins/Android/AndroidManifest.xml`を編集してください。  
（もし上記の場所に`AndroidManifest.xml`が存在しない場合は  
`Assets/Plugins/Android/Applihelp_AndroidManifest_sample.xml`を参考にして作成してください。）

##### Minimum SDK Version  
android:minSdkVersionは10以上を指定してください。

```xml
<uses-sdk
	android:minSdkVersion="10"
	android:targetSdkVersion="18" />
```

##### Permission、Activity、Receiver
Permission、Activity、Receiverに関するAndroidManifest.xmlの詳細についてはApplihelp SDK for Androidの[Installation](https://github.com/flexfirm/applihelp_android_sdk/blob/master/DeveloperGuide_1.4.0.md#installation)「AndroidManifest.xmlの編集」をご参照ください。

### iOS固有の設定
Unityから出力された、iOSのアプリケーションプロジェクトに、以下の設定を行ってください。

#### フレームワーク追加
Applihelp SDKが必要とする以下のフレームワークをアプリケーションプロジェクトに追加してください。

- CoreFoundation.framework
- CoreGraphics.framework
- CoreTelephony.framework
- CoreText.framework
- Foundation.framework
- libsqlite3.0.dylib
- Security.framework
- SystemConfiguration.framework
- UIKit.framework

#### リンカフラグ設定
アプリケーションプロジェクトのビルド設定`Other Linker Flags`に以下2つの設定を追加してください。
Static Libraryに含まれるカテゴリクラスを利用するために必要な設定です。

 - `-all_load` **※**
 - `-ObjC`

**※** アプリケーションで使用しているライブラリによっては "duplicate symbol" エラーが発生する場合があります。その場合は替わりに `-force_load {環境に応じたパス}/libapphelp.a ` を指定してください。

#### リソースファイル配置
`APHResources`に含まれる全てのリソースファイルをアプリケーションプロジェクトのビルド設定`Copy Bundle Resources`に追加してください。SDKが使用する画像および言語ファイルが含まれています。
**ファイル名は変更しないでください。**

##### 言語
- `APHResources/languages/en.lproj/APHLocalizable.strings`
- `APHResources/languages/ja.lproj/APHLocalizable.strings`

##### 画像
- `APHResources/images/aph-bubble-min@3x.png`
- `APHResources/images/aph-bubble-min@2x.png`
- `APHResources/images/aph-bubble-min.png`

##### テーマ
- `APHResources/themes/APHTheme.plist`

**[[⬆]](#TOC)**

<a name="Usage">Usage</a>
--------------------------------------------------
### Applihelpの初期設定を行う  

<h3 name="initialize">SenderID設定 <small>[Android]</small></h3>
このメソッドによりGCMのSenderIDをセットします。  
Push通知(GCM)を利用する場合は、__Applihelpの各機能を利用する前に必ず実行してください。__  
Push通知(GCM)を利用しない場合は実行する必要はありません。
```CS
//(C#)
void Start()
{
#if UNITY_ANDROID
	//GCMのSenderIDを設定
	AppHelpPlugin.setSenderID("Your-Sender-ID");
#endif
}
	
```
- `Your-Sender-ID`：GCM(Google Cloud Messaging)のSender ID  
まだSender IDを取得していない場合、以下を参考にSender IDを取得してください。  
[Google Cloud Messaging Getting Started](http://developer.android.com/google/gcm/gs.html)  

<h3 name="initialize">AppID設定 <small>[iOS]</small></h3>
__Applihelpの各機能を利用する前に必ず実行してください。__  
iTunes Connectで発行したアプリケーションのAppIDを指定してください。  

```CS
//(C#)
void Start()
{
#if UNITY_IOS
	//AppIDを設定
	AppHelpPlugin.setAppID("Your-Application-AppID");
#endif
}
	
```

### Applihelp　FAQ画面、もしくは問合せ履歴を表示 <small>[Android]</small> <small>[iOS]</small>
FAQが登録されていればFAQ画面を、登録されていなければ問合せ履歴を表示します。 

```CS
//(C#)
AppHelpPlugin.showAppHelp();
```

例えばボタンを設置してこのメソッドをを呼び出す場合は以下のようになります。  
```CS
//(C#)
void OnGUI()
{
    if (GUI.Button(new Rect(50,50, 200, 100), "ヘルプ"))
    {
        AppHelpPlugin.showAppHelp();
    }
}

```

</br>
### Applihelp　FAQ画面表示 <small>[Android]</small> <small>[iOS]</small>
ApplihelpのFAQ画面を表示します。 

```CS
//(C#)
AppHelpPlugin.showFaq();
```

</br>

### Applihelp　お問い合わせ履歴画面表示 <small>[Android]</small> <small>[iOS]</small>
Applihelpのお問い合わせ履歴画面表示を表示します。  

```CS
//(C#)
AppHelpPlugin.showAppHelp();
```

</br>


### お問い合わせ画面表示 <small>[Android]</small> <small>[iOS]</small>


Applihelpのお問い合わせ画面を表示します。  
ユーザ名が未設定の場合はプロフィール入力画面が表示されます。  
```CS
//(C#)
AppHelpPlugin.showRegisterIssue();
```

</br>

### ユーザ名取得・設定 <small>[Android]</small> <small>[iOS]</small>
キャッシュに保存されているユーザ名を取得します。  
```CS
//(C#)
string userName = AppHelpPlugin.getUserName();
```

お問い合わせ時のユーザ名をあらかじめ設定することが可能です。  
初回お問い合わせ時にユーザ名の入力を求められます。しかし、事前にユーザ名を設定することによって、それをスキップすることが可能です。  
```CS
//(C#)
AppHelpPlugin.setUserName("User-Name");
```
</br>

### カスタム情報を設定する <small>[Android]</small> <small>[iOS]</small> 
アプリで取得できる任意の情報（以降、「カスタム情報」）を、ApplihelpのWebコンソールで確認することができます。
 任意のkeyとvalueの組み合わせで複数のカスタム情報を以下のように設定できます。
この情報は、以下のメソッドを呼び出した後のユーザーの問合せ送信ごと、もしくはメッセージ送信ごとに一緒に送信されます。
 送信した情報は、Messages画面で最新の1件のみ確認できます。  
```CS
//(C#)
Dictionary<string,string> dictionary 
	= new Dictionary<string,string> ();
dictionary.Add( "user_id", "3000" );
dictionary.Add( "name", "たけし" );
dictionary.Add( "in_app_purchase", "true" );
AppHelpPlugin.setCustomInfo(dictionary);
```  

カスタム情報をクリアする場合は以下のメソッドを呼び出します。  
```CS
//(C#)
AppHelpPlugin.clearCustomInfo();
```  

</br>

### GCM登録ID取得・設定 <small>[Android]</small>
キャッシュに保存されているGCM登録IDを取得します。  
```CS
//(C#)
string GcmRegistrationId = AppHelpPlugin.getGcmRegistrationId();
```
<br>

GCM登録IDをあらかじめ設定することが可能です。  
Applihelpを内部で初期化する際にGCM登録IDを取得します。  しかし、事前にGCM登録IDを設定することによって、それをスキップすることが可能です。  
```CS
//(C#)
AppHelpPlugin.setGcmRegistrationId("GCM-Registration-ID");
```
</br>
### 新着通知数取得 <small>[Android]</small> <small>[iOS]</small>
1度も取得されていない未読メッセージの件数を取得します。 
```CS
//(C#)
AppHelpPlugin.getNotificationCount("ReceiveNotification-GameObjectName");
```
`ReceiveNotification-GameObjectName`：新着通知数を受け取るgameObject名
<br>

問い合わせた際の戻り値（新着通知数）は以下のコールバック
メソッドで取得します。  
取得に失敗した場合はcountに"fail"が渡されます。  

**なお、このコールバックメソッドを記述したscriptは  
`AppHelpPlugin.getNotificationCount("ReceiveNotification-GameObjectName")`メソッドで指定したGameObjectにアタッチされている必要があります**

```CS
//(C#)
public void getNotificationCountCallBack(string count){
	if (count != "fail") {
		Debug.Log("新着通知数＝"+count+"件です");
	}
}
```
</br>

### PUSH通知受信
お問い合わせに対して新着回答がある場合、PUSH通知を受信することが可能です。  

#### Android(GCM)
Push通知受信時の端末の挙動（通知メッセージや、バイブ・音・ライトなど）は  
`Assets/Plugins/Android/res/values-ja/ah_unity_strings.xml`で設定します。  
設定値の詳細は上記ファイルのコメントをご参照ください。

#### Android(GCMをカスタマイズする)
Push通知受信時の挙動をカスタマイズする場合は、BroadcastReceiverを持つ独自のプラグインjarファイルを作成する必要があります。  
BroadcastReceiverの実装例は下記ガイドの「PUSH通知受信(GCM)をカスタマイズする」をご参照ください。  
(https://github.com/flexfirm/applihelp_android_sdk/blob/master/DeveloperGuide_1.4.0.md#usage)  

#### iOS
アプリでPUSH通知を有効にするためには Apple Developer での設定が必要です。設定は以下を参考にしてください。  
[Local および Push Notification プログラミングガイド | Apple Developer](https://developer.apple.com/jp/devcenter/ios/library/documentation/RemoteNotificationsPG.pdf)  

実装は、Applihelp SDK for iOSの[usage](https://github.com/flexfirm/applihelp_ios_sdk/blob/master/DeveloperGuide_1.3.1.md#usage)「Push通知受信」をご参照ください。
その際、`AppDelegate`クラスは`Assets/Plugins/iOS/ApplihelpAppController.mm`になります。

### 文字列
<small>[Android]</small>  
Applihelpが使用する文字列については、Applihelp SDK for Androidの[usage](https://github.com/flexfirm/applihelp_android_sdk/blob/master/DeveloperGuide_1.4.0.md#usage)「文字列」をご参照ください。

<small>[iOS]</small>  
Applihelpが使用する文字列については、Applihelp SDK for iOSの[usage](https://github.com/flexfirm/applihelp_ios_sdk/blob/master/DeveloperGuide_1.3.1.md#usage)「Localizable.stringsの編集」及び「文字列」をご参照ください。

### 外観
<small>[Android]</small>  
Applihelpの外観については、Applihelp SDK for Androidの[usage](https://github.com/flexfirm/applihelp_android_sdk/blob/master/DeveloperGuide_1.4.0.md#usage)「外観」をご参照ください。

<small>[iOS]</small>  
Applihelpの外観については、Applihelp SDK for iOSの[usage](https://github.com/flexfirm/applihelp_ios_sdk/blob/master/DeveloperGuide_1.3.1.md#usage)「外観」をご参照ください。
### 不要になったファイル一覧
新しいバージョンのapplihelp.unitypackageをインポートした際、古いapplihelp.unitypackageにのみ存在するファイルはプロジェクトに残ったままとなります。  
Unityの仕組み上、不要になったファイルは自動で削除されないため、**ご自身で削除していただく必要があります**。  

以下、不要になったファイル一覧です。  
`Assets/Plugins/Android/res/layout/ah_register_profile_activity.xml`  
`Assets/Plugins/Android/apphelp4unity.jar`

**[[⬆]](#TOC)**

<a name="Changelogs">Changelogs</a>
--------------------------------------------------
- [Ver.1.2.0]Released on November 19, 2014  
	- [更新][Versions](#Versions)各OSのSDKバージョン
		- Android SDK [Ver.1.4.0]  
		- iOS SDK [Ver.1.3.1]
	- [Installation](#Installation)／Android固有の設定／AndroidManifest.xmlの編集  
		- [追加]Permission、Activity、Receiver
		- [削除]Permission
		- [削除]Activity
		- [削除]Receiver
	- [Usage](#Usage)
		- [削除]Applihelp　メイン画面表示
		- [追加]Applihelp　FAQ画面、もしくは問合せ履歴を表示
		- [追加]Applihelp　FAQ画面を表示
		- [追加]Applihelp　お問い合わせ履歴画面表示
		- [追加]Android(GCMをカスタマイズする)
		- [追加]不要になったファイル一覧  

- [Ver.1.1.0]Released on August 1, 2014  
	- iOS対応
	- 各OSのSDKバージョン
		- Android SDK [Ver.1.3.0]  
		- iOS SDK [Ver.1.2.0]
 
- [Ver.1.0.0]Released on July 23, 2014  
	- Android対応
	- 各OSのSDKバージョン
		- Android SDK [Ver.1.3.0]  

**[[⬆]](#TOC)**

<a name="License">License and Credits</a>
--------------------------------------------------
このソフトウェアは、以下のオープンソースソフトウェアを使用しています。  

**[Asynchronous Http Client](http://loopj.com/android-async-http/)
© James Smith All rights reserved.** <small>[Android]</small>  
The Android Asynchronous Http Client is released under the Android-friendly Apache License, Version 2.0. Read the full license here: [http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0)

**[AFNetworking](https://github.com/AFNetworking/AFNetworking) © 2013 AFNetworking (http://afnetworking.com/) All rights reserved.** <small>[iOS]</small>  
[MIT License.](http://opensource.org/licenses/MIT)

**[fmdb](https://github.com/ccgus/fmdb) © 2008 Flying Meat Inc. All rights reserved.** <small>[iOS]</small>  
[MIT License.](http://opensource.org/licenses/MIT)

**[HPGrowingTextView](https://github.com/HansPinckaers/GrowingTextView) © 2011 Hans Pinckaers All rights reserved.** <small>[iOS]</small>  
[MIT License.](http://opensource.org/licenses/MIT)

**[JSMessagesViewController](https://github.com/jessesquires/MessagesTableViewController) © 2013 Jesse Squires All rights reserved.** <small>[iOS]</small>  
[MIT License.](http://opensource.org/licenses/MIT)

**[OHAttributedLabel](https://github.com/AliSoftware/OHAttributedLabel) © 2010 Olivier Halligon All rights reserved.** <small>[iOS]</small>  
[MIT License.](http://opensource.org/licenses/MIT)

**[Reachability](https://developer.apple.com/library/ios/samplecode/reachability/Introduction/Intro.html) © Apple Inc. All rights reserved.** <small>[iOS]</small>  

**[SVProgressHUD](https://github.com/samvermette/SVProgressHUD) © 2011 Sam Vermette All rights reserved.** <small>[iOS]</small>  
Copyright (c) 2011 Sam Vermette

Permission is hereby granted, free of charge, to any person
obtaining a copy of this software and associated documentation
files (the "Software"), to deal in the Software without
restriction, including without limitation the rights to use,
copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the
Software is furnished to do so, subject to the following
conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES
OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND
NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT
HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY,
WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING
FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR
OTHER DEALINGS IN THE SOFTWARE.

A different license may apply to other ressources included in this package, 
including Joseph Wain's Glyphish Icons. Please consult their 
respective headers for the terms of their individual licenses.
**[[⬆]](#TOC)**

---
© [KSK Co., Ltd.](http://www.flexfirm.jp) All rights reserved.
