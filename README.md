# iOS Performance SDK

Repository for Android and iOS performance SDK's to generate performance reports at [Appachhi](https://appachhi.com/)

## Introduction

Appachhi iOS SDK provides the capability to capture device specific usage metrics like Core based CPU usage (percentage), Memory usage (MB), Thread count and the Network usage(KB). 

These metrics will then be passed to the Appachhi platform to analyse and produce the graphs on reports.

The SDK also provides the users the ability to generate ScreenTransition details along Launch Performance details.

Capture method trace details using the SDK’s run script injection


## Include Framework to your Podfile

Framework already exists in your project? Run

```python
$~ pod update
```

Else, Add following to your Podfile

```python
pod 'Appachhi', :podspec => 'https://raw.githubusercontent.com/irfan-cloudnix/Appachhi/master/Appachhi.podspec'
```

To continue, run the following command in your terminal
```python
$~ pod install
```

## Start using!
Import the library to AppDelegate.m or your PrefixHeader file
```python
#import <Appachhi/Appachhi.h>
```

### **Extract Device & Launch Performance Details**

1. **To generate Launch Performance report, use below code**
```
[appachhi logEvent:@"Event"];
```
> Ex:
```
[appachhi logEvent:@"Init invoked"];
[appachhi logEvent:@"View Loaded"];
[appachhi logEvent:@"Completed"];
```
logEvent takes NSString argument informing about the event currently executing or completed.

2. **To generate Device Performance report, use below code in didFinishLaunchingWithOptions of AppDelegate.m**
```
[appachhi capturePerformance];
```

### ScreenTransition Details

1. **Native objective-c App**
    1. Import Appachhi library wherever you want to make the screenTransition calls

       `#import <Appachhi/Appachhi.h>`
   1. Call screenTransitionStart by passing a relatable name of the transition wherever the screen rendering preparations are done.

       `[appachhi screenTransitionStart:@"Dashboard"];`
    1. Similarly call screenTransitionEnd with the same screen name given above and place it wherever the screen rendering completion is expected.

       `[appachhi screenTransitionEnd:@"Dashboard"];`

2. **React-Native App**
    1. Import react-native module

        ```import {NativeModules} from 'react-native'```
    1. Call start or end transition calls based on the screen rendering, pass screen name to the function call as an argument.

        ```NativeModules.AppachhiReact.reactScreenTransitionStart("Dashboard");```
```NativeModules.AppachhiReact.reactScreenTransitionEnd("Dashboard");```

2. **Native objective-c app with a WebView**
    1. Attach appachhi listener to WKUserContentController instance

        ```[controller addScriptMessageHandler:self name:@"appachhi"];```
    1. Inherit WKScriptMessageHandler in your ViewController class

        ```@interface UserViewController : UIViewController```

        ```<WKScriptMessageHandler>```

    1. Override didReceiveScriptMessage call with below implementation

        ```- (void)userContentController:(WKUserContentController *)userContentController didReceiveScriptMessage:(WKScriptMessage *)message {```

         ```[appachhi webViewScreenTransition:message.body];```

        ```}```

    1. Finally, in your WebView code, make following calls to indicate screenTransitionStart and End events.

        ```window.webkit.messageHandlers.appachhi.postMessage("START-dashboard");```
        ```window.webkit.messageHandlers.appachhi.postMessage("END-dashboard");```

- - - -
Done! Build and upload the App to Appachhi Platform
- - - -
