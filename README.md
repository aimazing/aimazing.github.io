# SDK Documentation

## Overview
* [Features](#features)
* [Requirements](#requirements)
* [Installation](#installation)

## Classes
* [Emitter](#emitter)
* [Recognizer](#recognizer)
* [Utility](#utility)

## Callback Functions
* [EmitterCallback](#emittercallback)
* [RecognizerCallback](#recognizercallback)
* [UtilityCallback](#utilitycallback)

---

### Features

[Emitter](#emitter)
* Transmitting data through sound waves.
* Headset plugged detection.
* Auto volume control.

[Recognizer](#recognizer)
* Receive & processing sound waves data.
* Headset plugged detection.

[Utility](#utility)
* Calibration utility ensures the device's mic/speaker is supported.
* Check required permissions are granted.
* Request required permissions.


### Requirements
Hardware
* Microphone can produces 16kHz \~ 20kHz
* Speaker can receives 16kHz \~ 20kHz

Software
* Android Jelly Bean (4.1.x) or higher 


# Classes

## Emitter
---
### Construct
---
Construct Emitter instance to start using Aimazing SDK to transmit data.
```java
new Emitter(Activity _activity, EmitterCallback _callback)
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
---
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
---
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
---
### Construct
---
Construct Recognizer instance to start using Aimazing SDK to receive data.
```java
new Recognizer(Activity _activity, Recognizer _callback)
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
---
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
---
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
### Construct
---
Construct Utility instance to use the utilities Aimazing SDK provide.
```java
new Utility(Activity _activity, UtilityCallback _callback);
```

#### Arguments
* Activity - Android activity class
* [UtilityCallback](#utilitycallback) - UtilityCallback instance

#### Example
```java
Utility utility;
UtilityCallback utilityCallback;

utilityCallback = new UtilityCallback() {
  @Override
  public void onCalibrationSuccess() {
    MainActivity.this.runOnUiThread(new Runnable() {
      @Override
      public void run() {
        new AlertDialog.Builder(MainActivity.this)
                .setTitle("Aimazing SDK Calibration Test")
                .setMessage("Success")
                .setNeutralButton("OK", new DialogInterface.OnClickListener() {
                  public void onClick(DialogInterface dialog, int which) {
                    // do nothing
                  }
                })
                .show();
      }
    });
  }

  @Override
  public void onCalibrationFailed() {
    MainActivity.this.runOnUiThread(new Runnable() {
      @Override
      public void run() {
        new AlertDialog.Builder(MainActivity.this)
                .setTitle("Aimazing SDK Calibration Test")
                .setMessage("Failed")
                .setNeutralButton("OK", new DialogInterface.OnClickListener() {
                  public void onClick(DialogInterface dialog, int which) {
                    // do nothing
                  }
                })
                .show();
      }
    });
  };

utility = new Utility(MainActivity.this, utilityCallback);
```
---
### Start Calibration Process for Current Device
---
In order to start using Aimazing SDK, the microphone and speaker of the device need to pass this calibration process to ensure that the device is fully supports the functions Aimazing SDK provide. The result of calibration process will be sent through callback functions. Please refer to the description of [UtilityCallback](https://app.nuclino.com/teams/13:23281/documents/850d0a7e-3a41-4164-a92f-23f1e34b69dc) for further details.
```java
void calibration();
```

#### Arguments
N/A

#### Example
```java
calibrationButton = (Button)findViewById(R.id.calibrationButton);
calibrationButton.setOnClickListener(new View.OnClickListener() {
  @Override
  public void onClick(View v) {
    utility.calibration();
  }
});
```
---
### Check Permissions Granted
---
Aimazing SDK requires the control of playback volume and microphone access. Please use this method to check whether the permissions mention before are granted.
```java
boolean checkPermission();
```

#### Arguments
N/A

#### Example
```java
if (!utility.checkPermission()) {
  utility.requestPermission();
}
```
---
### Request Permissions
---
If the permissions not granted, please use this method to request the permissions from users. Override `onRequestPermissionsResult` in Android Activity can catch the requesting result.
```java
void requestPermission();
```

#### Arguments
N/A

#### Example
```java
if (!utility.checkPermission()) {
  utility.requestPermission();
}

@Override
public void onRequestPermissionsResult(int requestCode, String permissions[], int[] grantResults) {
  switch (requestCode) {
    case 1:
      if (grantResults[0] == PackageManager.PERMISSION_GRANTED) {
        // permission granted
      } else {
        // permission denied!
        new AlertDialog.Builder(this)
                .setTitle("Unable to start Aimazing SDK")
                .setMessage("Permission Not Granted")
                .setNeutralButton("RETRY", new DialogInterface.OnClickListener() {
                  public void onClick(DialogInterface dialog, int which) {
                    // do nothing
                    utility.requestPermission();
                  }
                })
                .show();
      }
  }
}
```

---
## Callback
---
### EmitterCallback
---
#### onStarted()
While start playing sound waves, this callback function will be called.

#### onStopped()
While stop playing sound waves, this callback function will be called.

#### onError(String errorMessage)
While playing failed, this callback function will be called, the error message will be passed as `errorMessage`.

### Example
```java
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
    MainActivity.this.runOnUiThread(new Runnable() {
      @Override
      public void run() {
        new AlertDialog.Builder(MainActivity.this)
                .setTitle("Emitter Error")
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
```

---
### RecognizerCallback
---
#### onStarted()
While start receiving sound waves, this callback function will be called.

#### onStopped()
While stop receiving sound waves, this callback function will be called.

#### onError(String errorMessage)
While recognizer failed to start, this callback function will be called, the error message will be passed as `errorMessage`.

#### onSuccess(String message)
While recognizer got the message from emitter, this callback function will be called, the message will be passed as `message` .

### Example
```java
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
```

---
### UtilityCallback
---
#### onCalibrationSuccess()
While the device pass the calibration test, this callback function will be called.

#### onCalibrationFailed()
While the device fail the calibration test, this callback function will be called.

### Example
```java
utility = new Utility(this, new UtilityCallback() {
  @Override
  public void onCalibrationSuccess() {
    MainActivity.this.runOnUiThread(new Runnable() {
      @Override
      public void run() {
        new AlertDialog.Builder(MainActivity.this)
                .setTitle("Aimazing SDK Calibration Test")
                .setMessage("Success")
                .setNeutralButton("OK", new DialogInterface.OnClickListener() {
                  public void onClick(DialogInterface dialog, int which) {
                    // do nothing
                  }
                })
                .show();
      }
    });
  }

  @Override
  public void onCalibrationFailed() {
    MainActivity.this.runOnUiThread(new Runnable() {
      @Override
      public void run() {
        new AlertDialog.Builder(MainActivity.this)
                .setTitle("Aimazing SDK Calibration Test")
                .setMessage("Failed")
                .setNeutralButton("OK", new DialogInterface.OnClickListener() {
                  public void onClick(DialogInterface dialog, int which) {
                    // do nothing
                  }
                })
                .show();
      }
    });
  }
});
```
