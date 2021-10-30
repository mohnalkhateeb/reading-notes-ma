# Android Fundamentals
## Application Fundamentals 
- Android apps can be written using Kotlin, Java, and C++ languages. The Android SDK tools compile your code along with any data and resource files into an APK or an Android App Bundle.

- An Android package, which is an archive file with an .apk suffix, contains the contents of an Android app that are required at runtime and it is the file that Android-powered devices use to install the app.

- An Android App Bundle (AAB) is a publishing format and is not installable on Android devices, it defers APK generation and signing to a later stage. When distributing your app through Google Play for example, Google Play's servers generate optimized APKs that contain only the resources and code that are required by a particular device that is requesting installation of the app.

- Each Android app lives in its own security sandbox, protected by the following Android security features:

    - The Android operating system is a multi-user Linux system in which each app is a different user.

    - Each process has its own virtual machine (VM), so an app's code runs in isolation from other apps.

- The Android system implements the principle of least privilege. That is, each app, by default, has access only to the components that it requires to do its work and no more.

## App components
are the essential building blocks of an Android app. Each component is an entry point through which the system or a user can enter your app.

- There are four different types of app components:

    - Activities
    - Services
    - Broadcast receivers
    - Content providers

#### Activities
 An activity is the entry point for interacting with the user. It represents a single screen with a user interface.
- An activity facilitates the following key interactions between system and app:
    - Keeping track of what the user currently cares about (what is on screen) to ensure that the system keeps running the process that is hosting the activity.
    - Knowing that previously used processes contain things the user may return to (stopped activities), and thus more highly prioritize keeping those processes around.
    - Helping the app handle having its process killed so the user can return to activities with their previous state restored.
    - Providing a way for apps to implement user flows between each other, and for the system to coordinate these flows. 
- You implement an `activity` as a subclass of the `Activity` class. 

#### Services
A service is a general-purpose entry point for keeping an app running in the background for all kinds of reasons. It is a component that runs in the background to perform long-running operations or to perform work for remote processes.

- Bound services run because some other app (or the system) has said that it wants to make use of the service. This is basically the service providing an API to another process. The system thus knows there is a dependency between these processes, so if process A is bound to a service in process B, it knows that it needs to keep process B (and its service) running for A. Further, if process A is something the user cares about, then it also knows to treat process B as something the user also cares about. Because of their flexibility (for better or worse), services have turned out to be a really useful building block for all kinds of higher-level system concepts. Live wallpapers, notification listeners, screen savers, input methods, accessibility services, and many other core system features are all built as services that applications implement and the system binds to when they should be running.

- A `service` is implemented as a subclass of `Service`.

#### A broadcast receiver
 is a component that enables the system to deliver events to the app outside of a regular user flow, allowing the app to respond to system-wide broadcast announcements. Because broadcast receivers are another well-defined entry into the app, the system can deliver broadcasts even to apps that aren't currently running. 
 - A `broadcast receiver` is implemented as a subclass of `BroadcastReceiver` and each broadcast is delivered as an Intent object.

 #### Content providers
  A content provider manages a shared set of app data that you can store in the file system, in a SQLite database, on the web, or on any other persistent storage location that your app can access. Through the content provider, other apps can query or modify the data if the content provider allows it.

  - To the system, a content provider is an entry point into an app for publishing named data items, identified by a URI scheme. Thus an app can decide how it wants to map the data it contains to a URI namespace, handing out those URIs to other entities which can in turn use them to access the data.
  
  - There are a few particular things this allows the system to do in managing an app:
    - Assigning a URI doesn't require that the app remain running, so URIs can persist after their owning apps have exited. The system only needs to make sure that an owning app is still running when it has to retrieve the app's data from the corresponding URI.
    - These URIs also provide an important fine-grained security model. 
 - A content provider is implemented as a subclass of `ContentProvider` and must implement a standard set of APIs that enable other apps to perform transactions.

- A unique aspect of the Android system design is that any app can start another app’s component. For example, if you want the user to capture a photo with the device camera, there's probably another app that does that and your app can use it instead of developing an activity to capture a photo yourself. You don't need to incorporate or even link to the code from the camera app. Instead, you can simply start the activity in the camera app that captures a photo. When complete, the photo is even returned to your app so you can use it. To the user, it seems as if the camera is actually a part of your app.

### Activating components
Three of the four component types—activities, services, and broadcast receivers—are activated by an asynchronous message called an intent. Intents bind individual components to each other at runtime. 
- An intent is created with an Intent object, which defines a message to activate either a specific component (explicit intent) or a specific type of component (implicit intent).
- For activities and services, an intent defines the action to perform (for example, to view or send something) and may specify the URI of the data to act on, among other things that the component being started might need to know. 
- For broadcast receivers, the intent simply defines the announcement being broadcast. 
- Unlike activities, services, and broadcast receivers, content providers are not activated by intents. Rather, they are activated when targeted by a request from a `ContentResolver`. The content resolver handles all direct transactions with the content provider so that the component that's performing transactions with the provider doesn't need to and instead calls methods on the `ContentResolver` object. This leaves a layer of abstraction between the content provider and the component requesting information (for security).

- There are separate methods for activating each type of component:

    - You can start an activity or give it something new to do by passing an Intent to `startActivity()` or `startActivityForResult()` (when you want the activity to return a result).
    - With Android 5.0 (API level 21) and later, you can use the `JobScheduler` class to schedule actions. For earlier Android versions, you can start a service (or give new instructions to an ongoing service) by passing an Intent to `startService()`. You can bind to the service by passing an Intent to bindService().
    - You can initiate a broadcast by passing an Intent to methods such as `sendBroadcast()`, `sendOrderedBroadcast()`, or `sendStickyBroadcast()`.
    - You can perform a query to a content provider by calling `query()` on a `ContentResolver`.
- For more information about using intents, see the Intents and Intent Filters document. 

### The manifest file
- Before the Android system can start an app component, the system must know that the component exists by reading the app's manifest file, AndroidManifest.xml. Your app must declare all its components in this file, which must be at the root of the app project directory.

- The manifest does a number of things in addition to declaring the app's components, such as the following:

    - Identifies any user permissions the app requires, such as Internet access or read-access to the user's contacts.
    - Declares the minimum API Level required by the app, based on which APIs the app uses.
    - Declares hardware and software features used or required by the app, such as a camera, bluetooth services, or a multitouch screen.
    - Declares API libraries the app needs to be linked against (other than the Android framework APIs), such as the Google Maps library.

##### Declaring components
The primary task of the manifest is to inform the system about the app's components. For example, a manifest file can declare an activity as follows:


        <?xml version="1.0" encoding="utf-8"?>
        <manifest ... >
            <application android:icon="@drawable/app_icon.png" ... >
                <activity android:name="com.example.project.ExampleActivity"
                        android:label="@string/example_label" ... >
                </activity>
                ...
            </application>
        </manifest>

##### Declaring component capabilities
        <manifest ... >
            ...
            <application ... >
                <activity android:name="com.example.project.ComposeEmailActivity">
                    <intent-filter>
                        <action android:name="android.intent.action.SEND" />
                        <data android:type="*/*" />
                        <category android:name="android.intent.category.DEFAULT" />
                    </intent-filter>
                </activity>
            </application>
        </manifest>

##### Declaring app requirements
        <manifest ... >
            <uses-feature android:name="android.hardware.camera.any"
                        android:required="true" />
            ...
        </manifest>

