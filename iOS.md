# Aimazing SDK Documentation (iOS)
---
## Overview

#### [Installation](#installation-1)

#### [Classes](#classes-1)
* [Emitter](#emitter)
* [Recognizer](#recognizer)
* [Utility](#utility)

#### [Struct](#struct-1)
* [EmitterError](#emittererror)
* [RecognizerError](#recognizererror)
* [UtilityError](#utilityerror)

---
## Installation
---
#### Import library
```swift
import AimazingLib
```
---
#### Add description into info.plist
Allowed server connection should be stated clearly in info.plist

```
 <key>NSAppTransportSecurity</key>
    <dict>
        <key>NSExceptionDomains</key>
        <dict>
            <key>aimpay.sg</key>
            <dict>
                <!--Include to allow subdomains-->
                <key>NSIncludesSubdomains</key>
                <true/>
                <!--Include to specify minimum TLS version-->
                <key>NSExceptionMinimumTLSVersion</key>
                <string>TLSv1.0</string>
            </dict>
        </dict>
    </dict>
```
---
# Classes
---
## Emitter
If AimazingSDK is installed as OTT sender(ie. payee app), Emitter class provide all the methods you need.

The steps we recommend you to follow:

1. Check the registration status. If already registered as sender, skip step2 below. If already registered as receiver, step2 below will override the position, from receiver to sender. (to Utility.isRegistered()) 
2. Register on AimazingSDK as sender.(to Emitter.register()) 
3. Start using AimazingSDK to transmit OTT.(to Emitter.start())


#### Step 1 and 2 require internet connectivity, we recommend you to complete these two steps immediately after your user login to your app. Step 3 can be in totally offline environment, it means that sender can generate and send OTT offline.
---
### Construct
---
Construct Emitter instance to start using Aimazing SDK to transmit data.
```swift
Emitter(_view: UIView)
```

#### Arguments
* **_view** - The view that the UIViewController manages

#### Example
```swift
var emitter: Emitter

emitter = Emitter(_view: view)
```
---
### Register
---
Register as vaild emitter 
```swift
func register(token: String, completion: (isSuccess: Bool) -> Void)
```

#### Arguments
* **token** - tokenC, Please refer "What is TokenC?" for the details of TokenC
* **completion** - Callback function when complete register procedure with registration status


#### Example
```swift
emitter.register(token: "123a4cc7be7683adb71530", completion: { isSuccess in
        // THING TO DO
})
```
---
### Start Playing
---
Start transmitting data through sound waves. The sound waves will keep playing until stop() is manually triggered.
```swift
func start(completion:((error: Int) -> Void))
```

#### Arguments
* **completion** - Callback function when successfully start transmitting data, return error if failed to start transmitting data, error value can be referred to [EmitterError](#emittererror)

#### Example
```swift
emitter.start( completion: { (error) in
        // THINGS TO DO
})
```

---
### Stop Playing
---
Stop playing sound waves.
```swift
func stop(completion:(() -> Void))
```

#### Arguments
* **completion** - Callback function when stop transmitting data

#### Example
```swift
emitter.stop( completion: {
      // THINGS TO DO 
})
```
---

## Recognizer
If AimazingSDK is installed as OTT receiver(ie. merchant app), Recognizer class provide all the methods you need.

The steps we recommend you to follow: 

1. Check the registration status. If already registered as receiver, skip step2 below. If already registered as sender, step2 below will override the position, from receiver to sender. (to Utility.isRegistered()) 
2. Register on AimazingSDK as receiver.(to Recognizer.register()) 
3. Start using AimazingSDK to receive OTT.(to Recognizer.start())


#### These steps above require internet connectivity.
---
### Construct
---
Construct Recognizer instance to start using Aimazing SDK to receive data.
```swift
Recognizer(_view: UIView)
```

#### Arguments
* **_view** - The view that the UIViewController manages

#### Example
```swift
var recognizer: Recognizer
    
recognizer = Recognizer(_view: view)
```
---
### Register
---
Register as vaild reconizer 
```swift
func register(token: String, completion: (isSuccess: Bool) -> Void)
```

#### Arguments
* **token** - tokenC, please refer "What is TokenC?" for the details of TokenC
* **completion** - Callback function when complete register procedure with registration status


#### Example
```swift
recognizer.register(token: "123a4cc7be7683adb71530", completion: { isSuccess in
        // THING TO DO
})
```

---
### Start Receiving
---
Start receiving data. The result will be sent through callback functions. 
```swift
func start(onStarted:(() -> Void), onGotHeader:(() -> Void), onGotOTT:(() -> Void), onSuccess:((content: String) -> Void), onFailed:((content, error)  -> Void))
```

#### Arguments
* **onStarted** - Callback function when start receiving data
* **onGotOTT** - Callback function when received OTT
* **onSuccess** - Callback function when successfully retrieving  tokenC from server
* **onFailed** - Callback function when failed to receive data, error value can be referred to [RecognizerError](#recognizererror) 

#### Example
```swift
recognizer.start( onStarted: {
            // THINGS TO DO WHEN START RECEIVING SOUND WAVE DATA
        }, onGotOTT: {
            // THINGS TO DO WHEN RECEIVED OTT FROM EMITTING SIDE
        }, onSuccess: {(content) in
            // THINGS TO DO WHEN SUCCESSFULLY RETRIEVING TOKENC
        }, onFailed: {(content, error) in
            // THINGS TO DO WHEN FAILED TO RECEIVE DATA
        })
```
---
### Stop Receiving
---
Stop the receiving process.
```swift
func stop(completion:(() -> Void))
```

#### Arguments
* **completion** - Callback function when stop receiving data

#### Example
```swift
recognizer.stop( completion: {
      // THINGS TO DO 
})
```
---
## Utility
---
### Construct
---
Construct Utility instance.
```swift
Utility(_view: UIView)
```

#### Arguments
* **_view** - The view that the UIViewController manages

#### Example
```swift
var utility: Utility

utility = Utility(_view: view)
```
---
### Start Calibration Process for Current Device
---
In order to start using Aimazing SDK, the microphone and speaker of the device need to pass this calibration process to ensure that the device is fully supports the functions Aimazing SDK provide. The result of calibration process will be sent through callback functions.

```swift
func calibration(onStarted: () -> Void, onStopped: （）-> Void, onFailed: (error: Int) -> Void)
```

#### Arguments
* **onStarted** - Callback function when calibration started
* **onStopped** - Callback function when calibration succeeded
* **onFailed** - Callback function when device failed to use sound wave to trasmit/receive data, error value can be referred to [UtilityError](#utilityerror)


#### Example
```swift
utility.calibration( onStarted: {
      // THINGS TO DO 
}, onStopped: {
      // THINGS TO DO 
}, onFailed: { error in
      // THINGS TO DO 
})
```
---
### Is Device JailBreak?
---
Check the device is JailBroken or not, due to safety reasons, we recommend you to limit the functionality to JailBroken devices.
```swift
func isJailBroken() -> Bool
```

#### Example
```swift
if (utility.isJailBroken()) {
        // THINGS TO DO 
}
```
---
### Is Registered?
---
Check the register status, it will return an character
```swift
func isRegistered() -> Character
```

### Return
* **"R"**: Registered as receiver
* **"S"**: Registered as sender
* **"\0"**: Not register yet


#### Example
```swift
if (utility.isRegistered() == "\0") {
        emitter.register(...)
}
```

---
# Struct
---
## EmitterError
---
* **HEADSET_PLUGGED**: Error when headset is detected
* **NOT_REGISTERED**: Error when Emitter hasn't registered yet

## RecognizerError
---
* **HEADSET_PLUGGED**: Error when headset is detected
* **TOKEN_RETRIEVE_ERROR**: Error when the TOKEN received hasn't found on server
* **NOT_REGISTERED**: Error when Recognizer hasn't registered yet

## UtilityError
---
* **HEADSET_PLUGGED**: Error when headset is detected
* **CALIBRATION_FAILED**: Error when exceed the testing time, we can conclude that the device can't fully support SDK function
