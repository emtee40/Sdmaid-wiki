# AppCleaner
The AppCleaner is responsible for **expendable** files that can be attribuetd to a specific installed application. Files that are expedendable should fullfil the following criteria:
* App automatically recreates them if necessary
* Deleting them does not cause errors within an app
* No user created information is lost (e.g. a picture taken with the camera is "user created").

This generally includes all official cache locations (i.e. `/data/data/<pkg>/cache/` and `/<sdcard>/Android/data/<pkg>/cache/`) as well as uncommon cache locations (due to non-standard directory naming or location) and specific directories that have been manually added to SD Maids databases.

The AppCleaner works on a per App basis, meaning that any file it checks is related to a known installed app. This is in contrast to the SystemCleaner which is generally unaware of any app<->file relation.

[[[ https://cloud.githubusercontent.com/assets/1439229/19081899/361729ec-8a5c-11e6-97a0-3067db432b78.png | height = 300px]]](https://cloud.githubusercontent.com/assets/1439229/19081899/361729ec-8a5c-11e6-97a0-3067db432b78.png)
[[[ https://cloud.githubusercontent.com/assets/1439229/19081898/36126cc2-8a5c-11e6-8344-bd568411c4d8.png | height = 300px]]](https://cloud.githubusercontent.com/assets/1439229/19081898/36126cc2-8a5c-11e6-8344-bd568411c4d8.png)
[[[ https://cloud.githubusercontent.com/assets/1439229/19081900/361c5ea8-8a5c-11e6-8faa-96eb0f74ed8a.png | height = 300px]]](https://cloud.githubusercontent.com/assets/1439229/19081900/361c5ea8-8a5c-11e6-8faa-96eb0f74ed8a.png)
[[[ https://cloud.githubusercontent.com/assets/1439229/19081896/360df32c-8a5c-11e6-865a-43a815644185.png | height = 300px]]](https://cloud.githubusercontent.com/assets/1439229/19081896/360df32c-8a5c-11e6-865a-43a815644185.png)

## Filter

The filter configuration which can be accessed from the toolbar allows you to further configure which type of "expendable" files SD Maid looks for.

### Generic
Gerneric filters target files among all apps, e.g. all kinds of advertisement files in apps.

* "Default caches" targets the files in default `<pkg>/cache/` folders on public and private storages.
* "Hidden caches" targets cache like files that are in non-default cache directories, e.g. `videoCache`, `.cache`.
* "WebView caches" targets files downloaded by app internal browsers, e.g. if you an app shows you a webpage inside itself.
* "Bug reporting" targets files created by bug/crash reporting tools in apps. Turning this on might make it more difficult for developers to improve their app if it crashes on your device. It can be quite useful though if you don't have internet and an app still creates files that it will never be able to send.
* "Analytics" similar to "Bug reporting" just targeting analytics related files such as statistics about what you do in an app.
* "Advertisement" files created or belonging to advertisement functionality, e.g. caches or collected infos.

[[[ https://user-images.githubusercontent.com/1439229/30683752-f344ea10-9eae-11e7-8b82-56ea1478c408.png | height = 300px]]](https://user-images.githubusercontent.com/1439229/30683752-f344ea10-9eae-11e7-8b82-56ea1478c408.png)

### Specific
These filteres have specific targets such as certain filetypes or just one specific app.

* "WhatsApp sent files" targets files that WhatsApp creates when you send someone a picture of video through WhatsApp.
* "WhatsApp received files" files received through WhatsApp.
* "Telegram" targets received and send files by the app Telegram.
* "Threema" targets files received through the app Threema.
* "Old WhatsApp backups" targets WhatsApp message backups that are older than 1 day (so you keep the most recent one).

[[[ https://user-images.githubusercontent.com/1439229/30683753-f345f630-9eae-11e7-95da-f69a41142245.png | height = 300px]]](https://user-images.githubusercontent.com/1439229/30683753-f345f630-9eae-11e7-95da-f69a41142245.png)

## Settings
### Include System-Apps
Whether system apps should be included in the results.

### Sort mode
How the results should be sorted.

### Minimum cache age
SD Maid can filter out files that are below a minimum cache age (in days), e.g. keep files newer than `x` days. Generally speaking, cache is not a bad thing, so this setting allows for a compromise between performance and storage space. The default setting of `0` means that no age filter is applied and the maximum amount of files are suggested for deletion.
Note that SD Maid can not enforce this for all files on unrooted devices, see [Exclusions](https://github.com/d4rken/sdmaid-public/wiki/AppCleaner#some-appcleaner-exclusions-dont-work-without-root).

### Skip running apps
Skip currently running apps. If running apps are not skipped they are killed before deleting their cache. Apps will be killed 'softly', i.e. using [killBackgroundProcesses](https://developer.android.com/reference/android/app/ActivityManager.html#killBackgroundProcesses(java.lang.String)), even if root is available. For a running app the event will be similar to being killed in low memory situations. Killed background services may restart at will.

### Show inaccessible items
Some items may be known to SD Maid on unrooted devices, but can't be accessed directly (i.e. deleted). This is usually the private default cache at `/data/user/0/<pkg>/cache`. When this option is activated, SD Maid will query the system for an app's cache size and if available display a fake file for that location with the size the system has returned. 

This feature requires the ["Usage Statistics" permission](https://github.com/d4rken/sdmaid-public/wiki/Setup#usage-statistics-permission) on Android 8.0+. 

Activating the ["accessibility service feature"](https://github.com/d4rken/sdmaid-public/wiki/AppCleaner#accessibility-service) implicitly activates this feature too.

### freeStorageAndNotify
`freeStorageAndNotify` is a trick that can be used on some ROMs (< Android 6.0) to clear private caches without root. It basically triggers the `System>Storage>Clear Cache` action. This can be turned off because using it also clears the default public caches. If you have cache files in public caches that you don't want deleted, e.g. have exclusions for, you have to disable this function such that SD Maids exclusions can take effect (because using this function outsources some work to the system which doesn't know about SD Maids exclusions).

Also see [Exclusions](https://github.com/d4rken/sdmaid-public/wiki/AppCleaner#some-appcleaner-exclusions-dont-work-without-root).

### Accessibility service
Cache cleaning via accessibility service is the only way to clean private app caches on Android 6.0+ without root. When this feature is activated, SD Maid will open the system's settings window for each app and click the `Clear cache` button as fast as the system allows it.
With this option we can get closer to what is possible on a rooted device, without root access. Expendable files in private app storage, but outside of the default cache directory, are the remaining difference.

This feature implicitly enables the option "Show inaccessible items". On Android 8.0+ it requires the ["Usage Statistics" permission](https://github.com/d4rken/sdmaid-public/wiki/Setup#usage-statistics-permission) so SD Maid can determine how large the default caches are for each app. 

Caveats:
* This doesn't work via [Scheduler](https://github.com/d4rken/sdmaid-public/wiki/Scheduler) because SD Maid has to use the screen.
* Some [Exclusions](https://github.com/d4rken/sdmaid-public/wiki/AppCleaner#some-appcleaner-exclusions-dont-work-without-root) are ignored for paths covered by the system (`<pkg>/cache`) as SD Maid relies on the system to perform some of the deletion and has no control over the process at file level. Excluding a whole app should still work though.
* How fast this can be performed may depend on how long and fancy your system's animations are when switching screens.

####  Troubleshooting
* If deletion "seems" slow, and you see SD Maid "looping" multiple times on the same app, then SD Maid is looking for something and not finding it. To perform the automation, SD Maid relies on specific elements and texts on screen, if these change with a system update, then SD Maid may become lost.
    * As a temporary solution, you can try setting your system language to english.
    * A permanent solution requires an SD Maid update to include the new pattern of your ROM. Please create a bug ticket and include a [debug log](https://github.com/d4rken/sdmaid-public/wiki/Reporting-a-bug#debugrun-log) of SD Maid getting stuck and looping on an app.
    * On SD Maid 5.1.9+, you can also manually enter a custom sequence within the AppCleaner settings. It's not as powerful as adding support via update, but if your ROM's UI is not too complicated, you can often make it work.

* If you are getting an error about ROM & locale then SD Maid does not understand your devices language, create an issue ticket and provide the [necessary details](https://github.com/d4rken/sdmaid-public/issues/2396)
* If you get an error message on deletion stating that the accessibility service is enabled but not running, try restarting the device. The service is launched by the system, if it crashes or is killed, the system may consider it unreliable and not launch it again until reboot. The underlying cause is not clear:
    * There is currently no known error that could explain this, but if you record a logcat and spot an error, please open a ticket.
    * On some devices apps are killed by "RAM cleaners" or "battery optimizers", it's plausible that the act of killing SD Maid also tears down the accessibility service and makes it look like the service crashed, from the systems point of view.
    * Make sure SD Maid is placed in internal storage, not moved to external storage (i.e. App2SD etc.), if this is the solution for your device, please also open a ticket because I would like to better understand this behavior.
    * On BlackBerry devices the ["Power Centre" app](https://github.com/d4rken/sdmaid-public/issues/2628) may be killing the service.
    * On Xiaomi devices (MIUI ROMs) the system's autostart prevention may be affecting the service's reliability. Disable the service, reboot, enable autostart for SD Maid, then enable the accessibility service and reboot again. See [workaround here](https://github.com/d4rken/sdmaid-public/issues/4386#issuecomment-753662688) by [@spiritETG](https://github.com/spiritETG).

[[[ https://cloud.githubusercontent.com/assets/1439229/19081897/360e14b0-8a5c-11e6-81a5-0bf10d6acd01.png | height = 300px]]](https://cloud.githubusercontent.com/assets/1439229/19081897/360e14b0-8a5c-11e6-81a5-0bf10d6acd01.png)
[[[ https://user-images.githubusercontent.com/1439229/55688219-7c6c5c00-5976-11e9-8d9d-b9ab554f7145.png | height = 300px]]](https://user-images.githubusercontent.com/1439229/55688219-7c6c5c00-5976-11e9-8d9d-b9ab554f7145.png)

## FAQ
### When to use it?
#### Low space
Unless you are really low on free space it's generally not beneficial empty all your app caches every day. It's not harmful to your device or apps though. Low free space can negatively impact device performance, but this only happens below 10% or less (varies with storage) free space. Above it you would gain better performance by keeping a moderate file cache.

> Imagine an email app, its great to have fast access to some mails, but the cache also contains things like pictures from 3 day old emails. Now be honest how often do you really go back to view a few days old content? In most cases I'd rather have more free space and let the app just re-cache the files when they are needed. Additionally some of the cache files might already be stale now and would have to be reloaded anyways or not at all.

#### Misbehaving apps
Some apps create ridiculous amounts of files in their cache folders, some during normal operations, some because of bugs. Large amounts of small files can negatively impact app performance as traversing all these files costs time. There are cases where apps use buggy analytics software that causes them to never transmit and or delete their analytics files, leading to over ~50.000 files to accumulate and slowing down the app (through the slowed down analytics software).
It's also possible that apps screwed app their own cache management and start to display wrong files or fail to load, clearing their cache might help and is often less effort than reinstalling the app and setting it up again.

### Why delete cache?
First lets get something straight: **Caches are not a bad thing.** Loading files from cache instead of downloading or recalculating them again can make loading faster and save device resources (battery/cpu/bandwidth). 

In a perfect app world, all apps manage their cache perfectly and a tool such as this would be unnecessary. But the world isn't perfect and apps missmanage their cache by e.g. not limiting it's size. Often the user is given no control over it except for wiping the whole cache. The AppCleaner aims to provide the user with detailed information and more fine grained control over each apps cache.

### Why not just use the systems built-in functions?
There are locations with cache-like files that Androids built-in function does not cover, but AppCleaner does. 
The systems cache clearing routines only cover the default caches (e.g. `/data/data/<pkg>/cache` and `<sdcard>/Android/data/<pkg>/cache`).
Apps are not forced to use these directories and may also create directories and files on the root of the sdcard and save their cache in those directories or use weird constelations such as `.cache` or `/files/cache`. This may be unintentional due to mistakes or not knowing any better, but can also be deliberate. The AppCleaner helps you manage this.

### What about clearing cache via recovery?
The AppCleaner deletes individual application caches. Clearing cache via recovery wipes the `/cache` partition which is unrelated to app caches and contains system. 
On older Android versions (around 2.3?) the cache partition was heavily used to store temporary files when downloading an app through the Android Market. Today it's not that much in use on a daily base. AFAIK some built in backup code uses it, as well as recovery and OTA updates, though there is much variance between ROMs. In essence is not used by normal apps due to special permissions being required to access it.

### Differences between rooted and unrooted devices
The AppCleaner differs slightly between rooted and unrooted device. The cleanable amount is roughly the same, but unrooted devices can't clean some files individually, meaning that to remove certain files, you have to press the "Delete all" button. This is necessary because without root we have to abuse a hidden system function called `freeStorageAndNotify` which tricks the system into running it's own cache routine which deletes the official private app caches we can't access on unrooted devices.

This is means that you can still click an entry and it will clean just that app, but there might be additional files belonging to this app that can only be removed if you use the 'clean all' routine. Such entries are labeled with an extra and icon and will also display "20+? Elements" (this entry has 20 files/folders plus an unknown amount of extra items). It's unfortunately the only way to achieve this without the luxury of root as we can't access the files directly, but instead have to ask the system for this information and this is all the information/options we get. 

### Cache deletion on unrooted devices running Android 6.0+
The aforementioned `freeStorageAndNotify` trick is no longer available since Android 6.0. This means that SD Maid is not able to use this trick to delete the official private app cache on unrooted devices (i.e. `/data/data/<pkg>/cache`). SD Maid will still process official caches and all other types of expendable files on public storage.

Without root there is currently no known workaround without user interaction. As a manual workaround, to achieve pre 6.0 behavior, without root, you could manually clear the official caches through _Settings>Storage>ClearCache_ and then use SD Maid to find and delete expendable files (e.g. hidden-caches) that the systems built-in functionality doesn't cover. A possible compromise is the use of the "Accessibility API", [see here](https://github.com/d4rken/sdmaid-public/wiki/AppCleaner#accessibility-service).

Be aware that a lot of other apps are not aware of this change in Android 6.0 and may still seem to work on first sight, because they don't confirm their operations.

[[[ https://user-images.githubusercontent.com/1439229/41261716-51ce233c-6ddd-11e8-99f0-5a55dbcbfa24.png | height = 300px]]](https://user-images.githubusercontent.com/1439229/41261716-51ce233c-6ddd-11e8-99f0-5a55dbcbfa24.png)
[[[ https://user-images.githubusercontent.com/1439229/41261718-53ea9b28-6ddd-11e8-9418-214b7bad8227.png | height = 300px]]](https://user-images.githubusercontent.com/1439229/41261718-53ea9b28-6ddd-11e8-9418-214b7bad8227.png)
[[[ https://user-images.githubusercontent.com/1439229/41261723-561df39a-6ddd-11e8-9c75-80f82680efc7.png | height = 300px]]](https://user-images.githubusercontent.com/1439229/41261723-561df39a-6ddd-11e8-9c75-80f82680efc7.png)

### Some AppCleaner exclusions don't work without root
If you are using the AppCleaner on an unrooted device, you may have noticed that despite creating an exclusion for a specific app, it is still part of the scan results. This happens because when functions are used to delete caches that SD Maid doesn't have direct access to. If we cause the system to delete caches files for us, then the system does not know about the exclusions.

Affected functions:

* `freeStorageAndNotify` 
* Accessibility service

### Improving support for cache deletion via accessibility service
Some devices and or languages differ too much for SD Maid to automatically understand what has to be done. In such cases SD Maid will either abort quickly with an error message (`Unsupported ...`) or the screen will flicker on each app and progress will be very slow (as SD Maid retries until giving up and moving to the next app).

To add support in these cases, create a [new issue ticket](https://github.com/d4rken/sdmaid-public/issues/new) and provide 3 things:
* A step by step description of what has to be clicked on which screen to clear the cache.
* Screenshots of each screen mentioned in the step by step instructions, alternatively a video.
* A debug log showing how SD Maid sees the screen.

To record a debug log of SD Maids view:
* Start a debug log recording [as described here](https://github.com/d4rken/sdmaid-public/wiki/Reporting-a-bug#debugrun-log)
* While the recording is running, open SD Maid, open the debug menu (long press settings) and click "Debug Accessibility Service"
* Now a toolbar should have appeared on the bottom and SD Maid will additionally record everything seen from SD Maid's point-of-view into the log file
* Manually clear the cache for 1-2 apps
* Stop the recording and attach it with the screens to your ticket

Note that if something containing personal information became visible on screen while running "debug accessibility service" (incoming message) that this may be visible in the log, and you should consider repeating the process.