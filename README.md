[![LICENSE](https://img.shields.io/github/license/weddle/webex-teams-sdk-wrapper-sample.svg)](https://github.com/weddle/webex-teams-sdk-wrapper-sample/blob/master/LICENSE)

# Webex Teams Android SDK Wrapper Lab

## Learn how to embed video calling with Webex Teams

In this lab, you will learn how to embed video calling into a simple Android App using the [Webex Teams Android SDK Wrapper](https://github.com/weddle/webex-teams-sdk-wrapper).

No coding is required - we will walk you through each step of the process from zero to making video calls.

## Requirements

In order to run this lab, you will need either a Mac, Windows or Linux PC with Android Studio installed.

If you wish to fully test the application once it has been built, you will need a compatible Android device (tested on Google Pixel and Samsung Galaxy S7 Edge, but others should work) and a guest token (JWT) to authenticate with.  Ask your lab instructor if you do not have a guest token.

# Lab Instructions
 
## Import project into Android Studio

## Modify gradle to include the SDK Wrapper

## Add calling logic to MainActivity

The Webex Teams SDK Wrapper uses an Activity as a drop in to make and display the Video call.  The Call Activity is started by passing an intent with a Guest Token (JWT) and the TeamsID to call.

Open MainActivity.java under your project in Android Studio.

You will see a section of code that looks something like this:

```
mCallButton.setOnClickListener(view -> {
    Log.i(CLASS_TAG, "Call Button Pressed");
    /*
    ...
    */

});
```

This code block sets a Listener that is invoked whenever the Button referred to by mCallButton is clicked.  You can see that it is currently setup to print a Log that simply says "Call Button Pressed"

You'll see that there is some additional code after the log statement that has been commented out.  This is the code that invokes the SDK Wrapper to make the call.

Let's take a closer look:

```
Intent intent = new Intent(MainActivity.this, SparkCall.class);
intent.putExtra(SparkCall.INTENT_CALLEE, mCallEdit.getText().toString());
intent.putExtra(SparkCall.INTENT_JWT, mTokenEdit.getText().toString());

startActivity(intent);
```

Android Applications are made up of Activities, which Google defines as "a single, focused thing a user can do".  In this case, we are interested in leaving our Application's Main Activity, and starting a SparkCall Activity (this is a holdover from the old name for Webex Teams).


Android Activities are started by Intents, or "abstract descriptions of operations to be performed."  Intents can be very general (implicit intents), for example sending an email, or they can be very specific (explicit intents) specifying the exact java Class to be run.

For our purposes, we will use an explicit intent to start the video call.  We create  the intent with the following statement:

```
Intent intent = new Intent(MainActivity.this, SparkCall.class);
```


## Declare required permissions in Android Manifest



## License
Webex Teams Android SDK Wrapper Sample is available under the MIT license. See the LICENSE file for more info.
