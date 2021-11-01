# Intents, Activities, and SharedPreferences
## Tasks and the back stack
A task is a collection of activities that users interact with when trying to do something in your app. These activities are arranged in a stack—the back stack—in the order in which each activity is opened.

### Lifecycle of a task and its back stack
![tasksLifeCycle](https://developer.android.com/images/fundamentals/diagram_backstack.png)

- If the user continues to press or gesture Back, then each activity in the stack is popped off to reveal the previous one, until the user returns to the Home screen (or to whichever activity was running when the task began). When all activities are removed from the stack, the task no longer exists.

#### Back press behavior for root launcher activities

- Root launcher activities are activities that declare an Intent filter with both `ACTION_MAIN` and `CATEGORY_LAUNCHER`. These activities are unique because they act as entry points into your app from the app launcher and are used to start a task.

- When a user presses or gestures Back from a root launcher activity, the system handles the event differently depending on the version of Android that the device is running

- **System behavior on Android 11 and lower** : The system finishes the activity.
- **System behavior on Android 12 and higher** : The system moves the activity and its task to the background instead of finishing the activity. 

#### Background and foreground tasks
- A task is a cohesive unit that can move to the background when a user begins a new task or goes to the Home screen.While in the background, all the activities in the task are stopped, but the back stack for the task remains intact—the task has simply lost focus while another task takes placetask can then return to the foreground so users can pick up where they left off.
![backAndFore](https://developer.android.com/images/fundamentals/diagram_multitasking.png)

#### Multiple activity instances
- Because the activities in the back stack are never rearranged, if your app allows users to start a particular activity from more than one activity, a new instance of that activity is created and pushed onto the stack (rather than bringing any previous instance of the activity to the top). 
![sAcMtime](https://developer.android.com/images/fundamentals/diagram_multiple_instances.png)

#### Multi-window environments
- When apps are running simultaneously in a multi-windowed environment, supported in Android 7.0 (API level 24) and higher, the system manages tasks separately for each window; each window may have multiple tasks. The same holds true for Android apps running on Chromebooks: the system manages tasks, or groups of tasks, on a per-window basis.


### Manage tasks
 you can manage task with attributes in the `<activity>` manifest element and with flags in the intent that you pass to `startActivity()`.

* #### Defining launch modes
     - Launch modes allow you to define how a new instance of an activity is associated with the current task. You can define different launch modes in two ways:

        - Using the manifest file
        - Using Intent flags : `startActivity()` and `Intent`

* #### Define launch modes using the manifest file
-  you can specify how the activity should associate with a task using the `<activity>` element's launchMode attribute.
- The `launchMode` attribute specifies an instruction on how the activity should be launched into a task

- There are four different launch modes you can assign to the launchMode attribute:
    - **standard** (the default mode) : Default. The system creates a new instance of the activity in the task from which it was started and routes the intent to it.
    - **singleTop** : If an instance of the activity already exists at the top of the current task, the system routes the intent to that instance through a call to its `onNewIntent()` method, rather than creating a new instance of the activity. The activity can be instantiated multiple times, each instance can belong to different tasks, and one task can have multiple instances (but only if the activity at the top of the back stack is not an existing instance of the activity).
    - **singleTask** : The system creates a new task and instantiates the activity at the root of the new task.However, if an instance of the activity already exists in a separate task, the system routes the intent to the existing instance through a call to its `onNewIntent()` method, rather than creating a new instance. Only one instance of the activity can exist at a time.
    - **singleInstance** : Same as "singleTask", except that the system doesn't launch any other activities into the task holding the instance. The activity is always the single and only member of its task; any activities started by this one open in a separate task.
    ![singletask](https://developer.android.com/images/fundamentals/diagram_backstack_singletask_multiactivity.png)

#### Define launch modes using Intent flags

- **`FLAG_ACTIVITY_NEW_TASK`** :Start the activity in a new task. If a task is already running for the activity you are now starting, that task is brought to the foreground with its last state restored and the activity receives the new intent in `onNewIntent()`. This produces the same behavior as the **"singleTask"** `launchMode` value, discussed in the previous
- **`FLAG_ACTIVITY_SINGLE_TOP`** : If the activity being started is the current activity (at the top of the back stack), then the existing instance receives a call to `onNewIntent()`, instead of creating a new instance of the activity. This produces the same behavior as the *"singleTop"* `launchMode` value, discussed in the previous section.
 - **`FLAG_ACTIVITY_CLEAR_TOP`** : If the activity being started is already running in the current task, then instead of launching a new instance of that activity, all of the other activities on top of it are destroyed and this intent is delivered to the resumed instance of the activity (now on top), through `onNewIntent()`). There is no value for the launchMode attribute that produces this behavior. `FLAG_ACTIVITY_CLEAR_TOP` is most often used in conjunction with `FLAG_ACTIVITY_NEW_TASK`. When used together, these flags are a way of locating an existing activity in another task and putting it in a position where it can respond to the intent.

#### Handle affinities

- An affinity indicates which task an activity prefers to belong to. By default, all the activities from the same app have an affinity for each other. So, by default, all activities in the same app prefer to be in the same task. However, you can modify the default affinity for an activity. Activities defined in different apps can share an affinity, or activities defined in the same app can be assigned different task affinities.
    - You can modify the affinity for any given activity with the `taskAffinity` attribute of the `<activity>` element.

    - The `taskAffinity` attribute takes a string value, which must be unique from the default package name declared in the `<manifest>` element, because the system uses that name to identify the default task affinity for the app.

#### Clear the back stack

- **`alwaysRetainTaskState`** : If this attribute is set to **"true"** in the root activity of a task, the default behavior just described does not happen. The task retains all activities in its stack even after a long period.
- **`clearTaskOnLaunch`** : If this attribute is set to **"true"** in the root activity of a task, the task is cleared down to the root activity whenever the user leaves the task and returns to it. In other words, it's the opposite of alwaysRetainTaskState. The user always returns to the task in its initial state, even after leaving the task for only a moment.
- **`finishOnTaskLaunch`** : This attribute is like clearTaskOnLaunch, but it operates on a single activity, not an entire task. It can also cause any activity to finish, except for the root activity. When it's set to "true", the activity remains part of the task only for the current session. If the user leaves and then returns to the task, it is no longer present.

#### Start a task

- You can set up an activity as the entry point for a task by giving it an intent filter with **"android.intent.action.MAIN"** as the specified action and **"android.intent.category.LAUNCHER"** as the specified category. For example:

        <activity ... >
            <intent-filter ... >
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
            ...
        </activity>

### Save key-value data
#### Get a handle to shared preferences
* You can create a new shared preference file or access an existing one by calling one of these methods:

    - `getSharedPreferences()` — Use this if you need multiple shared preference files identified by name, which you specify with the first parameter. You can call this from any `Context` in your app.
    - `getPreferences()` — Use this from an Activity if you need to use only one shared preference file for the `activity`. Because this retrieves a default shared preference file that belongs to the activity, you don't need to supply a name.

#### Write to shared preferences
- To write to a shared preferences file, create a `SharedPreferences.Editor` by calling `edit()` on your `SharedPreferences`.
- Pass the keys and values you want to write with methods such as `putInt()` and `putString()`. Then call `apply()` or `commit()` to save the changes
- `apply()` changes the in-memory `SharedPreferences` object immediately but writes the updates to disk asynchronously. Alternatively, you can use `commit()` to write the data to disk synchronously. But because `commit()` is synchronous, you should avoid calling it from your main thread because it could pause your UI rendering.

        SharedPreferences sharedPref = getActivity().getPreferences(Context.MODE_PRIVATE);
        SharedPreferences.Editor editor = sharedPref.edit();
        editor.putInt(getString(R.string.saved_high_score_key), newHighScore);
        editor.apply();

#### Read from shared preferences
- To retrieve values from a shared preferences file, call methods such as getInt() and getString(), providing the key for the value you want, and optionally a default value to return if the key isn't present

        SharedPreferences sharedPref = getActivity().getPreferences(Context.MODE_PRIVATE);
        int defaultValue = getResources().getInteger(R.integer.saved_high_score_default_key);
        int highScore = sharedPref.getInt(getString(R.string.saved_high_score_key), defaultValue);