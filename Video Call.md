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


```log
09:41:35.310 VideoCa...Number  I  members not in room updated: member: HoldContact{id=0, server_id=990000003929, serverId=0, phone=0, avatar=0, firstName='null', lastName='null', serverName='Mmm', serverLastName='', username='null', mutual=0, infoupdated=0, deleted=0, last=null, userRole=4, countMedia=-1, blocked_by_user=false, mobile='null', deleted_dash=0, hasIntra=false, photo=null, otherPhones=null}, membersNotInRoom: [HoldContact{id=0, server_id=990000003929, serverId=0, phone=0, avatar=0, firstName='null', lastName='null', serverName='Mmm', serverLastName='', username='null', mutual=0, infoupdated=0, deleted=0, last=null, userRole=4, countMedia=-1, blocked_by_user=false, mobile='null', deleted_dash=0, hasIntra=false, photo=null, otherPhones=null}, HoldContact{id=0, server_id=990000003929, serverId=0, phone=0, avatar=0, firstName='null', lastName='null', serverName='Mmm', serverLastName='', username='null', mutual=0, infoupdated=0, deleted=0, last=null, userRole=4, countMedia=-1, blocked_by_user=false, mobile='null', deleted_dash=0, hasIntra=false, photo=null, otherPhones=null}, HoldContact{id=3929, server_id=990000003929, serverId=3929, phone=0, avatar=0, firstName='null', lastName='null', serverName='Mmm', serverLastName='', username='null', mutual=0, infoupdated=0, deleted=0, last=null, userRole=4, countMedia=-1, blocked_by_user=false, mobile='null', deleted_dash=0, hasIntra=false, photo=null, otherPhones=null}]
```



```java
DialogsConfigsServers defaultServer = new DialogsConfigsServers(context);  
if (defaultServer.checkIsDefaultServer(DialogsConfigsServers.ServerFlavor.Intragram)) {  
    Map<DialogsConfigsServers.ServerAttr, String> serverDefaultConfig = defaultServer.setDefaultConfigLocal();  
    XmppAndClientConfig.resetConfigs(serverDefaultConfig.get(DialogsConfigsServers.ServerAttr.apacheServer), serverDefaultConfig.get(DialogsConfigsServers.ServerAttr.ejabberdServer), serverDefaultConfig.get(DialogsConfigsServers.ServerAttr.ejabberdDomain),  
            Integer.parseInt(Objects.requireNonNull(serverDefaultConfig.get(DialogsConfigsServers.ServerAttr.ejabberdPort))), serverDefaultConfig.get(DialogsConfigsServers.ServerAttr.voipServer), serverDefaultConfig.get(DialogsConfigsServers.ServerAttr.voipPort), false);  
}
```

