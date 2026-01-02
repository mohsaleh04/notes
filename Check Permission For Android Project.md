
کارهایی که باید اتنجام بشه ایناست:
1. باید پرمیشن ها (مجوز ها) فقط وقت هایی  که به او ها نیاز هست، درخواست بشن .. مثلاً اگه جایی به میکروفن احتیاج داشت، حتماً باید فقط همونجا بهش درخواست داده بشه، نه اینکه همین که اپ باز میشه (ChatsActivity یا DialogsActivity) نمایش داده بشه، 
   این مشکل باید برطرف بشه.
2. باید قبل درخواست هر پرمیشن یه rational به کاربر نمایش داده بشه که واقعا برنامه چرا نیاز به میکرفون فرد داره..
   **نکته**: خودت میتونی یه رشنال واسش بسازی .. خواستی هم میتونی از رشنال های خود سورس تلگرام هم استفاده کنی.

------------------------------------------------------------------------

### اینا کل مجوزهاییه که درخواست میکنه:

```XML
<uses-permission android:name="android.permission.READ_PRIVILEGED_PHONE_STATE" />  
<uses-permission android:name="android.permission.RECEIVE_SMS" />  
<uses-permission android:name="android.permission.REQUEST_IGNORE_BATTERY_OPTIMIZATIONS" />  
<uses-permission android:name="android.permission.ACCESS_NETWORK_STATE" />  
<uses-permission android:name="android.permission.INTERNET" />  
<uses-permission android:name="android.permission.FOREGROUND_SERVICE" />  
<uses-permission android:name="android.permission.RECORD_AUDIO" />  
<uses-permission android:name="android.permission.MODIFY_AUDIO_SETTINGS" />  
<uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />  
<uses-permission android:name="android.permission.WAKE_LOCK" />  
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />  
<uses-permission android:name="android.permission.POST_NOTIFICATIONS" />  
<uses-permission android:name="android.permission.DISABLE_KEYGUARD" />  
<uses-permission android:name="android.permission.READ_MEDIA_IMAGES" />  
<uses-permission android:name="android.permission.READ_MEDIA_VIDEO" />  
<uses-permission android:name="android.permission.READ_MEDIA_AUDIO" />  
<uses-permission android:name="android.permission.GET_ACCOUNTS" />  
<uses-permission android:name="android.permission.READ_CONTACTS" />  
<uses-permission android:name="android.permission.WRITE_CONTACTS" />  
<uses-permission android:name="android.permission.MANAGE_ACCOUNTS" />  
<uses-permission android:name="android.permission.READ_PROFILE" />  
<uses-permission android:name="android.permission.WRITE_SYNC_SETTINGS" />  
<uses-permission android:name="android.permission.READ_SYNC_SETTINGS" />  
<uses-permission android:name="android.permission.AUTHENTICATE_ACCOUNTS" />  
<uses-permission android:name="android.permission.VIBRATE" />  
<uses-permission android:name="android.permission.SYSTEM_ALERT_WINDOW" />  
<uses-permission android:name="android.permission.READ_PHONE_STATE" />  
<uses-permission android:name="android.permission.RECEIVE_BOOT_COMPLETED" />  
<uses-permission android:name="android.permission.USE_FINGERPRINT" />  
<uses-permission android:name="android.permission.USE_BIOMETRIC" />  
<uses-permission android:name="android.permission.INSTALL_SHORTCUT" />  
<uses-permission android:name="com.android.launcher.permission.INSTALL_SHORTCUT" />  
<uses-permission android:name="com.android.launcher.permission.UNINSTALL_SHORTCUT" />  
<uses-permission android:name="android.permission.CAMERA" />  
<uses-permission android:name="android.permission.BLUETOOTH" />  
<uses-permission android:name="android.permission.MANAGE_OWN_CALLS" />  
<uses-permission android:name="android.permission.USE_FULL_SCREEN_INTENT" />  
<uses-permission android:name="android.permission.REQUEST_INSTALL_PACKAGES" />  
<uses-permission android:name="android.permission.READ_PHONE_NUMBERS" />  
<uses-permission android:name="android.permission.OTHER_SENSORS" />  
<uses-permission android:name="com.sec.android.provider.badge.permission.READ" />  
<uses-permission android:name="com.sec.android.provider.badge.permission.WRITE" />  
<uses-permission android:name="com.htc.launcher.permission.READ_SETTINGS" />  
<uses-permission android:name="com.htc.launcher.permission.UPDATE_SHORTCUT" />  
<uses-permission android:name="com.sonyericsson.home.permission.BROADCAST_BADGE" />  
<uses-permission android:name="com.sonymobile.home.permission.PROVIDER_INSERT_BADGE" />  
<uses-permission android:name="com.anddoes.launcher.permission.UPDATE_COUNT" />  
<uses-permission android:name="com.majeur.launcher.permission.UPDATE_BADGE" />  
<uses-permission android:name="com.huawei.android.launcher.permission.CHANGE_BADGE" />  
<uses-permission android:name="com.huawei.android.launcher.permission.READ_SETTINGS" />  
<uses-permission android:name="com.huawei.android.launcher.permission.WRITE_SETTINGS" />  
<uses-permission android:name="android.permission.READ_APP_BADGE" />  
<uses-permission android:name="com.oppo.launcher.permission.READ_SETTINGS" />  
<uses-permission android:name="com.oppo.launcher.permission.WRITE_SETTINGS" />  
<uses-permission android:name="me.everything.badger.permission.BADGE_COUNT_READ" />  
<uses-permission android:name="me.everything.badger.permission.BADGE_COUNT_WRITE" />  
<uses-permission android:name="com.android.vending.BILLING" />  
<uses-permission  
    android:name="android.permission.WRITE_EXTERNAL_STORAGE"  
    tools:node="replace" />  
<uses-permission android:name="android.permission.ACCESS_BACKGROUND_LOCATION" />  
<uses-permission android:name="android.permission.FOREGROUND_SERVICE_DATA_SYNC" />  
<uses-permission android:name="android.permission.FOREGROUND_SERVICE_MEDIA_PLAYBACK" />  
<uses-permission android:name="android.permission.FOREGROUND_SERVICE_MEDIA_PROJECTION" />  
<uses-permission android:name="android.permission.FOREGROUND_SERVICE_MICROPHONE" />  
<uses-permission android:name="android.permission.FOREGROUND_SERVICE_CAMERA" />
```

