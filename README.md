# Aimazing SDK v2.0 Documentation

---
## Introduction

### What is Aimazing OTT?

Aimazing OTT(one-time token) is time based one-time token, every Aimazing OTT  are 28-digits numbers and will be alive in 30 seconds. 

### What is TokenC?

Aimazing is helping our clients to do user authentication. In short, Aimazing SDK can help the receiving side(ie: merchant portal) recognizes who is the sending side.

For every users of Aimazing SDK(sending and receiving side), are needed to provide a TokenC to Aimazing during Emitter/Recognizer registration. TokenC is the unique identity for every users in client's application.

### What is the authentication process look like?

The authentication process is like the scenario below.
AimPay is an mobile wallet product which is using AimazingSDK, AimPay has launched two apps, MerchantApp and CustomerApp. The position of CustomerApp in AimazingSDK is OTT sender, MerchantApp is OTT receiver.
When a customer(customer1), install and login into AimPay CustomerApp, AimPay will generate TokenC1 for customer1 to register AimazingSDK as a sender.
So, while customer1 using AimPay to make payment, AimazingSDK Emitter will generate OTT and send it to the merchant side, AimazingSDK Recognizer in AimPay MerchantApp will process the OTT and return TokenC1 to AimPay MerchantApp.
Finally, AimPay MerchantApp will know customer1 is the payee by using TokenC1.

---

## Features

### Emitter

* Registration.
* Generating OTT.
* Transmitting OTT through sound waves.
* Headset plugged detection.
* Auto volume control.

### Recognizer

* Registration.
* Receive & processing sound waves data.
* Headset plugged detection.

### Utility

* Calibration utility ensures the device's mic/speaker is supported.
* Check required permissions are granted.
* Request required permissions.
* Check registration status.
* Check device is rooted.

## Requirements

### Hardware

* Microphone can produces 16kHz \~ 20kHz
* Speaker can receives 16kHz \~ 20kHz

### Software

* Android Jelly Bean (4.1.x) or higher
* iOS 9.0 or higher




## How to Use?

* [Android](https://github.com/aimazing/aimazing.github.io/blob/v2.0/Android.md)
* [iOS](https://github.com/aimazing/aimazing.github.io/blob/v2.0/iOS.md)
