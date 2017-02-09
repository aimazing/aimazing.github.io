# Aimazing SDK Documentation (Android)
---
## Overview

#### [Installation](#installation-1)

#### [Classes](#classes-1)
* [Emitter](#emitter)
* [Recognizer](#recognizer)
* [Utility](#utility)

#### [Callback Functions](#callback-functions-1)
* [EmitterCallback](#emittercallback)
* [RecognizerCallback](#recognizercallback)
* [UtilityCallback](#utilitycallback)

---
## Installation
---
1. Place the .aar file into the Android Project `app/libs/`.
2. Insert the fragment below into `build.gradle`.
```
repositories {
    flatDir {
        dirs 'libs'
    }
}
```
3. Insert 
```
compile(name:'aimazinglib-release', ext:'aar')
compile 'com.squareup.okhttp3:okhttp:3.4.1'
```
into `dependencies{}` of `build.gradle`.

---
# Classes
---

## Emitter
If AimazingSDK is installed as OTT sender(ie. payee app), Emitter class provide all the methods you need.

The steps we recommend you to follow:
0. Check the registration status. If already registered as sender, skip step1 below. If already registered as receiver, step1 below will override the position, from receiver to sender. (to Utility.isRegistered())
1. Register on AimazingSDK as sender.(to Emitter.register())
2. Start using AimazingSDK to transmit OTT.(to Emitter.start())

Step 0 and 1 require internet connectivity, we recommend you to complete these two steps immediately after your user login to your app.
Step 2 can be in totally offline environment, it means that sender can generate and send OTT offline.

---
### Construct
---
Construct Emitter instance to start using Aimazing SDK to transmit data.
```java
new Emitter(Activity _activity, EmitterCallback _callback)
```

#### Arguments
* Activity - Android activity class
* [EmitterCallback](#emittercallback) - EmitterCallback instance

#### Example
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
  
  @Override
  public void onRegisterFailed() {
    Log.e("EMITTERCALLBACK", "registerFailed");
  }
  
  @Override
  public void onRegisterSuccess() {
    Log.e("EMITTERCALLBACK", "registerSuccess");
  }
};

emitter = new Emitter(MainActivity.this, emitterCallback);
```

---
### Register
---
The registration result will be sent through callback function `onRegisterSuccess()` or `onRegisterFailed()`. Please refer "What is TokenC?" for the details of TokenC.
```java
void register(String tokenC)
```

#### Arguments
* tokenC - tokenC

#### Example
```java
emitter.register("123a4cc7be7683adb71530");
```

---
### Start Playing
---
AimazingSDK will generate OTT and send it through sound waves.
```java
void start()
```

#### Arguments
N/A

#### Example
```java
emitter.start();
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

If AimazingSDK is installed as OTT receiver(ie. merchant app), Recognizer class provide all the methods you need.

The steps we recommend you to follow:
0. Check the registration status. If already registered as receiver, skip step1 below. If already registered as sender, step1 below will override the position, from receiver to sender. (to Utility.isRegistered())
1. Register on AimazingSDK as receiver.(to Recognizer.register())
2. Start using AimazingSDK to receive OTT.(to Recognizer.start())

These steps above require internet connectivity.

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
    Log.e("RECOGNIZERCALLBACK", "error: " + message);
  }
  
  @Override
  public void onFailed(final String message) {
    Log.e("RECOGNIZERCALLBACK", "failed: " + message);
  }
 
  @Override
  public void onTimeout(final String mode) {
    Log.e("RECOGNIZERCALLBACK", "timeout");
  } 
 
  @Override
  public void onGotHeader() {
    Log.e("RECOGNIZERCALLBACK", "headerGot");
  } 
 
  @Override
  public void onGotOTT() {
    Log.e("RECOGNIZERCALLBACK", "ottGot");
  }
  
  @Override
  public void onRegisterFailed() {
    Log.e("RECOGNIZERCALLBACK", "registerFailed");
  }
  
  @Override
  public void onRegisterSuccess() {
    Log.e("RECOGNIZERCALLBACK", "registerSuccess");
  }
};
    
recognizer = new Recognizer(MainActivity.this, recognizerCallback);
```

---
### Register
---
The registration result will be sent through callback function `onRegisterSuccess()` or `onRegisterFailed()`. Please refer "What is TokenC?" for the details of TokenC.
```java
void register(String tokenC)
```

#### Arguments
* tokenC - tokenC

#### Example
```java
recognizer.register("123a4cc7be7683adb71530");
```

---
### Start Receiving
---
Start receiving ott. The sender's tokenC will be sent through callback functions. Please refer to the description of [RecognizerCallback](#recognizercallback) for further details.
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
### Is Device Rooted?
---
Check the device is rooted or not, due to safety reasons, we recommend you to limit the functionality to rooted devices.
```java
boolean isDeviceRooted();
```

#### Arguments
N/A

#### Example
```java
if (utility.isDeviceRooted())
  Log.e("isDeviceRooted", "device rooted, please be careful");
```

---
### Is Registered?
---
Check the register status, it will return an character
```java
char isRegistered()
```

#### Arguments
N/A

#### Return
- 'R': registered as receiver
- 'S': registered as sender
- null: not register yet

#### Example
```java
if (null == utility.isRegistered())
  emitter.register("11afdsh2271g2");
```

---
# Callback Functions
---
### EmitterCallback
---
#### onStarted()
While start playing sound waves, this callback function will be called.

#### onStopped()
While stop playing sound waves, this callback function will be called.

#### onError(String errorMessage)
While playing failed, this callback function will be called, the error message will be passed as `errorMessage`.

#### onRegisterSuccess()
While register success.

#### onRegisterFailed()
While register failed, please try again.

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
  
  @Override
  public void onRegisterSuccess() {
    MainActivity.this.runOnUiThread(new Runnable() {
      @Override
      public void run() {
        new AlertDialog.Builder(MainActivity.this)
                .setTitle("Emitter Success")
                .setMessage("Emitter Register Success")
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
  public void onRegisterFailed() {
    MainActivity.this.runOnUiThread(new Runnable() {
      @Override
      public void run() {
        new AlertDialog.Builder(MainActivity.this)
                .setTitle("Emitter Error")
                .setMessage("Emitter Register Failed")
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

#### onFailed(String message)
While OTT from sender invalid, this callback function will be called, which means transaction failed.

#### onGotHeader()
Debug use.

#### onGotOTT()
While recognizer got the OTT from sender, and waiting for Aimazing server response, this callback function will be called.

#### onTimeout()
Debug use.

#### onRegisterSuccess()
While register success.

#### onRegisterFailed()
While register failed, please try again.


### Example
```java
recognizerCallback = new RecognizerCallback() {
  @Override
  public void onStarted() {
    Log.e("RECOGNIZERCALLBACK", "started");
  }

@Override
      public void onStarted() {

      }

      @Override
      public void onStopped() {

      }

      @Override
      public void onSuccess(final String s) {
        MainActivity.this.runOnUiThread(new Runnable() {
          @Override
          public void run() {
            recognizedTextView.setText(s);
          }
        });
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

      @Override
      public void onFailed(final String s) {
        MainActivity.this.runOnUiThread(new Runnable() {
          @Override
          public void run() {
            recognizedTextView.setText("Failed: " + s);
          }
        });
      }

      @Override
      public void onTimeout(final String mode) {
        MainActivity.this.runOnUiThread(new Runnable() {
          @Override
          public void run() {
            recognizedTextView.setText("Timeout: " + mode);
          }
        });
      }

      @Override
      public void onGotHeader() {
        MainActivity.this.runOnUiThread(new Runnable() {
          @Override
          public void run() {
            recognizedTextView.setText("");
          }
        });
      }

      @Override
      public void onGotOTT() {
        MainActivity.this.runOnUiThread(new Runnable() {
          @Override
          public void run() {
            recognizedTextView.setText("GET OTT");
          }
        });
      }

      @Override
      public void onRegisterFailed() {
        MainActivity.this.runOnUiThread(new Runnable() {
          @Override
          public void run() {
            new AlertDialog.Builder(MainActivity.this)
                    .setTitle("Recognizer Error")
                    .setMessage("Recognizer Register Failed")
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
      public void onRegisterSuccess() {
        MainActivity.this.runOnUiThread(new Runnable() {
          @Override
          public void run() {
            new AlertDialog.Builder(MainActivity.this)
                    .setTitle("Recognizer Success")
                    .setMessage("Recognizer Register Success")
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
