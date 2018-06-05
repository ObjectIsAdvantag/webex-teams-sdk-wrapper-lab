[![LICENSE](https://img.shields.io/github/license/weddle/webex-teams-sdk-wrapper-lab.svg)](https://github.com/weddle/webex-teams-sdk-wrapper-lab)

# Webex Teams Android SDK Wrapper Lab

## Learn how to embed video calling with Webex Teams

In this lab, you will learn how to embed video calling into a simple Android App using the [Webex Teams Android SDK Wrapper](https://github.com/weddle/webex-teams-sdk-wrapper).

No coding is required - we will walk you through each step of the process from zero to making video calls.

## Requirements

In order to run this lab, you will need either a Mac, Windows or Linux PC with Android Studio installed.

If you wish to fully test the application once it has been built, you will need a compatible Android device (tested on Google Pixel and Samsung Galaxy S7 Edge, but others should work) and a guest token (JWT) to authenticate with.  Ask your lab instructor if you do not have a guest token.

# Lab Instructions
 
## Import project into Android Studio

Open a command prompt and use git to clone this repository to your local machine.

```
git clone https://github.com/weddle/webex-teams-sdk-wrapper-lab
```

First select **"File -> Open"** and navigate to open the folder that was just created.

Android Studio will import the repo and rebuild the necessary project files.

**You will see a number of errors - don't worry, that's expected at this point**


N.B. - **Do Not** select "File -> New -> Project from Version Control -> Github" as Android Studio will not properly rebuild the project if imported that way.



## Setup gradle to include the SDK Wrapper

The next thing we need to do is to include the SDK and Wrapper in our project.  The Android Studio build process is handled by Gradle, so we will be modifying gradle files in this step.

First we need to update the root build.gradle for the project.  Specifically, we'll need to make some changes to the allprojects section:

```
allprojects {
    repositories {
        google()
        jcenter()
//        maven { url 'https://devhub.cisco.com/artifactory/sparksdk/' }
//        maven { url 'https://jitpack.io' }
    }
}
```

Look at the two lines that have been commented out.  The first line tells gradle to include the Cisco repository where the Webex Teams SDK can be included.  The second line references something called JitPack.
 
We use JitPack to distribute the SDK Wrapper releases.  (If you are curious, you can learn more about Jitpack here).

**Uncomment both of the maven lines by removing the //**

Next we need to make a few changes to the application build.gradle file.


Add the dependencies to your application build.gradle, so open it in Android Studio.

First, you will see a section labeled android.  In it, you'll see a defaultconfig section that looks something like this:

```
    defaultConfig {
        applicationId "com.ryanweddle.webexteamssdkwrappersample"
        ...
        testInstrumentationRunner "android.support.test.runner.AndroidJUnitRunner"
        // multiDexEnabled true
    }
```

**Uncomment the line that says "multiDexEnabled true"**.

This is a requirement of the Webex Teams SDK.

Next you'll see a section for dependencies that looks something like this:

```
dependencies {
    implementation fileTree(include: ['*.jar'], dir: 'libs')
    ...
    // compile('com.ciscospark:androidsdk:1.3.0@aar', { transitive = true })
    // implementation 'com.github.weddle:webex-teams-sdk-wrapper:v0.2'

}
```

**Uncomment the last two lines to include both the SDK and the Wrapper in your project**

You will see a message that says something like the following:

```
Gradle files have changed since last project sync.
A project sync may be necessary for the IDE to work properly.
```

**Click "Sync Now" to sync your project**


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

You can see we have specified the exact class to be run in the second argument to the Intent constructor.

In our case, it's not enough to specify that we want to start a call, we also need to pass some additional information.  We need a way to specify who we are calling and to identify ourselves to the Webex Teams platform as an authorized caller.

While there are many ways we could do this, Android provides a simple way to pass additional information with the intent.  We will use the putExtra method to Parcel the guest token and Teams ID we want to call.

```
intent.putExtra(SparkCall.INTENT_CALLEE, mCallEdit.getText().toString());
intent.putExtra(SparkCall.INTENT_JWT, mTokenEdit.getText().toString());
```

The first argument to the putExtra method specifies the specific data we are adding, in this case either a TeamsID or a guest token (JWT).  In the second argument, we pass the actual data - in this case we can simply get the text contained in the EditText fields in our app.

Now that we have created our Intent, we are ready to start the Call Activity, we do this with the last statement:

```
startActivity(intent);
```

When this statement executes, it will fulfill the intent we have created by starting the SparkCall Activitiy with the extra data we have provided.

Before moving on, **uncomment this code by removing the surrounding comment brackets.** 


## Declare required permissions in Android Manifest

We need to do one last thing before we are ready to build our app.  Open up AndroidManifest.xml from your project and take a look.

You will notice a section that looks like this:
```
    <!---
    <uses-permission android:name="android.permission.CAMERA" />
    <uses-permission android:name="android.permission.RECORD_AUDIO" />
    <uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />
    -->
```

Android requires that we declare the permissions our application will need to use.  These are specified in the manifest file.  In this case, uncomment the CAMERA, RECORD_AUDIO, and MODIFY_AUDIO_SETTINGS permissions to declare them.


**Delete the lines <!- and --> in order to uncomment the permissions.**

## Build and run the application

Now that we've got all the code ready, it's time to build the application.

Under the Build menu, select "Make Project" in order to build the app.  After a few moments, you should see a message that the build completed successfully.

Under the Run menu, select "Run" to run your app.  Since this app will use the camera, you should use an actual Android device to test, rather than an Android Virtual Device.


## License
Webex Teams Android SDK Wrapper Lab is available under the MIT license. See the LICENSE file for more info.
