گفته شد که کلاسهایی که مربوط به `VideoCall` هستن رو یه چکی داشته باشم...

`VideoCallhelper` => برایدرخواست های video کال استفاده میشه

حتماً کلاس `Voip Activity  Boshra` را رو یه بررسی داشته باشم.
همه پکیج های مربوط به تماس تصویری در پکیج `videocall` هستند.

سرور هایی که استفاده شده -> جیتسی برای تماس تصویری هستش . 

> یه باگی پیدا کردم که وقتی که تماس صوتی یا تصویری رو یکی زنگ میزنه و من مثلاً موبایلم خاموشه یا در دسترس نیستم، اون نوتیفیکشنش همچنان اون باالا هست حتی وقتی که تماس تموم شده و بی پاسخ مونده.


------------------------------------------------------------------------
 %%~~خب Intragram-default-config
  کاری که باید انجام بدی اینه که اون مقادیر پیشفرض رو که میخواد ست کنه، روی ادیت تکست ها اعمال کنه.. 
و بعد یه تابعی بسازی براش که بید اطلاعات رو از پارامتر بگیره و بعد اونها رو ذخیره کنه .. 
ارگومان ها رو از روی ادیت تسک ها بخون%%~~
**فیچر اضافه شد!**

------------------------------------------------------------------------

-  [ ] خواندن سورس `ChatAttachAlertBoshra` برای فهمیدن اشکال کد (اشکال اینه که بعد از تأیید permission دیگه اون عکس ها نمیاد و  آپدیت نمیشه)  
-  [ ] دیباگ اون اشکال
--------------------------------------------------------------------------
%%یه باگی توی تماس (صفحه تماس) پیدا کردم که این چندتاس:
1. آیکون های مربوط به اقدامات توی تماس ،خیلی کوچیک و کنار هم هستن و حالت Disable و Enable هم ندارن مث اینکه.
2. همچنین وقتی یه طرف، تماس رو هولد کرده (نگه داشته) طرف مقابل هم میتونه اونو هولد کنه! 
3. و اینکه کلاً اصن از خود کردآبای واسه چی اون ریپوی خود تلگرام رو آپدیت نمیکنن و روی این فِچ نمیزنن
4. و بپرس چرا حداقل api 24 قبوله و اندروید های 6 و 5 مجاز به استفاده نیستن؟
   همچنین .. سؤال کن . این کند بودنش به خاطر دیپندنسی هایی که داره نیس؟%%
------------------------------------------------------------------------
## ویدئو کنفرانس ... (تماس تصویری)

برای شروع اول اول...
1. دوتا استپ های `createRoom` و `joinRoom` رو بررسی کنم که اگه پارامتر های اضافی داشته باشه، حذف یا اگه چیزی جدید شده بود با آقای صفایی صحبت کنم و اینهات رو سینک کنم....
2. اصل کار روی `addMember` هستش ....  باید این رو اول بررسی کنم که اگه تغییراتی داشته در سطح API با آقای صفایی یهع چکی داشته باشم...
3. و اینکه  بعد باید وقتی دکمه افزودن اعضا زده شد که در `VoipActivityBoshra` هست فک ککنم... 
   اونجا اسم همه رو بیاره (کارت هاش رو خودت باید درستش کنی) و وضعیت فعلی کاربران مثلاً اینمکه آیا افلاین هستن یا نه آیا در تماس هستند یا نه .. و این صوبتا، کلاً این ها رو بررسی کنیم .. و این Card هاش رو در `VideoCallMembersBottomSheet` و `VideoCallMembmersCellBoshra` درستش کنم../..

>Apis => 
1- Create room 
2-Join room
3- Call-members
4- Decline
5-members 
--> roomMembers
------

`statusTextView` ها رو تست کن
> باگ مربوط بهشون رو فیکس کن...
> 
> inroom
> wating
> ringing
> busy
> decline

------------------------------------------------------------------------

api هاش رو بدست بیار ...
از توی خود سایت .. بعد برو اونجا توی APiServiceش هست . اونجا رو یه چک بزن.

### API endpoints

1. request-room -> request a room
2. call-members -> call to all of the meet members
3. join-room -> join to an existing room
4. call-exists-room -> check is wanted room exists
5. state-notify -> notify to memr state of 
6. ...

requestCallingToMember -> call-members (valid)


`sipPhone` مقدارش توی state-notify برای چیه

خب تا اینجا این 5 تا رو هندل کردم ... مونده بقیه اش...

بعداً یه `video-notify` واسم میفرستن که با اون تشخیص میدیم، وضعیت فعلی کاربر چیه


متن تایتل بالای اسما رو پاک کن .. که اسم کاربرها رو توی صفحه تماس مینویسه .. واسه تماس صوتیه .. برای تماس تصویری خیلی معنی نداره

## 1
```xml
<message xmlns='jabber:client' xml:lang='notify_video' to='990000003834@inews.local.boshrapardaz.ir' from='990000003834@inews.local.boshrapardaz.ir' type='headline' id='k5C3kK'><boshra message_type='0'/><body>{&quot;user_server_id&quot;:&quot;990000003834&quot;,&quot;roomName&quot;:&quot;2025_5_22_f611ea33-3d1d-4e39-a04e-88aa35ef3d14&quot;,&quot;resource&quot;:&quot;webclient229555775470117747502481964549&quot;,&quot;uuid&quot;:&quot;87AABC43-11C6-4673-A3B6-76CDC52C0CC0&quot;,&quot;roomMember&quot;:{&quot;server_id&quot;:&quot;990000003834&quot;,&quot;serverName&quot;:&quot;Saleh Masoudi&quot;,&quot;serverLastName&quot;:&quot;&quot;,&quot;avatar&quot;:174712018415127,&quot;user_role&quot;:1},&quot;member&quot;:{&quot;server_id&quot;:&quot;990000003834&quot;,&quot;serverName&quot;:&quot;Saleh Masoudi&quot;,&quot;serverLastName&quot;:&quot;&quot;,&quot;avatar&quot;:174712018415127,&quot;user_role&quot;:1}}</body></message>
```

