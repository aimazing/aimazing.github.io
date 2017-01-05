# SDK Documentation

## Overview
* [Features](#features)
* [Requirements](#requirements)
* [Usage](#usage)

## Classes
* [Emitter](#emitter)
* [Recognizer](#recognizer)

## Struct
* [EmitterError](#emittererror)
* [RecognizerError](#recognizererror)

---

### Features

#### [Emitter](#emitter)
* Transmitting data through sound waves.
* Headset plugged detection.
* Auto volume control.

#### [Recognizer](#recognizer)
* Receive & processing sound waves data.
* Headset plugged detection.



### [Requirements](#requirements)
Hardware
* Microphone can produces 16kHz \~ 20kHz
* Speaker can receives 16kHz \~ 20kHz

Software
* iOS 9.0 or higher 

### [Usage](#usage)

```swift
import AimazingLib
```


# Classes

## Emitter
---
### Construct
---
Construct Emitter instance to start using Aimazing SDK to transmit data.
```swift
Emitter(_view: UIView)
```

#### Arguments
* UIView - The view that the UIViewController manages

#### Example
```swift
var emitter: Emitter

emitter = Emitter(_view: view)
```
---
### Start Playing
---
Start transmitting data through sound waves. The sound waves will keep playing until stopEmit() is called.
```swift
func startEmit(message: message, completion:((error: Int) -> Void))
```

#### Arguments
* message - message to transmit, which is Base64 encoded
* completion - Callback function when successfully start transmitting data, return error if failed to start transmitting data, error value can be referred to [EmitterError](#emittererror)

#### Example
```swift
emitter.startEmit("HelloWorld123", completion: { (error) in
        // THINGS TO DO
})
```

---
### Stop Playing
---
Stop playing sound waves.
```swift
void stopEmit(completion:(() -> Void))
```

#### Arguments
* completion - Callback function when stop transmitting data

#### Example
```swift
emitter.stopEmit(completion: {
      // THINGS TO DO 
})
```
---

## Recognizer
---
### Construct
---
Construct Recognizer instance to start using Aimazing SDK to receive data.
```swift
Recognizer(_view: UIView)
```

#### Arguments
* UIView - The view that the UIViewController manages

#### Example
```swift
var recognizer: Recognizer
    
recognizer = Recognizer(_view: view)
```
---
### Start Receiving
---
Start receiving data. The result will be sent through callback functions. 
```swift
void startRecognition(onStarted:(() -> Void), onGotHeader:(() -> Void), onSuccess:((content: String) -> Void), onFailed:((content, error)  -> Void))
```

#### Arguments
* onStarted - Callback function when start receiving data
* onGotHeader - Callback function when received transmittion header
* onSuccess - Callback function when successfully receiving completed message
* onFailed - Callback function when failed to receive data, error value can be referred to [RecognizerError](#recognizererror) 

#### Example
```swift
recognizer.startRecognition(onStarted: {
            // THINGS TO DO WHEN START RECEIVING SOUND WAVE DATA
        },onGotHeader: {
            // THINGS TO DO WHEN RECEIVED TRANSMITTION HEADER
        },onSuccess: {(content) in
            // THINGS TO DO WHEN SUCCESSFULLY RECEIVING COMPLETED MESSAGE
        },onFailed: {(content, error) in
            // THINGS TO DO WHEN FAILED TO START RECEIVING DATA
        })
```
---
### Stop Receiving
---
Stop the receiving process.
```swift
void stopRecognition(completion:(() -> Void))
```

#### Arguments
* completion - Callback function when stop receiving data

#### Example
```swift
recognizer.stopRecognition( completion: {
      // THINGS TO DO 
})
```
---

# Struct

## [EmitterError](#emittererror)
---
* HEADSET_PLUGGED: Error when headset is detected

## [RecognizerError](#recognizererror)
---
* HEADSET_PLUGGED: Error when headset is detected
* HEADER_TIMEOUT: Error when time receving second header exceed
* CHECKSUM_ERROR: Error when the received message can't match with checksum