```java
/*  
 * Copyright (c) 2019. * Hosein Kordabadi * Mail: hoseinkord3343@gmail.com * 989138553343 */  
package org.boshra.ui.dialogs;  
  
import static org.telegram.messenger.ApplicationLoader.getConfigHost;  
  
import android.app.AlertDialog;  
import android.app.Dialog;  
import android.content.Context;  
import android.os.Bundle;  
import android.view.View;  
import android.view.ViewGroup;  
import android.view.Window;  
import android.view.WindowManager;  
import android.widget.Button;  
import android.widget.CompoundButton;  
import android.widget.EditText;  
import android.widget.ImageButton;  
import android.widget.Switch;  
import android.widget.Toast;  
  
import androidx.annotation.NonNull;  
  
import org.boshra.messenger.Pref;  
import org.boshra.messenger.api.XmppAndClientConfig;  
import org.boshra.messenger.helper.HelperService;  
import org.telegram.messenger.AndroidUtilities;  
import org.telegram.messenger.LocaleController;  
import org.telegram.messenger.R;  
  
import java.util.Map;  
import java.util.Objects;  
  
/**  
 * Created by MhkDeveloper on 18/05/2017. */  
public class DialogsConfigsServers extends Dialog {  
    private final Map<ServerFlavor, String> serverFlavors = new java.util.HashMap<>();  
  
    Switch switch_config;  
    Switch switch_celf;  
    ViewGroup layoutContent;  
    EditText edtEjabberdDomain;  
    EditText edtEjabberdServer;  
    EditText edtEjabberdPort;  
    //    EditText edtVideoCallServer;  
//    EditText edtVideoCallPort;  
    EditText edtApache;  
    EditText edtVoipServer;  
    EditText edtVoipPort;  
    //    EditText edtVoipTransport;  
    Button btnSave;  
    ImageButton btnDefaultConfigs;  
  
    public DialogsConfigsServers(@NonNull Context context) {  
        super(context);  
        setServerFlavors();  
    }  
  
    private void setServerFlavors() {  
        serverFlavors.put(ServerFlavor.Intragram, "inews.local.boshrapardaz.ir");  
        serverFlavors.put(ServerFlavor.Idogram, "idogram.a.dejon.co");  
        serverFlavors.put(ServerFlavor.Matin, "matin.sarvmmp.ir");  
    }  
  
  
    public enum ServerFlavor {  
        Intragram,  
        Idogram,  
        Matin  
    }  
  
    public enum ServerAttr {  
        ejabberdDomain,  
        ejabberdServer,  
        ejabberdPort,  
        apacheServer,  
        voipServer,  
        voipPort  
    }  
  
  
    public Map<ServerAttr, String> setDefaultConfigLocal() {  
        Map<ServerAttr, String> serverDefaultConfig = new java.util.HashMap<>();  
        serverDefaultConfig.put(ServerAttr.ejabberdDomain, "inews.local.boshrapardaz.ir");  
        serverDefaultConfig.put(ServerAttr.ejabberdServer, "inews.local.boshrapardaz.ir");  
        serverDefaultConfig.put(ServerAttr.ejabberdPort, "5222");  
        serverDefaultConfig.put(ServerAttr.apacheServer, "inews.local.boshrapardaz.ir");  
        serverDefaultConfig.put(ServerAttr.voipServer, "inews.local.boshrapardaz.ir");  
        serverDefaultConfig.put(ServerAttr.voipPort, "5060");  
        return serverDefaultConfig;  
    }  
  
    private Map<ServerAttr, String> setDefaultConfigGlobal() {  
        Map<ServerAttr, String> serverDefaultConfig = new java.util.HashMap<>();  
        serverDefaultConfig.put(ServerAttr.ejabberdDomain, "inews.local.boshrapardaz.ir");  
        serverDefaultConfig.put(ServerAttr.ejabberdServer, "ejabberd.boshrastar.ir");  
        serverDefaultConfig.put(ServerAttr.ejabberdPort, "5222");  
        serverDefaultConfig.put(ServerAttr.apacheServer, "idogram.boshrastar.ir");  
        serverDefaultConfig.put(ServerAttr.voipServer, "idogram.boshrastar.ir");  
        serverDefaultConfig.put(ServerAttr.voipPort, "5060");  
        return serverDefaultConfig;  
    }  
  
    @Override  
    protected void onCreate(Bundle savedInstanceState) {  
        super.onCreate(savedInstanceState);  
        setContentView(R.layout.dialog_config_servers);  
  
        Pref.setBackgroundOrForegroundServiceRefresh(false);  
  
        WindowManager.LayoutParams params = new WindowManager.LayoutParams();  
        Window window = getWindow();  
  
        params.copyFrom(window.getAttributes());  
        params.width = WindowManager.LayoutParams.MATCH_PARENT;  
        params.height = WindowManager.LayoutParams.WRAP_CONTENT;  
        window.setAttributes(params);  
  
        findViews();  
  
//        boolean configmode = Pref.getConfigMode();  
//        switch_config.setChecked(configmode);  
        setVisible(true);  
        switch_config.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {  
            @Override  
            public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {  
                if (Pref.getBackgroundOrForegroundServiceRefresh()) {  
                    Toast.makeText(getContext(), LocaleController.getString("PleaseWait", R.string.PleaseWait), Toast.LENGTH_SHORT).show();  
                    switch_config.setChecked(!isChecked);  
                    return;  
                }  
                setVisible(isChecked);  
                Pref.changeConfigMode(isChecked);  
                restartService();  
            }  
        });  
  
        edtEjabberdServer.setText(Pref.getEjabberdServer());  
        edtEjabberdDomain.setText(Pref.getEjabberdDomain());  
        edtEjabberdPort.setText(String.valueOf(Pref.getEjabberdPort()));  
        edtApache.setText(Pref.getApacheServer());  
        edtVoipServer.setText(Pref.getVoipServer());  
        edtVoipPort.setText(Pref.getVoipPort());  
//        edtVoipTransport.setText(Pref.getVoipTransport());  
//        edtVideoCallServer.setText(Pref.getVideoCallServerUrl());  
//        edtVideoCallPort.setText(String.valueOf(Pref.getVideoCallPort()));  
        switch_celf.setChecked(Pref.getSelfSigned());  
        if (checkIsDefaultServer(ServerFlavor.Intragram)) {  
            btnDefaultConfigs.setVisibility(View.VISIBLE);  
        } else {  
            btnDefaultConfigs.setVisibility(View.GONE);  
        }  
        btnSave.setOnClickListener(v -> {  
            if (Pref.getBackgroundOrForegroundServiceRefresh()) {  
                Toast.makeText(getContext(), LocaleController.getString("PleaseWait", R.string.PleaseWait), Toast.LENGTH_SHORT).show();  
                return;  
            }  
            if (!valid()) {  
                return;  
            }  
  
            String apache = edtApache.getText().toString();  
            String ejUrl = edtEjabberdServer.getText().toString();  
            String ejXmppDomain = edtEjabberdDomain.getText().toString();  
            int ejPort = Integer.valueOf(edtEjabberdPort.getText().toString());  
            String voipServer = edtVoipServer.getText().toString();  
            String voipPort = edtVoipPort.getText().toString();  
//                String voipTransport = edtVoipTransport.getText().toString();  
  
            XmppAndClientConfig.resetConfigs(apache, ejUrl, ejXmppDomain,  
                    ejPort, voipServer, voipPort, switch_celf.isChecked()  
  
            );  
  
            btnSave.setClickable(false);  
//                restartService();  
            dismiss();  
        });  
        btnDefaultConfigs.setOnClickListener(v -> {  
            CharSequence[] items = new CharSequence[]{  
                    "Default Local Server",  
                    "Default Global Server"  
            };  
            new AlertDialog.Builder(getContext())  
                    .setSingleChoiceItems(items, 0, null)  
                    .setPositiveButton("Set", (dialog, whichButton) -> {  
                        dialog.dismiss();  
                        int selectedPosition = ((AlertDialog) dialog).getListView().getCheckedItemPosition();  
                        Map<ServerAttr, String> serverDefaultConfig;  
                        if (selectedPosition == 0) {  
                            serverDefaultConfig = setDefaultConfigLocal();  
                        } else {  
                            serverDefaultConfig = setDefaultConfigGlobal();  
                        }  
                        XmppAndClientConfig.resetConfigs(serverDefaultConfig.get(ServerAttr.apacheServer), serverDefaultConfig.get(ServerAttr.ejabberdServer), serverDefaultConfig.get(ServerAttr.ejabberdDomain),  
                                Integer.parseInt(Objects.requireNonNull(serverDefaultConfig.get(ServerAttr.ejabberdPort))), serverDefaultConfig.get(ServerAttr.voipServer), serverDefaultConfig.get(ServerAttr.voipPort), switch_celf.isChecked()  
  
                        );  
  
                        btnSave.setClickable(false);  
//                restartService();  
                        dismiss();  
  
                        Toast.makeText(getContext(), "default server config set", Toast.LENGTH_SHORT).show();  
                    })  
                    .show();  
        });  
    }  
  
    public boolean checkIsDefaultServer(ServerFlavor defaultServer) {  
        return getConfigHost().equals(serverFlavors.get(defaultServer));  
    }  
  
    private boolean valid() {  
        boolean valid = true;  
  
        if (isEmptyEditText(edtApache)) {  
            valid = false;  
            edtApache.setError("Apache Address not valid");  
        }  
        if (isEmptyEditText(edtEjabberdServer)) {  
            valid = false;  
            edtEjabberdServer.setError("Ejabberd server not valid");  
        }  
        if (isEmptyEditText(edtEjabberdDomain)) {  
            valid = false;  
            edtEjabberdDomain.setError("Ejabberd domain not valid");  
        }  
        if (isEmptyEditText(edtVoipServer)) {  
            valid = false;  
            edtVoipServer.setError("Voip server not valid");  
        }  
        if (isEmptyEditText(edtVoipPort)) {  
            valid = false;  
            edtVoipPort.setError("Voip port not valid");  
        }  
//        if (isEmptyEditText(edtVideoCallServer)) {  
//            valid = false;  
//            edtVideoCallServer.setError("Video call server!!");  
//        }  
//        if (isEmptyEditText(edtVideoCallPort)) {  
//            valid = false;  
//            edtVideoCallPort.setError("Video call port not valid");  
//        }  
//        if (isEmptyEditText(edtVoipTransport)) {  
//            valid = false;  
//            edtVoipTransport.setError("Voip transport not valid");  
//        }  
        if (isEmptyEditText(edtEjabberdPort)) {  
            valid = false;  
            edtEjabberdPort.setError("Ejabberd port not valid");  
        }  
        return valid;  
    }  
  
    private boolean isEmptyEditText(EditText edtUsername) {  
        if (edtUsername == null || edtUsername.getText().toString().length() <= 1) {  
            return true;  
        }  
        return false;  
    }  
  
    Runnable runnable;  
  
  
    private void restartService() {  
        Pref.setBackgroundOrForegroundServiceRefresh(true);  
        if (runnable != null) {  
            AndroidUtilities.cancelRunOnUIThread(runnable);  
  
  
        }  
        runnable = new Runnable() {  
            @Override  
            public void run() {  
                HelperService.startSeviceXmpp();  
                Pref.setBackgroundOrForegroundServiceRefresh(false);  
            }  
        };  
  
        HelperService.stopSeviceXmpp();  
        AndroidUtilities.runOnUIThread(runnable, 5000);  
    }  
  
    private void setVisible(boolean configmode) {  
        if (configmode) {  
            layoutContent.setVisibility(View.VISIBLE);  
        } else {  
            layoutContent.setVisibility(View.GONE);  
  
        }  
    }  
  
    private void findViews() {  
        switch_config = findViewById(R.id.switch_config);  
        switch_celf = findViewById(R.id.switch_celf);  
        layoutContent = findViewById(R.id.layoutContent);  
//        edtVideoCallServer = findViewById(R.id.edtVideoCallServer);  
//        edtVideoCallPort = findViewById(R.id.edtVideoCallPort);  
        edtEjabberdDomain = findViewById(R.id.edtEjabberdDomain);  
        edtEjabberdServer = findViewById(R.id.edtEjabberdServer);  
        edtEjabberdPort = findViewById(R.id.edt_ej_port);  
        edtApache = findViewById(R.id.edtApache);  
        edtVoipServer = findViewById(R.id.edtVoipServer);  
        edtVoipPort = findViewById(R.id.edtVoipPort);  
//        edtVoipTransport = findViewById(R.id.edtVoipTransport);  
        btnSave = findViewById(R.id.btnSave);  
        btnDefaultConfigs = findViewById(R.id.use_default_configs_btn);  
    }  
}
```