## 2
```xml
<message xmlns='jabber:client' xml:lang='notify_video' to='990000003834@inews.local.boshrapardaz.ir' from='990000003929@inews.local.boshrapardaz.ir' type='headline' id='rqv0O3'><boshra message_type='0'/><body>{&quot;user_server_id&quot;:&quot;990000003929&quot;,&quot;roomName&quot;:&quot;2025_5_22_f611ea33-3d1d-4e39-a04e-88aa35ef3d14&quot;,&quot;resource&quot;:&quot;AndroidI24271747812248274&quot;,&quot;uuid&quot;:&quot;493F07B4-31F6-4B72-B96D-0663E83B4FD0&quot;,&quot;roomMember&quot;:{&quot;server_id&quot;:&quot;990000003929&quot;,&quot;serverName&quot;:&quot;Mmm&quot;,&quot;serverLastName&quot;:&quot;&quot;,&quot;avatar&quot;:0,&quot;user_role&quot;:4},&quot;member&quot;:{&quot;server_id&quot;:&quot;990000003929&quot;,&quot;serverName&quot;:&quot;Mmm&quot;,&quot;serverLastName&quot;:&quot;&quot;,&quot;avatar&quot;:0,&quot;user_role&quot;:4}}</body></message>
```

## 3
```xml
<message xmlns='jabber:client' xml:lang='notify_video' to='990000003834@inews.local.boshrapardaz.ir' from='990000003929@inews.local.boshrapardaz.ir' type='headline' id='Qd4Nwf'><boshra message_type='0'/><body>{&quot;user_server_id&quot;:&quot;990000003929&quot;,&quot;roomName&quot;:&quot;2025_5_22_f611ea33-3d1d-4e39-a04e-88aa35ef3d14&quot;,&quot;resource&quot;:&quot;AndroidI24271747812248274&quot;,&quot;uuid&quot;:&quot;8E5801B5-BDB1-4B87-97D1-5D45B9CD9C83&quot;,&quot;roomMember&quot;:{&quot;server_id&quot;:&quot;990000003929&quot;,&quot;serverName&quot;:&quot;Mmm&quot;,&quot;serverLastName&quot;:&quot;&quot;,&quot;avatar&quot;:0,&quot;user_role&quot;:4},&quot;member&quot;:{&quot;server_id&quot;:&quot;990000003929&quot;,&quot;serverName&quot;:&quot;Mmm&quot;,&quot;serverLastName&quot;:&quot;&quot;,&quot;avatar&quot;:0,&quot;user_role&quot;:4}}</body></message>
```

## 4
```xml
<message xmlns='jabber:client' xml:lang='notify_video' to='990000003834@inews.local.boshrapardaz.ir' from='990000003929@inews.local.boshrapardaz.ir' type='headline' id='Ri9U3d'><boshra message_type='0'/><body>{&quot;user_server_id&quot;:&quot;990000003929&quot;,&quot;roomName&quot;:&quot;2025_5_22_f611ea33-3d1d-4e39-a04e-88aa35ef3d14&quot;,&quot;resource&quot;:&quot;AndroidI24271747812248274&quot;,&quot;uuid&quot;:&quot;BFDB90F1-E7D7-4F63-A6C2-8DBE4732E68D&quot;,&quot;roomMember&quot;:{&quot;server_id&quot;:&quot;990000003929&quot;,&quot;serverName&quot;:&quot;Mmm&quot;,&quot;serverLastName&quot;:&quot;&quot;,&quot;avatar&quot;:0,&quot;user_role&quot;:3},&quot;member&quot;:{&quot;server_id&quot;:&quot;990000003929&quot;,&quot;serverName&quot;:&quot;Mmm&quot;,&quot;serverLastName&quot;:&quot;&quot;,&quot;avatar&quot;:0,&quot;user_role&quot;:3}}</body></message>
```

## 5
```xml
<message xmlns='jabber:client' xml:lang='notify_video' to='990000003834@inews.local.boshrapardaz.ir' from='990000003929@inews.local.boshrapardaz.ir' type='headline' id='O9wu7V'><boshra message_type='0'/><body>{&quot;user_server_id&quot;:&quot;990000003929&quot;,&quot;roomName&quot;:&quot;2025_5_22_f611ea33-3d1d-4e39-a04e-88aa35ef3d14&quot;,&quot;resource&quot;:&quot;AndroidI24271747812248274&quot;,&quot;uuid&quot;:&quot;B2234027-55A1-429F-9B6D-629EABD6207F&quot;,&quot;roomMember&quot;:{&quot;server_id&quot;:&quot;990000003929&quot;,&quot;serverName&quot;:&quot;Mmm&quot;,&quot;serverLastName&quot;:&quot;&quot;,&quot;avatar&quot;:0,&quot;user_role&quot;:2},&quot;member&quot;:{&quot;server_id&quot;:&quot;990000003929&quot;,&quot;serverName&quot;:&quot;Mmm&quot;,&quot;serverLastName&quot;:&quot;&quot;,&quot;avatar&quot;:0,&quot;user_role&quot;:2}}</body></message> 
```

