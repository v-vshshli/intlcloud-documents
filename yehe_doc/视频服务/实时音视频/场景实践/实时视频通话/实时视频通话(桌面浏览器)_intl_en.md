This document describes how to implement a browser-based video call solution.
- Part 1 describes how to activate the service and run the demo.
- Part 2 describes how to build your own video call feature using the TRTCCalling component.

## Environment Requirements
Currently, the desktop version of Chrome offers better support for the features of the TRTC SDK for desktop browsers; therefore, Chrome is recommended for the demo.

TRTCCalling uses the following ports and domain name for data transfer, which should be added to the allowlist of the firewall. After configuration, please use [Official Demo](https://demo-1252463788.cos.ap-shanghai.myqcloud.com/trtccalling/demo/index.html) to check whether the configuration has taken effect.
  - TCP port: 8687
  - UDP ports: 8000, 8080, 8800, 843, 443, 16285
  - Domain name: qcloud.rtc.qq.com

The service supports the following platforms:

| OS |          Browser (Desktop)         | Minimum Version Requirement |
| :------: | :--------------------------: | :----------------: |
|  macOS  |     Safari     |        11+         |
|  macOS  |     Chrome     |        56+         |
|  macOS  |    Firefox     |        56+         |
|  macOS  |      Edge      |        80+         |
| Windows  |     Chrome     |        56+         |
| Windows | QQ Browser (WebKit core) |      10.4+       |
| Windows  |    Firefox     |        56+         |
| Windows  |      Edge      |        80+         |

## Running Demo


[](id:step1)
### Step 1. Create an application
1. [Sign up](https://intl.cloud.tencent.com/document/product/378/17985) for a Tencent Cloud account and complete identity verification.
2. Log in to the TRTC console and select **Development Assistance** > **[Demo Quick Run](https://console.cloud.tencent.com/trtc/quickstart)**.
3. Enter an application name, e.g., `TestTRTC`, and click **Create**.


[](id:step2)
### Step 2. Download the SDK and demo source code
1. Download the SDK and demo source code for your platform.
2. Click **Next**.

[](id:step3)
### Step 3. Configure demo project files
1. In the **Modify Configuration** step, select the platform in line with the source package downloaded.
2. Find and open the `Web/TRTCScenesDemo/trtc-calling-web/public/debug/GenerateTestUserSig.js` file.
3. Set parameters in the `GenerateTestUserSig.js` file as follows:
<ul style="margin:0"><li/>SDKAPPID: 0 by default. Set it to the actual `SDKAppID`.
<li/>SECRETKEY: left empty by default. Set it to the actual key.</ul>
4. Click **Next** to complete the creation.
5. After compilation, click **Return to Overview Page**.

>!
>- The method for generating `UserSig` described in this document involves configuring `SECRETKEY` in client code. In this method, `SECRETKEY` may be easily decompiled and reversed, and if your key is leaked, attackers can steal your Tencent Cloud traffic. Therefore, **this method is only suitable for the local execution and debugging of the demo**.
>- The correct `UserSig` distribution method is to integrate the calculation code of `UserSig` into your server and provide an application-oriented API. When `UserSig` is needed, your application can send a request to the business server for a dynamic `UserSig`. For more information, please see [How do I calculate UserSig on the server?](https://intl.cloud.tencent.com/document/product/647/35166).

[](id:step4)
### Step 4. Run the demo
1. In the npm command line, enter the following commands:
```
npm install
npm run serve
```
2. Open Chrome and go to `http://localhost:8080/`. If the above steps are performed correctly, the page shown in the following figure is displayed:
3. Enter your user ID and click **Log In**. Then, click **Video Call**.
4. Enter the user ID of the receiver and click **Call**.
5. Start the video call.


## Building Your Own Video Call
### Step 1. Import the TRTCCalling component

>?
>- Since version 0.6.0, you need to manually install dependencies [trtc-js-sdk](https://www.npmjs.com/package/trtc-js-sdk), [tim-js-sdk](https://www.npmjs.com/package/tim-js-sdk), and [tsignaling](https://www.npmjs.com/package/tsignaling).
>- To reduce the size of trtc-calling-js.js, and prevent version conflict between trtc-calling-js.js and the already in use trtc-js-sdk, tim-js-sdk or tsignaling, which may stop the latter three from being packaged into the former, you need to manually install the dependencies before use.

```
  npm i trtc-js-sdk --save
  npm i tim-js-sdk --save
  npm i tsignaling --save
  npm i trtc-calling-js --save



  //If you use trtc-calling-js via script, you need to manually import trtc.js first in the specified order.
  <script src="./trtc.js"></script>

  // First, manually import tim-js.js.
  <script src="./tim-js.js"></script>

  // Then, manually import tsignaling.js.
  <script src="./tsignaling.js"></script>



  // At last, manually import trtc-calling-js.js.
  <script src="./trtc-calling-js.js"></script>
```

### Step 2. Create a TRTCCalling object
Create a TRTCCalling object and set the `SDKAppID` parameter to your `SDKAppID`.
```
import TRTCCalling from 'trtc-calling-js';

let options = {
  SDKAppID: 0 // Replace 0 with your `SDKAppID` when connecting.
};
const trtcCalling = new TRTCCalling(options);
```

### Step 3. Log in
Call the login function to log in, with `userID` set to your user ID and `userSig` set to your signature. For how to calculate userSig, please see [FAQs > UserSig](https://intl.cloud.tencent.com/document/product/647/35166).

```javascript
trtcCalling.login({
  userID,
  userSig,
});
```

### Step 4. Make a one-to-one call
#### Caller: making a call
```javascript
trtcCalling.call({
  userID,  // User ID
  type: 2, // Call type, 0-unknown, 1-audio call, 2-video call
  timeout  // Timeout, in seconds
});
```

#### Receiver: answering a call
```javascript
// Accept
trtcCalling.accept({
  inviteID, // Invitation ID, which identifies an invitation
  roomID,   // Room ID
  callType  // 0-unknown, 1-audio call, 2-video call
});
// Reject
trtcCalling.reject({ 
  inviteID, // Invitation ID, which identifies an invitation
  isBusy // Whether the line is busy. 0-unknown, 1-audio call, 2-video call
  })
```

#### Turning on local camera
```javascript
trtcCalling.openCamera()
```

#### Showing remote video
```javascript
trtcCalling.startRemoteView({
  userID, // Remote user ID
  videoViewDomID // The user data will be rendered in the DOM ID node.
})
```

#### Showing local video preview
```javascript
trtcCalling.startLocalView({
  userID, // Local user ID
  videoViewDomID // The user data will be rendered in the DOM ID node.
})
```

#### Hanging up
```javascript
trtcCalling.hangup()
```
