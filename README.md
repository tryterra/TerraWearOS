# TerraWearOS Library

A library built with Kotlin 1.5.31 to connect WearOS Devices to Terra Enabling Developers LTD.

As WearOS will only provide real-time data, this package will contain components to stream data straight to our servers and into your webhooks!

## Import the library

You will first need to import the library into your project. Typically this is located under `.app/libs`. Upon doing so, you can then call it as a dependency on your `build.gradle` file.

## Permissions 

The package will require you to request 3 different permissions from the user namely:

`android.permission.BODY_SENSORS` \
`android.permission.ACCESS_FINE_LOCATION` \
`android.permission.ACTIVITY_RECOGNITION` 

## Connect to Terra

A coroutine function is provided to help you connect to Terra easily through an Internet connection.

```kotlin
val connectTerra: ConnectTerra = ConnectTerra("YOUR_X_API_KEY", "YOUR_DEV_ID")
connectTerra.connectTerra()
```

Upon successful connection, this will update the class property `ConnectTerra.userId` to have a Terra User ID associated to the user connecting to our servers.

Please ensure this connection is successful before continuing in your app.

## Daily Data

There is a polling system within the library to poll Daily Data, namely the fields: `Daily Calories`, `Daily Steps`, and `Daily Distance`, and `Daily Floors`.

You may instantiate this polling system as follows:

```kotlin
private val scheduler = DataPostScheduler("INTERVAL_IN_MILLI_SECONDS", "INITIAL_DELAY_IN_MILLI_SECONDS, "YOUR_X_API_KEY", "YOUR_DEV_ID", "TERRA_USER_ID")
```

You can then start the scheduler by running: 

```kotlin
scheduler.start()
```

You may also stop it by running:

```kotlin
scheduler.stop()
```

## Exercise Data

Exercise Data and the lifecycle of an Exercise on WearOS can be monitor and manipulated by the package's `HealthServicesManager` class. You may instantiate this class after the user is connected. For convenience, you may also use `Inject` for initialization as:

```kotlin
@Inject lateinit var healthServicesManager: HealthServicesManager
```

### Exercise Life Cycle

- Preparing Exercise: The exercise must be prepared (to warm up sensors). This can be controlled by:
```kotlin
healthServicesManager.prepareExercise("EXERCISE_TYPE")
```
- Starting Exercise: To start the exercise:
```kotlin
healthServicesManager.startExercise("EXERCISE_TYPE")
```
- Pausing Exercise: To pause the exercise:
```kotlin
healthServicesManager.pauseExercise()
```
- Resuming Exercise: To resume the exercise:
```kotlin
healthServicesManager.resumeExercise()
```
- End Exercise: To stop the exercise:
```kotlin
healthServicesManager.endExercise()
```

### Extra utility functions (Suspended Functions)

```kotlin
checkExerciseTypeCapabilities("EXERCISE_TYPE")
```
Returns a [`ExerciseTypeCapabilities?`](https://developer.android.com/reference/androidx/health/services/client/data/ExerciseTypeCapabilities) Class containing all the capabilities of the `Exercise Type` given.

```kotlin
allAvailableCapabilities()
```
Returns a Set of [`DataType`](https://developer.android.com/reference/androidx/health/services/client/data/DataType?hl=en) containing all the capabilities of the device.

```kotlin
hasCapability("DATATYPE")
```
Return true if the device has the capability to measure a `DataType`. False otherwise.

```kotlin
isExerciseInProgress()
```
Returns true if there is already ane exercise happening. False otherwise.

```kotlin
isTrackingExerciseInAnotherApp()
```
Returns true if there are exercises happening on another app. False otherwise.


```kotlin
registerForAllData(Set<"DATATYPES">)
```
Registers for background (passive) data for the given set of `Datatypes`

```kotlin
unregisterAllData()
```
Unregisters for all the pasive data registered.

```kotlin
markLap()
```
Marks lap on the exercise client from Health Services.


### Demo

A demo [application](https://github.com/tryterra/TerraWearOSDemo) is written to use this library.



