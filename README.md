# Construct

Construct Emitter instance to start using Aimazing SDK to transmit data.

    Emitter(Activity _activity, EmitterCallback _callback)

## Arguments

* Activity - Android activity class
* [EmitterCallback](https://app.nuclino.com/teams/13:23281/documents/27ecc2aa-bb51-4e00-b30f-b28989918199) - EmitterCallback instance

## Example

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

---

# Start Playing

Start transmitting data through sound waves. The sound waves will keep playing until stop() is called.

    void start(String _message)

## Arguments

* Message - message to transmit

## Example

    emitter.start("HelloWorld123");

---

# Stop Playing

Stop playing sound waves.

    void stop()

## Arguments

N/A

## Example

    emitter.stop();