این اون تابعیه که توی `onResume()` کال میشده و همه رو درخواست میکرده ... (_معمولاً_)

```java
if (checkPermission && !onlySelect && Build.VERSION.SDK_INT >= 23) {  
    Activity activity = getParentActivity();  
    if (activity != null) {  
        checkPermission = false;  
        AndroidUtilities.runOnUIThread(new Runnable() {  
            @Override  
            public void run() {  
                afterSignup = false;  
                if (activity.checkSelfPermission(Manifest.permission.READ_CONTACTS) != PackageManager.PERMISSION_GRANTED || activity.checkSelfPermission(Manifest.permission.RECORD_AUDIO) != PackageManager.PERMISSION_GRANTED || activity.checkSelfPermission(Manifest.permission.CAMERA) != PackageManager.PERMISSION_GRANTED || activity.checkSelfPermission(Manifest.permission.WRITE_EXTERNAL_STORAGE) != PackageManager.PERMISSION_GRANTED || (Build.VERSION.SDK_INT >= 33 && activity.checkSelfPermission(Manifest.permission.POST_NOTIFICATIONS) != PackageManager.PERMISSION_GRANTED)) {  
                    if (activity.shouldShowRequestPermissionRationale(Manifest.permission.READ_CONTACTS)) {  
                        AlertDialog.Builder builder = new AlertDialog.Builder(activity);  
                        builder.setTitle(LocaleController.getString("AppName", R.string.AppName));  
                        builder.setMessage(LocaleController.getString("PermissionContacts", R.string.PermissionContacts));  
                        builder.setPositiveButton(LocaleController.getString("OK", R.string.OK), null);  
                        showDialog(permissionDialog = builder.create());  
                    } else if (activity.shouldShowRequestPermissionRationale(Manifest.permission.WRITE_EXTERNAL_STORAGE)) {  
                        AlertDialog.Builder builder = new AlertDialog.Builder(activity);  
                        builder.setTitle(LocaleController.getString("AppName", R.string.AppName));  
                        builder.setMessage(LocaleController.getString("PermissionStorage", R.string.PermissionStorage));  
                        builder.setPositiveButton(LocaleController.getString("OK", R.string.OK), null);  
                        showDialog(permissionDialog = builder.create());  
                    } else if (activity.shouldShowRequestPermissionRationale(Manifest.permission.POST_NOTIFICATIONS)) {  
                        AlertDialog.Builder builder = new AlertDialog.Builder(activity);  
                        builder.setTitle(LocaleController.getString("AppName", R.string.AppName));  
                        builder.setMessage(LocaleController.getString("PermissionNOTIFICATIONS", R.string.PermissionNOTIFICATIONS));  
                        builder.setPositiveButton(LocaleController.getString("OK", R.string.OK), null);  
                        showDialog(permissionDialog = builder.create());  
                    } else if (activity.shouldShowRequestPermissionRationale(Manifest.permission.RECORD_AUDIO)) {  
                        AlertDialog.Builder builder = new AlertDialog.Builder(activity);  
                        builder.setTitle(LocaleController.getString("AppName", R.string.AppName));  
                        builder.setMessage(LocaleController.getString("PermissionNOTIFICATIONS", R.string.PermissionNoCamera));  
                        builder.setPositiveButton(LocaleController.getString("OK", R.string.OK), null);  
                        showDialog(permissionDialog = builder.create());  
                    } else if (activity.shouldShowRequestPermissionRationale(Manifest.permission.CAMERA)) {  
                        AlertDialog.Builder builder = new AlertDialog.Builder(activity);  
                        builder.setTitle(LocaleController.getString("AppName", R.string.AppName));  
                        builder.setMessage(LocaleController.getString("PermissionNOTIFICATIONS", R.string.PermissionNoCamera));  
                        builder.setPositiveButton(LocaleController.getString("OK", R.string.OK), null);  
                        showDialog(permissionDialog = builder.create());  
                    } else {  
                        askForPermissons();  
                    }  
                }  
            }  
        }, afterSignup ? 4000 : 500);  
  
  
    }  
} else
```