```xml
<?xml version="1.0" encoding="utf-8"?>  
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"  
    android:layout_width="match_parent"  
    android:layout_height="match_parent"  
    android:layout_margin="8dp"  
    android:gravity="left"  
    android:orientation="vertical">  
  
    <androidx.core.widget.NestedScrollView  
        android:layout_width="match_parent"  
        android:layout_height="wrap_content">  
  
        <LinearLayout  
            android:layout_width="match_parent"  
            android:layout_height="wrap_content"  
            android:orientation="vertical">  
  
            <LinearLayout  
                android:layout_width="match_parent"  
                android:layout_height="wrap_content"  
                android:layout_gravity="center"  
                android:gravity="center"  
                android:layout_marginHorizontal="8dp"  
                android:layout_marginVertical="16dp"  
                android:orientation="horizontal">  
  
                <TextView  
                    android:id="@+id/config_server_title"  
                    android:layout_width="0dp"  
                    android:layout_height="wrap_content"  
                    android:text="Config Servers"  
                    android:textColor="@android:color/black"  
                    android:layout_weight="1"  
                    android:textSize="24sp" />  
  
                <ImageButton  
                    android:id="@+id/use_default_configs_btn"  
                    android:layout_width="wrap_content"  
                    android:layout_height="wrap_content"  
                    android:layout_marginLeft="8dp"  
                    android:background="@null"  
                    android:tint="#000000"  
                    android:src="@drawable/msg_photo_settings" />  
  
            </LinearLayout>  
  
            <Switch  
                android:id="@+id/switch_config"  
                android:layout_width="match_parent"  
                android:layout_height="wrap_content"  
                android:gravity="left"  
                android:padding="14dp"  
                android:text="Enable Config"  
                android:textColor="@color/black"  
  
                android:visibility="gone" />  
  
            <LinearLayout  
                android:id="@+id/layoutContent"  
                android:layout_width="match_parent"  
                android:layout_height="wrap_content"  
                android:orientation="vertical">  
  
                <TextView  
                    android:layout_width="match_parent"  
                    android:layout_height="wrap_content"  
                    android:gravity="left"  
                    android:paddingLeft="14dp"  
                    android:text="Xmpp Domain (idogram.domain.name)"  
  
                    android:textColor="@color/gray_font"  
                    android:textSize="12dp" />  
  
                <EditText  
                    android:id="@+id/edtEjabberdDomain"  
                    android:layout_width="match_parent"  
                    android:layout_height="wrap_content"  
                    android:layout_margin="8dp"  
                    android:background="@drawable/edittext_register"  
                    android:gravity="left"  
                    android:hint="Domain"  
                    android:imeOptions="actionNext"  
                    android:nextFocusDown="@+id/edtEjabberdServer"  
                    android:singleLine="true"  
  
  
                    android:text="" />  
  
                <TextView  
                    android:layout_width="match_parent"  
                    android:layout_height="wrap_content"  
                    android:gravity="left"  
                    android:paddingLeft="14dp"  
                    android:text="Ejabberd Server (idogram.domain.name)"  
                    android:textColor="@color/gray_font"  
                    android:textSize="12dp" />  
  
                <EditText  
                    android:id="@+id/edtEjabberdServer"  
                    android:layout_width="match_parent"  
                    android:layout_height="wrap_content"  
                    android:layout_margin="8dp"  
                    android:background="@drawable/edittext_register"  
                    android:gravity="left"  
                    android:hint="Ejabberd Server"  
                    android:imeOptions="actionNext"  
                    android:nextFocusDown="@+id/edt_ej_port"  
                    android:singleLine="true"  
                    android:text="" />  
  
                <TextView  
                    android:layout_width="match_parent"  
                    android:layout_height="wrap_content"  
                    android:gravity="left"  
                    android:paddingLeft="14dp"  
                    android:text="Ejabberd Port (5222)"  
                    android:textColor="@color/gray_font"  
                    android:textSize="12dp" />  
  
                <EditText  
                    android:id="@+id/edt_ej_port"  
                    android:layout_width="match_parent"  
                    android:layout_height="wrap_content"  
                    android:layout_margin="8dp"  
                    android:background="@drawable/edittext_register"  
                    android:gravity="left"  
                    android:hint="Ejabberd port"  
                    android:imeOptions="actionNext"  
                    android:inputType="number"  
                    android:nextFocusDown="@+id/edtApache"  
                    android:singleLine="true"  
                    android:text="" />  
  
                <TextView  
                    android:layout_width="match_parent"  
                    android:layout_height="wrap_content"  
                    android:gravity="left"  
                    android:paddingLeft="14dp"  
                    android:text="Apache Server (idogram.domain.name)"  
                    android:textColor="@color/gray_font"  
                    android:textSize="12dp" />  
  
                <EditText  
                    android:id="@+id/edtApache"  
                    android:layout_width="match_parent"  
                    android:layout_height="wrap_content"  
                    android:layout_margin="8dp"  
                    android:background="@drawable/edittext_register"  
                    android:gravity="left"  
                    android:hint="Apache..."  
                    android:imeOptions="actionNext"  
                    android:nextFocusDown="@+id/edtVoipServer"  
                    android:singleLine="true"  
  
                    android:text="" />  
  
                <TextView  
                    android:layout_width="match_parent"  
                    android:layout_height="wrap_content"  
                    android:gravity="left"  
                    android:paddingLeft="14dp"  
                    android:text="Voip Server (idogram.domain.name)"  
                    android:textColor="@color/gray_font"  
                    android:textSize="12dp" />  
  
                <EditText  
                    android:id="@+id/edtVoipServer"  
                    android:layout_width="match_parent"  
                    android:layout_height="wrap_content"  
                    android:layout_margin="8dp"  
                    android:background="@drawable/edittext_register"  
                    android:gravity="left"  
  
                    android:hint="Voip Server"  
                    android:imeOptions="actionNext"  
                    android:nextFocusDown="@+id/edtVoipPort"  
  
                    android:singleLine="true"  
                    android:text="" />  
  
                <TextView  
                    android:layout_width="match_parent"  
                    android:layout_height="wrap_content"  
                    android:gravity="left"  
                    android:paddingLeft="14dp"  
                    android:text="Voip Port (5060)"  
                    android:textColor="@color/gray_font"  
                    android:textSize="12dp" />  
  
                <EditText  
                    android:id="@+id/edtVoipPort"  
                    android:layout_width="match_parent"  
                    android:layout_height="wrap_content"  
                    android:layout_margin="8dp"  
                    android:background="@drawable/edittext_register"  
                    android:gravity="left"  
                    android:hint="Voip Port"  
                    android:imeOptions="actionNext"  
                    android:nextFocusDown="@+id/edtVoipTransport"  
                    android:singleLine="true"  
                    android:text="" />  
  
                <TextView  
                    android:layout_width="match_parent"  
                    android:layout_height="wrap_content"  
                    android:gravity="left"  
                    android:paddingLeft="14dp"  
                    android:visibility="gone"  
  
                    android:text="Voip Transport (TCP|UDP)"  
                    android:textColor="@color/gray_font"  
                    android:textSize="12dp" />  
  
                <EditText  
                    android:id="@+id/edtVoipTransport"  
                    android:layout_width="match_parent"  
                    android:layout_height="wrap_content"  
                    android:layout_margin="8dp"  
                    android:background="@drawable/edittext_register"  
                    android:gravity="left"  
                    android:visibility="gone"  
                    android:hint="Voip Transport"  
                    android:imeOptions="actionNext"  
                    android:nextFocusDown="@+id/edtVideoCallServer"  
                    android:singleLine="true"  
                    android:text="" />  
  
<!--                <TextView-->  
<!--                    android:layout_width="match_parent"-->  
<!--                    android:layout_height="wrap_content"-->  
<!--                    android:gravity="left"-->  
<!--                    android:paddingLeft="14dp"-->  
<!--                    android:text="VideoCall Url (idogram.domain.name)"-->  
<!--                    android:textColor="@color/gray_font"-->  
<!--                    android:textSize="12dp" />-->  
<!--                <EditText-->  
<!--                    android:id="@+id/edtVideoCallServer"-->  
<!--                    android:layout_width="match_parent"-->  
<!--                    android:layout_height="wrap_content"-->  
<!--                    android:layout_margin="8dp"-->  
<!--                    android:background="@drawable/edittext_register"-->  
<!--                    android:gravity="left"-->  
<!--                    android:hint="Video call server"-->  
<!--                    android:imeOptions="actionNext"-->  
<!--                    android:nextFocusDown="@+id/edtVideoCallPort"-->  
<!--                    android:singleLine="true"-->  
<!--                    android:text="" />-->  
  
<!--                <TextView-->  
<!--                    android:layout_width="match_parent"-->  
<!--                    android:layout_height="wrap_content"-->  
<!--                    android:gravity="left"-->  
<!--                    android:paddingLeft="14dp"-->  
<!--                    android:text="VideoCall Port (443)"-->  
<!--                    android:textColor="@color/gray_font"-->  
<!--                    android:textSize="12dp" />-->  
  
<!--                <EditText-->  
<!--                    android:id="@+id/edtVideoCallPort"-->  
<!--                    android:layout_width="match_parent"-->  
<!--                    android:layout_height="wrap_content"-->  
<!--                    android:layout_margin="8dp"-->  
<!--                    android:background="@drawable/edittext_register"-->  
<!--                    android:gravity="left"-->  
<!--                    android:hint="Ejabberd port"-->  
<!--                    android:imeOptions="actionDone"-->  
<!--                    android:inputType="number"-->  
  
<!--                    android:singleLine="true"-->  
<!--                    android:text="" />-->  
  
                <Switch  
                    android:id="@+id/switch_celf"  
                    android:layout_width="match_parent"  
                    android:layout_height="wrap_content"  
                    android:gravity="left"  
                    android:padding="14dp"  
                    android:text="Self-signed"  
                    android:textColor="@color/gray_font"  
                    android:visibility="visible" />  
  
                <TextView  
                    android:id="@+id/txtWaiting"  
                    android:layout_width="match_parent"  
                    android:layout_height="wrap_content"  
                    android:gravity="left"  
                    android:text="Please Wait...."  
                    android:textColor="@color/colorPrimaryDark"  
                    android:textSize="18dp"  
                    android:visibility="gone" />  
  
                <Button  
                    android:id="@+id/btnSave"  
                    android:layout_width="match_parent"  
                    android:layout_height="wrap_content"  
                    android:layout_margin="8dp"  
  
                    android:background="@drawable/button_green_light"  
                    android:text="Save"  
                    android:textColor="@color/white" />  
  
            </LinearLayout>  
  
        </LinearLayout>  
    </androidx.core.widget.NestedScrollView>  
</LinearLayout>
```