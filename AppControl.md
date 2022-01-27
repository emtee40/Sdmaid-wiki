# AppControl
Is your one stop shop for all action related to controlling installed applications. After scanning you will have a searchable list of your installed apps. System apps are not shown by default, this has to be enabled in the settings.
Some options may only be available (possible) if SD Maid has been granted root permissions.

The list of apps is by default sorted by their installdate, with the newest apps first. Each app entry contains the app icon on the left, the app name and their package name, as well as the total app size on the row end. The bottom of each app entry contains a set of tags with specific attributes an app may have.

* `System` indicates that this app is a system. This may entail that it is priviledged to enhanced app permissions and actions regarding the app may be limited on unrooted devices.
* `Frozen` the app is disabled. It may not launch by any means and it will not be visible in the devices app launcher.
* `Stopped` the app has been forcefully stopped. It may not launch itself by any means until it has been manually launched by you.
* `Boot` the app contains broadcast receivers that allow the applications to do work after device reboots. If the app has such broadcast receivers but none are enabled, the tag is slightly greyed out.

Some actions can be executed on multiple apps, some only on single items. To execute actions on multiple apps long press an app then tap to select additional apps.

[[[ https://cloud.githubusercontent.com/assets/1439229/14576165/48de7be2-0367-11e6-8cdb-c9743011d39b.png | height = 300px]]](https://cloud.githubusercontent.com/assets/1439229/14576165/48de7be2-0367-11e6-8cdb-c9743011d39b.png)
[[[ https://cloud.githubusercontent.com/assets/1439229/14576166/4a13d6ba-0367-11e6-8820-5fb317144d5a.png | height = 300px]]](https://cloud.githubusercontent.com/assets/1439229/14576166/4a13d6ba-0367-11e6-8820-5fb317144d5a.png)
[[[ https://cloud.githubusercontent.com/assets/1439229/14576235/aed12ac6-0367-11e6-8ccd-a1b3a69a8f5a.png | height = 300px]]](https://cloud.githubusercontent.com/assets/1439229/14576235/aed12ac6-0367-11e6-8ccd-a1b3a69a8f5a.png)
[[[ https://cloud.githubusercontent.com/assets/1439229/14576303/268eff3e-0368-11e6-8710-09e13f663354.png | height = 300px]]](https://cloud.githubusercontent.com/assets/1439229/14576303/268eff3e-0368-11e6-8710-09e13f663354.png)
[[[ https://cloud.githubusercontent.com/assets/1439229/18914719/9769f71a-858e-11e6-8af8-d2eb1ec76073.png | height = 300px]]](https://cloud.githubusercontent.com/assets/1439229/18914719/9769f71a-858e-11e6-8af8-d2eb1ec76073.png)
[[[ https://cloud.githubusercontent.com/assets/1439229/18914718/974bfc24-858e-11e6-94c7-e5a612f48567.png | height = 300px]]](https://cloud.githubusercontent.com/assets/1439229/18914718/974bfc24-858e-11e6-94c7-e5a612f48567.png)
[[[ https://cloud.githubusercontent.com/assets/1439229/18858247/d70efd4a-846a-11e6-8f58-4886613a199f.png | height = 300px]]](https://cloud.githubusercontent.com/assets/1439229/18858247/d70efd4a-846a-11e6-8f58-4886613a199f.png)

## Actions
Some of these actions are only accessible in the apps details which can be reached by tapping an entry in the list.

### Run app
Launches the app.

### Kill app
Tries to kill the app. 

If root permission are available, SD Maid will use the linux `kill` applets on all pids running under that package's main process. This is final and equal to an app having never been started.

There is no comparable Android function to kill an app like that without root. The closest thing available would be to manually force stop the app but this can not be done programmatically by an app (without root). SD Maid uses the only available method that is left, which is [killBackgroundProcesses](https://developer.android.com/reference/android/app/ActivityManager.html#killBackgroundProcesses(java.lang.String)). This is equal to _nicely_ asking the system to kill an app. A few exception apply though. Generally all apps run by the system user (i.e. system apps) are off-limits. If we closely look at the methods name we notice it says `Background` and might ask ourselves, "What is a background process?". As with all things Android there are different answers to that depending on your Android version. The rule of thumb is that anything that doesn't have an active user-interface element (e.g. a window or on-going notification) is considered a background process by the system. The end effect is equal to what the system does when killing an app to reclaim memory, which is what the system regularly does, and the system may also restart an app if the memory is available again. The same goes for apps killed with this method which is why there isn't much benefit to any kind of non-root "task killing/cleaning" apps.

### Force stop app
Tries to force stop the app. Essentially the same action as you have in the systems app settings. For an app do this we need root permission. The app can't launch itself until you open it once manually. This state will persist even if SD Maid is uninstalled.

### Export apk
Tries to copy the apps `.apk` file from internal private storage to your public storages download directory. If the app is encrypted, then this action requires root, unencrypted apps can also be exported without root.

### Freeze app
Requires root. Disables the app. The app will not be visible and can't start itself. It has to be enabled again to work. No dataloss occurs and this state also persists if you uninstall SD Maid. Any good app enabled/disabler app can enable apps that have been disabled with SD Maid and vice versa.

### Move app
Requires root. If your device supports it, you can move apps from internal to external storage and vice versa. If an app does not want to be moved, SD Maid can force it, but the moved app may no longer run correctly. Excepted for the "force" option, this action is the same one as moving the app from the systems app menu. You can select multiple apps in the list and move them in batches.

### Delete app
Deletes the app and it's data. This includes the systems uninstall process as well as actions similar to SD Maids [Corpsefinder](https://github.com/d4rken/sdmaid-public/wiki/Corpsefinder). Unrooted devices have to confirm each uninstall, rooted devices can batch uninstall without extra confirmation.

### Reset app
Returns the app to its install state. This will delete the apps data, but not the app itself. Similar to `Clear data` in the systems app settings, but it also includes extra app directories outside of default directories, that the system does not know about.

### Receiver manager (autostart)
This feature requires root. It allows you to manage [BroadcastReceiver](http://developer.android.com/reference/android/content/BroadcastReceiver.html). These are Android components that can react to specific events such as incoming calls, display state changing or plugging the device in the charger. Receivers can be enabled or disabled. Similar to disabling whole apps, these actions persist even if uninstalling SD Maid. Other root tools if well written should be able to undo SD Maids operations and vice versa.

BroadcastReceiver can listen to custom internal events (`Intent`'s) as well as predefined system events. If SD Maid can attribute the BroadcasterReceiver as being register for a specific system event, the event type will be listed below the receiver name. Note that one receiver can be responsible for multiple event types.

_**Use this with caution, disabling items without knowing what you are doing will cause the app to malfunction in unforseen ways.**_

A broadcast receiver gives an app a short execution window after specific events happen, it does not mean the app keeps running.

The most popular type of receiver people are usually looking for are `ON_BOOT` receivers which allow an app to do actions after booting/rebooting the device. `ON_BOOT` receivers are not equal to Window's autostart. While a badly written app can slow down device boots significantly if executes useless actions directly after booting, it is important to note that there are many **legit** use-cases for such a receiver. Specifically in SD Maids own case, it is required to restore the alarm for the [Scheduler](https://github.com/d4rken/sdmaid-public/wiki/Scheduler).

#### Does disabling receivers improve battery life noticeably?
Sometimes, but usually not. Work done within a `BroadcastReceiver` is restricted. A receiver has to finish in [under 10 seconds](https://developer.android.com/reference/android/content/BroadcastReceiver#goAsync()).

Any longer running operation has to be run within a [`Service`](https://developer.android.com/reference/android/app/Service). A running service is usually visible via notification and since Android P is actually forced to show a notification when running. Services running without notifications are more prone to being killed by the system too! Most receivers just run a small piece of code that checks a few conditions and set something up (e.g. an alarm) or decide whether to launch a service or not. This is usually finished in less then 10ms. So the impact of a receiver on your battery depends on whether it launches a service that has an impact on your battery life ;).

#### Does disabling receivers improve performance?
In some cases, but not generally. The same caveats apply as mentioned above. To have an overall impact on performance the operation would have to keep running all the time which is just not what receivers do. That being said, it's possible to impact your performance momentarily. E.g. by disabling receivers that react to ON_BOOT you can theoretically make your device complete the boot up faster.

An uncommon but not rare practice is to read some data from storage within receivers. On most Android devices the performance bottleneck is I/O, not CPU (e.g. start a large file copy and try to multitask). If an app would do large amounts of I/O during it's 10s execution window then your device could feel slow.

Unless you have a collection of very badly written apps (why?!), it's more about preventing specific actions from happening, than improving performance/battery life.

[[[ https://cloud.githubusercontent.com/assets/1439229/14576238/b518f436-0367-11e6-9c18-c313471908a2.png | height = 300px]]](https://cloud.githubusercontent.com/assets/1439229/14576238/b518f436-0367-11e6-9c18-c313471908a2.png)
[[[ https://cloud.githubusercontent.com/assets/1439229/14576241/b7e336cc-0367-11e6-8ee9-c05d494acb62.png | height = 300px]]](https://cloud.githubusercontent.com/assets/1439229/14576241/b7e336cc-0367-11e6-8ee9-c05d494acb62.png)
[[[ https://cloud.githubusercontent.com/assets/1439229/14576243/bb52af4a-0367-11e6-94da-d27fe173065b.png | height = 300px]]](https://cloud.githubusercontent.com/assets/1439229/14576243/bb52af4a-0367-11e6-94da-d27fe173065b.png)
[[[ https://cloud.githubusercontent.com/assets/1439229/14576245/bc7b1d4e-0367-11e6-9f06-71c6c06e8054.png | height = 300px]]](https://cloud.githubusercontent.com/assets/1439229/14576245/bc7b1d4e-0367-11e6-9f06-71c6c06e8054.png)

## Settings
### Include System-Apps
Whether SD Maid should show system apps in the list.

### Keep desirable remnants
Some files belonging to an app may still be useful after uninstalling the app, e.g. photos or backups. Enabling this setting would cause SD Maid to, e.g. remove backups from TitaniumBackup if you uninstall TitaniumBackup.

### Preload app estate
An apps estate is the collection of all the files and directories it owns across all detected storages. This is disabled by default to speed up the app search. Sorting by size will implicitly enable this. If app estate is not preloaded, you can still press "Scan" in the size box within an apps details.

### Export destination
Where files (e.g. share app list or exported apks) are placed.

### Double check apps
Some ROMs don't correctly update an apps state (e.g. enabled/disabled) (as returned through the PackageManager API). Enabling this option will cause SD Maid to double check the state of each app received through the API with the content of the packages.xml file. **Unless you are have issues with exactly this, don't enable it.**

### Add shortcut
Adds an app shortcut to your home screen that directly opens the appcontrol page and refreshs it.

### Custom action
This option enables a new context menu action within AppControl that will execute accessibility service based UI automation.
Think of it like [AppCleaner's accessibility service option](https://github.com/d4rken/sdmaid-public/wiki/AppCleaner#accessibility-service), just more generalized. SD Maid will start clicking the entered texts for every app you execute this action on.

Execution always starts on the system details page of the target app. After that it's up to you, e.g. you could use it to batch force stop apps with a sequence like:
```
Force stop
Ok
```
First press `Force stop`, then press `Ok` on the confirmation dialog.


[[[ https://cloud.githubusercontent.com/assets/1439229/20377644/4e9fc21a-ac91-11e6-8315-ec3e67d5ba88.png | height = 300px]]](https://cloud.githubusercontent.com/assets/1439229/20377644/4e9fc21a-ac91-11e6-8315-ec3e67d5ba88.png)