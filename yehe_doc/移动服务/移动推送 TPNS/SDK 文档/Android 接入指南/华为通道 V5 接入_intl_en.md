## Overview


TPNS always keeps up with the update progress of each vendor channel's push service. It provides plugin dependent packages integrated with the HMS Core Push SDK of Huawei Push for your choice.

> !  
> - For Huawei Push, you can successfully register with the Huawei channel and push messages through it only in a signed release package environment.
> - The Huawei channel supports click callback but not arrival callback.

## Application Configuration on Huawei Push Platform

### Obtaining a key

1. Go to the [Huawei Developer Platform](http://developer.huawei.com).
2. Register a developer account and log in to the platform. For more information, please see [Account Registration and Verification](https://developer.huawei.com/consumer/cn/devservice/doc/20300). (If you are registering a new account, identity verification is required.)
3. Create an application on the Huawei Push platform. For more information, please see [Creating an app](https://developer.huawei.com/consumer/cn/doc/distribution/app/agc-create_app). (The application package name must be the same as that entered in the TPNS console.)
4. Get and copy the application's `AppID` and `AppSecret` and paste them into **Configuration Management** > **Basic Configuration** > **Huawei Official Push Channel** of the [TPNS console](https://console.cloud.tencent.com/tpns).



### Configuring the SHA-256 certificate fingerprint

Get the SHA-256 certificate fingerprint as instructed in [Generating a Signing Certificate Fingerprint](https://developer.huawei.com/consumer/cn/doc/development/HMS-Guides/Preparations#generate_finger). Then configure the fingerprint on the Huawei Push platform, and **remember to click <img src="https://main.qcloudimg.com/raw/f74e3aa948316533ce91f9add4a81a29.png"></img> to save the configuration**.



### Getting the Huawei Push configuration file

Log in to the Huawei Developer platform, go to **My Projects**, select a project, click **Project Settings**, and download the latest configuration file `agconnect-services.json` of your Huawei application.


### Enabling the push service

1. On the Huawei Push platform, choose **All services** > **Push Kit** to go to the **Push Kit** page.

2. On the **Push Kit** page, click **Enable now**. For more information, please see [Enabling Services](https://developer.huawei.com/consumer/cn/doc/distribution/app/agc-enable_service#enable-service).


## SDK Integration (Two Methods)

### Using Android Studio Gradle for automatic integration

1. In the `build.gradle` file in the Android project-level directory, add the Huawei repository address and HMS Gradle plugin dependencies under **repositories and dependencies** in **buildscript**, respectively:
```
buildscript {
    repositories {
        google()
        jcenter()
        maven {url 'http://developer.huawei.com/repo/'}     // Huawei Maven repository address
    }
    dependencies {
        // Other `classpath` configurations
        classpath 'com.huawei.agconnect:agcp:1.3.1.300'     // Gradle plugin dependencies of Huawei Push
    }
}
```
2. In the `build.gradle` file in the Android project-level directory, add the Huawei dependency repository address under **repositories** in **allprojects**:
```
allprojects {
    repositories {
        google()
        jcenter()
        maven {url 'http://developer.huawei.com/repo/'}     // Huawei Maven repository address
    }
}
```
3. Copy the application configuration file `agconnect-services.json` obtained from the Huawei Push platform to the `app` module directory.  
 ![](https://main.qcloudimg.com/raw/338c87faaeb388f648835f17aeddc490.png)
4. Add the following configuration to the `build.gradle` file at its beginning in the `app` module:
```
// Other Gradle plugins of the application
apply plugin: 'com.huawei.agconnect'      // HMS Push SDK Gradle plugin
android {
    // Application configuration content
}
```
5. Import the dependencies related to Huawei Push into the `build.gradle` file under the `app` module:
```
dependencies {
		// ...Other dependencies of the application
		implementation 'com.tencent.tpns:huawei:[VERSION]-release'      //  For Huawei pushes, [VERSION] is the SDK's latest version number, which can be obtained from the release notes of SDK for Android.
		implementation 'com.huawei.hms:push:5.0.2.300'       // HMS Core Push module dependent package
		}
```

>?
>- For Huawei pushes, [VERSION] is the SDK's latest version number, which can be obtained from the release notes of [SDK for Android](https://intl.cloud.tencent.com/document/product/1024/36191).
>- Starting from v1.2.1.3, TPNS SDK for Android officially supports Huawei Push v5. Please use TPNS Huawei dependency v1.2.1.3 or above to avoid integration conflicts.


### Using Android Studio for manual integration

If you cannot access Huawei Maven repository in your internal development environment, you can try the following manual integration method:

1. Download the [SDK installation package](https://console.cloud.tencent.com/tpns/sdkdownload).
2. Open the `Other-Push-jar` folder and import the dependent packages related to Huawei Push v5 by copying all JAR and AAR packages into the project.
3. In the `build.gradle` file in the Android project-level directory, add HMS Gradle plugin dependencies under **dependencies** in **buildscript**:
```
buildscript {
    repositories {
        google()
        jcenter()
    }
    dependencies {
        // Other `classpath` configurations
        classpath files('app/libs/agcp-1.3.1.300.jar')     // Gradle plugin dependencies of Huawei Push
    }
}
```
4. Copy the application configuration file `agconnect-services.json` obtained from the Huawei Push platform to the `app` module directory.  
![](https://main.qcloudimg.com/raw/447e6453a5a996128b12c5ca9cdb10c7.png)
5. Add the following configuration to the `build.gradle` file at its beginning in the `app` module:
```
// Other Gradle plugins of the application
apply plugin: 'com.huawei.agconnect'      // HMS Push SDK v4 Gradle plugin
android {
    // Application configuration content
}
```
6. Import the dependencies related to Huawei Push into the `build.gradle` file under the `app` module:
```
dependencies {
        // ...Other dependencies of the application
        implementation files('libs/tpns-huaweiv5-1.2.1.1.jar')      // TPNS plugin for HMS Core
        implementation fileTree(include: ['*.aar'], dir: 'libs')    // HMS Core Push module dependent package
    }
```
7. Add the following components between the `<application>` and `</application>` tags in the `manifest` file:
```
<application>
        <service
            android:name="com.huawei.android.hms.tpns.HWHmsMessageService"
            android:exported="false">
            <intent-filter>
                <action android:name="com.huawei.push.action.MESSAGING_EVENT" />
            </intent-filter>
        </service>
</application>
```

## Huawei Push Activation

Enable the third-party push API before calling TPNS registration API `XGPushManager.registerPush`:

```
// Enable third-party push
XGPushConfig.enableOtherPush(getApplicationContext(), true);
```

The log of successful registration is as follows:

```
V/TPush: [XGPushConfig] isUsedOtherPush:true
E/xg.vip: get otherpush errcode: errCode : 0 , errMsg : success
V/TPush: [XGPushConfig] isUsedOtherPush:true
I/TPush: [OtherPushClient] handleUpdateToken other push token is : IQAAAACy0PsqAADxfCrWG3kupbOraeAiYoo9n2B-bAfb2d--kctc8E_UnY_mrIdg9ionukZvC******dVD8GlJi_5-0rpskunnNMcat35HA other push type: huawei
```

## Code Obfuscation

```plaintext
-ignorewarnings
-keepattributes *Annotation* 
-keepattributes Exceptions 
-keepattributes InnerClasses 
-keepattributes Signature 
-keepattributes SourceFile,LineNumberTable 
-keep class com.hianalytics.android.**{*;} 
-keep class com.huawei.updatesdk.**{*;} 
-keep class com.huawei.hms.**{*;}
-keep class com.huawei.agconnect.**{*;}
```


> ?Obfuscation rules must be stored in the `proguard-rules.pro` file at the app project level.

## Advanced Configuration (Optional)

### Arrival receipt configuration for Huawei channel

The arrival receipt for the Huawei channel should be configured by yourself. After configuring this feature as instructed in [Acquisition of Vendor Channel Arrival Receipt](https://intl.cloud.tencent.com/document/product/1024/35246), you can view the arrival data for the Huawei push channel in the push records.


### Badge adaptation for Huawei devices

You can set the application badge on Huawei devices after applying for the application badge setting permission and setting the application start class. For more information, please see [Badge Adaption Guide](https://intl.cloud.tencent.com/document/product/1024/35828).

## Troubleshooting

#### Querying Huawei Push registration error code

The Huawei Push service has strict requirements on the integration configuration. If you fail to register with the Huawei channel, you can use the following method to get the Huawei Push registration error code:


1. In debugging mode of the push service, filter logs by the keyword `OtherPush` or `HMSSDK` to view the return code logs.
2. You can find the cause of the error and get the solution in [Common error codes](https://developer.huawei.com/consumer/cn/doc/development/HMS-2-References/hmssdk_huaweipush_api_reference_errorcode).



#### Why are there no alerts for notifications delivered through the Huawei channel?


Starting from EMUI 10.0, Huawei Push intelligently categorizes notification messages into two levels: general and important. Versions below EMUI 10.0 don't categorize notifications but have only one level, so all notifications are displayed through the "default notification" channel, which is equivalent to the important level on EMUI 10.0. If a notification is categorized as "general", there will be no vibration, sound, or status bar icon alerts for it. Currently, the notification level can be set to "important" through the custom notification channel; however, according to the applicable Huawei Push rules, the final display effect will still be determined jointly by the set level and the level calculated by Huawei Push's intelligent categorization, and the lower level will prevail; for example, if the two levels are "important" and "general", "general" will prevail. For more information, please see [How to Use Huawei Notification Channel](https://intl.cloud.tencent.com/document/product/1024/36250).


#### Others

When your application is released on Huawei AppGallery, it may fail the audit with the error message "Error:28: you need to package the certificate file into the APK when integrating with HMS. Please directly copy the assets directory to the application project's root directory" displayed. Please fix the problem as follows:
1. Download the official Huawei HMS SDK.
2. Copy all files and subdirectories in the `assets` directory to that in your application project. If the `assets` directory does not exist in the application project, create one.