----------------------------------------------------------------------------------------------------------------------

### باگ ها:

1. بعد از حذف اینیشیال پرمیشن ها ، حالا دکمه های شروع تماس (چه صوتی، چه تصویری) نمیتونن کار کنن، چون مجوز ندارن و <u>نمیتونن اونو درخواست کنن!</u> 
2. مجوز نوتیفیکیشن و اون مجوز هایی که پایه ای هست رو همون اول برنامه بگیر .. مث `Post Notificaiton` و `Picture-In-Picture`
-------------------------------------
**باگ جدید**
رشنال ها به نظر درست پیاده سازی شدن..
فقط دوتا آیکون تماس های ویدیوئی و صوتی توی TitleBar اپ، به درستی پیاده سازی نشدن و وقتی مجوز رو رد میکنی، دیگه اون رشنالش نشون داده نمیشه و باید فیکس بشه . همچنین وفتی روی اون هیستوری  تماس ها توی چت، ضربه میزنیم هم دیگه هیچ پرمیشنی درخواست نمیکنه.. (_عجیبه که روی موبایل هوآویم این مشکلی نداره!:/_)

------------------------------------------------------------------------

### باگ آپلود پروفایل
لاگ:
```
09:20:13.124 14315-14499 okhttp....Client  I  --> POST https://inews.local.boshrapardaz.ir/apiservice/public/api/v1/profile/upload http/1.1
09:20:13.124 14315-14499                   I  Content-Type: multipart/form-data; boundary=ebe47a46-046d-40f6-860f-d4f89436c5a7
09:20:13.124 14315-14499                   I  Content-Length: 20129
09:20:13.124 14315-14499                   I  Authorization: Bearer 1344216|Zh7SmfEn133OphRXpuRyijMOyTDTXCrtFsViqVDD
09:20:13.124 14315-14499                   I  Clversion: 4.4.1
09:20:13.124 14315-14499                   I  Accept: application/json
09:20:13.124 14315-14499                   I  resource: AndroidI25301746977735396
09:20:13.124 14315-14499                   I  client_type: android
09:20:13.124 14315-14499                   I  Appname: Intragram
09:20:13.124 14315-14499                   I  Cltype: android
09:20:13.124 14315-14499                   I  info: {"CL":"android","VN":"4.4.1","VC":"109","ip":"102.237.146.64","device":"A52sxq","brand":"Samsung","SDK":34,"op":"IR-MCI","IMEI":"secId-3699801f6e91cb99"}
09:20:13.126 14315-14499                   I  --ebe47a46-046d-40f6-860f-d4f89436c5a7
09:20:13.126 14315-14499                   I  Content-Disposition: form-data; name="file"; filename="-2147483648_-210014.jpg"
09:20:13.126 14315-14499                   I  Content-Type: image/jpeg
09:20:13.126 14315-14499                   I  Content-Length: 19913
09:20:13.126 14315-14499                   I  
09:20:13.126 14315-14499                   I  -------------CONTENT--------------
09:20:13.128 14315-14499                   I 
09:20:13.129 14315-14499                   I  --ebe47a46-046d-40f6-860f-d4f89436c5a7--
09:20:13.130 14315-14499                   I  --> END POST (20129-byte body)
09:20:13.134 14315-14499                   I  <-- HTTP FAILED: java.net.ProtocolException: unexpected end of stream
09:20:13.141 14315-14315 UploadF...Number  E  line number 104 onFailure: com.androidnetworking.error.ANError: java.net.ProtocolException: unexpected end of stream
```


یه چیزی یادم اومد...
واسه مجوز نوتیفیکشن، یه سوییچ توی پرفرنس ها ست کن که آیا نشون دادی مجوز رو یا نه .. که اگه نخواست ،دیگه هی هر دفعه اصرار نکنه .. (**_حتماً با آقای کردآبادی هماهنگ باش!_**)

کدایی که فراخوانی میشن توی `ChatActivityBoshra` هستند . باید اینها رو یه بررسی داشته باشم ..

بررسی انجام شد . اما خب من حتی جایی که دکمه ها کار میکنن توی این کلاس دیدم ، اما نمیشد .. ینی دیالوگ برای یه لحظه باز میشه و بعد یهو بسته میشه ! 🤦

نمیدونم از کجا پرمیشن میگیره .. شاید توی سورس اصلی تلگرامه و نمیزاره درست این کد کار کنه. حالا امید وارم بتونم به کرد آبادی بگن و روش دقیقسش رو مطلع شم.

**FEATURE ADDED**

