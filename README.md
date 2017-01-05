# SDK Documentation
---
## Overview
- [Features](#features)
- [Requirements](#requirements)
- [Installation](#installation)

## Classes
- [Emitter](#emitter)
- [Recognizer](#recognizer)
- [Utility](#utility)

## Callback Functions
- [EmitterCallback](#emittercallback)
- [RecognizerCallback](#recognizercallback)
- [UtilityCallback](#utilitycallback)

---

### Features

[Emitter](#emitter-1)
- Transmitting data through sound waves.
- Headset plugged detection.
- Auto volume control.

[Recognizer](#recognizer-1)
- Receive & processing sound waves data.
- Headset plugged detection.

[Utility](#utility-1)
- Calibration utility ensures the device's mic/speaker is supported.
- Check required permissions are granted.
- Request required permissions.


### Requirements
Hardware
* Microphone can produces 16kHz \~ 20kHz
* Speaker can receives 16kHz \~ 20kHz

Software
* Android Jelly Bean (4.1.x) or higher 


### Installation


---
# Classes
---

## Emitter

### Construct
Construct Emitter instance to start using Aimazing SDK to transmit data.
```java
Emitter(Activity _activity, EmitterCallback _callback)
```
####Arguments
* Activity - Android activity class
* [EmitterCallback](#emittercallback) - EmitterCallback instance

####Example
```java
Emitter emitter;
EmitterCallback emitterCallback;
    
emitterCallback = new EmitterCallback() {
  @Override
  public void onStarted() {
    Log.e("EMITTERCALLBACK", "started");
  }
    
  @Override
  public void onStopped() {
    Log.e("EMITTERCALLBACK", "stopped");
  }

  @Override
  public void onError(final String message) {
    Log.e("EMITTERCALLBACK", "error: " + message);
  }
};

emitter = new Emitter(MainActivity.this, emitterCallback);
```
---
### Start Playing
Start transmitting data through sound waves. The sound waves will keep playing until stop() is called.
```java
void start(String _message)
```

#### Arguments
* Message - message to transmit

####Example
```java
emitter.start("HelloWorld123");
```

---
### Stop Playing
Stop playing sound waves.
```java
void stop()
```

#### Arguments
N/A

#### Example
```java
emitter.stop();
```

---

## Recognizer

### Construct
Construct Recognizer instance to start using Aimazing SDK to receive data.
```java
Recognizer(Activity _activity, Recognizer _callback)
```

#### Arguments
* Activity - Android activity class
* [RecognizerCallback](#recognizercallback) - RecognizerCallback instance

#### Example
```java
Recognizer recognizer;
RecognizerCallback recognizerCallback;

recognizerCallback = new RecognizerCallback() {
  @Override
  public void onStarted() {
    Log.e("RECOGNIZERCALLBACK", "started");
  }

  @Override
  public void onStopped() {
    Log.e("RECOGNIZERCALLBACK", "stopped");
  }

  @Override
  public void onSuccess(final String message) {
    Log.e("RECOGNIZERCALLBACK", "success: " + message);
  }

  @Override
  public void onError(final String message) {
    MainActivity.this.runOnUiThread(new Runnable() {
      @Override
      public void run() {
        new AlertDialog.Builder(MainActivity.this)
                .setTitle("Recognize Error")
                .setMessage(message)
                .setNeutralButton("OK", new DialogInterface.OnClickListener() {
                  public void onClick(DialogInterface dialog, int which) {
                    // do nothing
                  }
                })
                .show();
      }
    });
  }
};
    
recognizer = new Recognizer(MainActivity.this, recognizerCallback);
```
---
### Start Receiving
Start receiving data. The result will be sent through callback functions. Please refer to the description of [RecognizerCallback](#recognizercallback) for further details. 
```java
void start()
```

#### Arguments
N/A

#### Example
```java
recognizer.start();
```
---
### Stop Receiving
Stop the receiving process.
```java
void stop()
```

#### Arguments
N/A

#### Example
```java
recognizer.stop();
```

---

## Utility



---
## Callback
---

### EmitterCallback


### RecognizerCallback


### UtilityCallback
