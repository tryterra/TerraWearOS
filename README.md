# TerraWearOS Library

This library should be used in conjunction with [TerraRTAndroid](https://github.com/tryterra/TerraRTAndroid).
It uses Bluetooth Connection to stream data to your android device and then from there streams to Terra through websockets!

**It is meant to be used for WearOS Apps!**
## Installation

Download the `.aar` file from this repository and include it within the `app/libs` folder in your project structure. You may now add it as a dependency in your gradle configuration files. In your project level gradle file (`build.gradle(:Project)`), edit the `repositories` to include:

```gradle
flatDir{
  dirs 'libs'
}
```

For example:
```gradle
repositories {
    google()
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
}
```

Then you must include the library and its dependent library in your app level gradle file(`build.gradle(:app)`) by adding the following lines under `dependencies`:

```gradle
implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-core-jvm:1.5.2'
implementation 'androidx.health:health-services-client:1.0.0-alpha03'
implementation 'com.google.guava:guava:30.1.1-android'
implementation 'androidx.wear:wear:1.2.0'
implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-core-jvm:1.5.2'
implementation 'com.google.code.gson:gson:2.8.7'
implementation files('libs/TerraWearOS-alpha.aar')

```

You may now import classes from the library as: `import co.tryterra.terrawearos.(Every class in this library imaginable (except private ones))`

## Usage

You will need to instantiate the `Terra` class as follows:

```kotlin
terra = Terra(context: Context, streamDataSet: Set<StreamDataTypes>
```

Upon initiating this class, you will be prompted with permissions for `Body Sensor`, `Location`, and `Activity Recognition`!

**Arguments**
- `context: Context` => The app context you are calling this class from
- `streamDataSet: Set<StreamDataTypes>` => The set of datatypes you wish to stream data for. Takes a set of `StreamDataTypes` enum.

### Initialise Bluetooth Connection

To put your device in discovery mode, run 
```kotlin
terra.startBluetoothDiscovery(callback: (Boolean) -> Unit)
```

You may now search for this device with the TerraRTAndroid package to initialise the connection!

The callback will be called when the connection is completed with either `true` (successful) or `false` (unsuccessful).

### Stream Data

You may now streaming data to your device!

```kotlin
terra.startStream()
```

You can also stop streaming by 
```kotlin
terra.stopStream()
```

### [WIP] Exercises (Available for testing)

You may also perform exercises on the device, and have the data streamed to your phone. 

You will first have to prepare the exercise by:

```kotlin
terra.prepareExercise(type: ExerciseTypes, dataTypes: Set<DataTypes>, shouldEnableGPS: Boolean, callback: (Boolean) -> Unit)
```

**This is only for warming up sensors!!**

**Arguments**
- `type: ExerciseTypes` => The type of exercise you wish to prepare for. Takes an `ExerciseTypes` enum.
- `dataTypes: Set<DataTypes>` => The datatypes from this exercise you wish to retrieve. Takes a set of `DataTypes` enum
- (Optional) `shouldEnableGPS: Boolean` => Default to false. Will determine if the location sensor needs to be warmed up or not ;)
- (Optional) `callback` => A callback function indicating when and if the preparation of the exercise is completed and successful **(RECOMMENDED)**

After preparation, you may then start an exercise:

```kotlin
terra!!.startExercise(
  type: ExerciseTypes,
  dataTypes: Set<DataTypes>,
  shouldEnableGPS: Boolean,
  shouldEnableAutoPauseAndResume: Boolean,
)
```

**Arguments**
- `type: ExerciseTypes` => The type of exercise you wish to prepare for. Takes an `ExerciseTypes` enum.
- `dataTypes: Set<DataTypes>` => The datatypes from this exercise you wish to retrieve. Takes a set of `DataTypes` enum
- (Optional) `shouldEnableGPS: Boolean` => Default to false. Allow for location data streaming
- (Optional) `shouldEnableAutoPauseAndResume: Boolean` => Defaults to false. Allow for the watch to automatic pause and resume exercise.

Doing so will automatically start streaming data to your device!

There is also the capability to pause, resume, and stop the exercise:

```kotlin
terra.stopExercise()
terra.resumeExercise()
terra.pauseExercise()
```
### Demo

A demo [application](https://github.com/tryterra/TerraWearOSDemo) is written to use this library.



