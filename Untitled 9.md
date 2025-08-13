```log
2025-08-12 20:51:15.721 31419-31419 XmppConnec...ine Number org.telegram.messenger.regular       I  122PingPeri startKeepAliveTimer: 
2025-08-12 20:51:15.722 31419-31419 XmppConnec...ine Number org.telegram.messenger.regular       I  227 onStartCommand: 
2025-08-12 20:51:15.722 31419-31419 XmppConnec...ine Number org.telegram.messenger.regular       I  230 onStartCommand : versionStartForeGround
2025-08-12 20:51:15.724 31419-31511 Compatibil...geReporter org.telegram.messenger.regular       D  Compat change id reported: 351566728; UID 10727; state: DISABLED
2025-08-12 20:51:15.725 31419-31511 XmppConnec...ine Number org.telegram.messenger.regular       I  128PingPeri sendPing: 
2025-08-12 20:51:15.729 31419-31419 AndroidRuntime          org.telegram.messenger.regular       D  Shutting down VM
2025-08-12 20:51:15.734 31419-31419 AndroidRuntime          org.telegram.messenger.regular       E  FATAL EXCEPTION: main (Ask Gemini)
                                                                                                    Process: org.telegram.messenger.regular, PID: 31419
                                                                                                    java.lang.RuntimeException: Unable to start service org.boshra.messenger.xmpp.XmppConnectionService@ff753fc with null: android.app.ForegroundServiceStartNotAllowedException: Service.startForeground() not allowed due to mAllowStartForeground false: service org.telegram.messenger.regular/org.boshra.messenger.xmpp.XmppConnectionService
                                                                                                    	at android.app.ActivityThread.handleServiceArgs(ActivityThread.java:5286)
                                                                                                    	at android.app.ActivityThread.-$$Nest$mhandleServiceArgs(Unknown Source:0)
                                                                                                    	at android.app.ActivityThread$H.handleMessage(ActivityThread.java:2531)
                                                                                                    	at android.os.Handler.dispatchMessage(Handler.java:106)
                                                                                                    	at android.os.Looper.loopOnce(Looper.java:230)
                                                                                                    	at android.os.Looper.loop(Looper.java:319)
                                                                                                    	at android.app.ActivityThread.main(ActivityThread.java:8919)
                                                                                                    	at java.lang.reflect.Method.invoke(Native Method)
                                                                                                    	at com.android.internal.os.RuntimeInit$MethodAndArgsCaller.run(RuntimeInit.java:578)
                                                                                                    	at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:1103)
                                                                                                    Caused by: android.app.ForegroundServiceStartNotAllowedException: Service.startForeground() not allowed due to mAllowStartForeground false: service org.telegram.messenger.regular/org.boshra.messenger.xmpp.XmppConnectionService
                                                                                                    	at android.app.ForegroundServiceStartNotAllowedException$1.createFromParcel(ForegroundServiceStartNotAllowedException.java:54)
                                                                                                    	at android.app.ForegroundServiceStartNotAllowedException$1.createFromParcel(ForegroundServiceStartNotAllowedException.java:50)
                                                                                                    	at android.os.Parcel.readParcelableInternal(Parcel.java:4904)
                                                                                                    	at android.os.Parcel.readParcelable(Parcel.java:4886)
                                                                                                    	at android.os.Parcel.createExceptionOrNull(Parcel.java:3086)
                                                                                                    	at android.os.Parcel.createException(Parcel.java:3075)
                                                                                                    	at android.os.Parcel.readException(Parcel.java:3058)
                                                                                                    	at android.os.Parcel.readException(Parcel.java:3000)
                                                                                                    	at android.app.IActivityManager$Stub$Proxy.setServiceForeground(IActivityManager.java:7234)
                                                                                                    	at android.app.Service.startForeground(Service.java:862)
                                                                                                    	at androidx.core.app.ServiceCompat$Api34Impl.startForeground(ServiceCompat.java:241)
                                                                                                    	at androidx.core.app.ServiceCompat.startForeground(ServiceCompat.java:172)
                                                                                                    	at org.boshra.messenger.xmpp.XmppConnectionService.onStartCommand(XmppConnectionService.java:232)
                                                                                                    	at android.app.ActivityThread.handleServiceArgs(ActivityThread.java:5268)
                                                                                                    	at android.app.ActivityThread.-$$Nest$mhandleServiceArgs(Unknown Source:0) 
                                                                                                    	at android.app.ActivityThread$H.handleMessage(ActivityThread.java:2531) 
                                                                                                    	at android.os.Handler.dispatchMessage(Handler.java:106) 
                                                                                                    	at android.os.Looper.loopOnce(Looper.java:230) 
                                                                                                    	at android.os.Looper.loop(Looper.java:319) 
                                                                                                    	at android.app.ActivityThread.main(ActivityThread.java:8919) 
                                                                                                    	at java.lang.reflect.Method.invoke(Native Method) 
                                                                                                    	at com.android.internal.os.RuntimeInit$MethodAndArgsCaller.run(RuntimeInit.java:578) 
                                                                                                    	at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:1103) 
2025-08-12 20:51:15.734 31419-31511 Xmpp: Line Number       org.telegram.messenger.regular       I  1473 chconn sendPing: cant send packet in send ping
2025-08-12 20:51:15.735 31419-31511 Xmpp Line Number        org.telegram.messenger.regular       I  1089 callConect() : called Connect
2025-08-12 20:51:15.736 31419-31511 TokenHelper Line Number org.telegram.messenger.regular       I  179 MasterKeyGetToken: 
2025-08-12 20:51:15.738 31419-31419 JitsiMeetSDK            org.telegram.messenger.regular       E  JitsiMeetUncaughtExceptionHandler FATAL ERROR (Ask Gemini)
                                                                                                    java.lang.RuntimeException: Unable to start service org.boshra.messenger.xmpp.XmppConnectionService@ff753fc with null: android.app.ForegroundServiceStartNotAllowedException: Service.startForeground() not allowed due to mAllowStartForeground false: service org.telegram.messenger.regular/org.boshra.messenger.xmpp.XmppConnectionService
                                                                                                    	at android.app.ActivityThread.handleServiceArgs(ActivityThread.java:5286)
                                                                                                    	at android.app.ActivityThread.-$$Nest$mhandleServiceArgs(Unknown Source:0)
                                                                                                    	at android.app.ActivityThread$H.handleMessage(ActivityThread.java:2531)
                                                                                                    	at android.os.Handler.dispatchMessage(Handler.java:106)
                                                                                                    	at android.os.Looper.loopOnce(Looper.java:230)
                                                                                                    	at android.os.Looper.loop(Looper.java:319)
                                                                                                    	at android.app.ActivityThread.main(ActivityThread.java:8919)
                                                                                                    	at java.lang.reflect.Method.invoke(Native Method)
                                                                                                    	at com.android.internal.os.RuntimeInit$MethodAndArgsCaller.run(RuntimeInit.java:578)
                                                                                                    	at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:1103)
                                                                                                    Caused by: android.app.ForegroundServiceStartNotAllowedException: Service.startForeground() not allowed due to mAllowStartForeground false: service org.telegram.messenger.regular/org.boshra.messenger.xmpp.XmppConnectionService
                                                                                                    	at android.app.ForegroundServiceStartNotAllowedException$1.createFromParcel(ForegroundServiceStartNotAllowedException.java:54)
                                                                                                    	at android.app.ForegroundServiceStartNotAllowedException$1.createFromParcel(ForegroundServiceStartNotAllowedException.java:50)
                                                                                                    	at android.os.Parcel.readParcelableInternal(Parcel.java:4904)
                                                                                                    	at android.os.Parcel.readParcelable(Parcel.java:4886)
                                                                                                    	at android.os.Parcel.createExceptionOrNull(Parcel.java:3086)
                                                                                                    	at android.os.Parcel.createException(Parcel.java:3075)
                                                                                                    	at android.os.Parcel.readException(Parcel.java:3058)
                                                                                                    	at android.os.Parcel.readException(Parcel.java:3000)
                                                                                                    	at android.app.IActivityManager$Stub$Proxy.setServiceForeground(IActivityManager.java:7234)
                                                                                                    	at android.app.Service.startForeground(Service.java:862)
                                                                                                    	at androidx.core.app.ServiceCompat$Api34Impl.startForeground(ServiceCompat.java:241)
                                                                                                    	at androidx.core.app.ServiceCompat.startForeground(ServiceCompat.java:172)
                                                                                                    	at org.boshra.messenger.xmpp.XmppConnectionService.onStartCommand(XmppConnectionService.java:232)
                                                                                                    	at android.app.ActivityThread.handleServiceArgs(ActivityThread.java:5268)
                                                                                                    	at android.app.ActivityThread.-$$Nest$mhandleServiceArgs(Unknown Source:0) 
                                                                                                    	at android.app.ActivityThread$H.handleMessage(ActivityThread.java:2531) 
                                                                                                    	at android.os.Handler.dispatchMessage(Handler.java:106) 
                                                                                                    	at android.os.Looper.loopOnce(Looper.java:230) 
                                                                                                    	at android.os.Looper.loop(Looper.java:319) 
                                                                                                    	at android.app.ActivityThread.main(ActivityThread.java:8919) 
                                                                                                    	at java.lang.reflect.Method.invoke(Native Method) 
                                                                                                    	at com.android.internal.os.RuntimeInit$MethodAndArgsCaller.run(RuntimeInit.java:578) 
                                                                                                    	at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:1103) 
                                                                                                    
                                                                                                    java.lang.RuntimeException: Unable to start service org.boshra.messenger.xmpp.XmppConnectionService@ff753fc with null: android.app.ForegroundServiceStartNotAllowedException: Service.startForeground() not allowed due to mAllowStartForeground false: service org.telegram.messenger.regular/org.boshra.messenger.xmpp.XmppConnectionService
                                                                                                    	at android.app.ActivityThread.handleServiceArgs(ActivityThread.java:5286)
                                                                                                    	at android.app.ActivityThread.-$$Nest$mhandleServiceArgs(Unknown Source:0)
                                                                                                    	at android.app.ActivityThread$H.handleMessage(ActivityThread.java:2531)
                                                                                                    	at android.os.Handler.dispatchMessage(Handler.java:106)
                                                                                                    	at android.os.Looper.loopOnce(Looper.java:230)
                                                                                                    	at android.os.Looper.loop(Looper.java:319)
                                                                                                    	at android.app.ActivityThread.main(ActivityThread.java:8919)
                                                                                                    	at java.lang.reflect.Method.invoke(Native Method)
                                                                                                    	at com.android.internal.os.RuntimeInit$MethodAndArgsCaller.run(RuntimeInit.java:578)
                                                                                                    	at com.android.internal.os.ZygoteInit.main(ZygoteInit.java:1103)
                                                                                                    Caused by: android.app.ForegroundServiceStartNotAllowedException: Service.startForeground() not allowed due to mAllowStartForeground false: service org.telegram.messenger.regular/org.boshra.messenger.xmpp.XmppConnectionService
                                                                                                    	at android.app.ForegroundServiceStartNotAllowedException$1.createFromParcel(ForegroundServiceStartNotAllowedException.java:54)
                                                                                                    	at android.app.ForegroundServiceStartNotAllowedException$1.createFromParcel(ForegroundServiceStartNotAllowedException.java:50)
                                                                                                    	at android.os.Parcel.readParcelableInternal(Parcel.java:4904)
                                                                                                    	at android.os.Parcel.readParcelable(Parcel.java:4886)
                                                                                                    	at android.os.Parcel.createExceptionOrNull(Parcel.java:3086)
2025-08-12 20:51:15.738 31419-31419 JitsiMeetSDK            org.telegram.messenger.regular       E  	at android.os.Parcel.createException(Parcel.java:3075) (Ask Gemini)
                                                                                                    	at android.os.Parcel.readException(Parcel.java:3058)
                                                                                                    	at android.os.Parcel.readException(Parcel.java:3000)
                                                                                                    	at android.app.IActivityManager$Stub$Proxy.setServiceForeground(IActivityManager.java:7234)
                                                                                                    	at android.app.Service.startForeground(Service.java:862)
                                                                                                    	at androidx.core.app.ServiceCompat$Api34Impl.startForeground(ServiceCompat.java:241)
                                                                                                    	at androidx.core.app.ServiceCompat.startForeground(ServiceCompat.java:172)
                                                                                                    	at org.boshra.messenger.xmpp.XmppConnectionService.onStartCommand(XmppConnectionService.java:232)
                                                                                                    	at android.app.ActivityThread.handleServiceArgs(ActivityThread.java:5268)
                                                                                                    	... 9 more
2025-08-12 20:51:15.774 31419-31419 Compatibil...geReporter org.telegram.messenger.regular       D  Compat change id reported: 263076149; UID 10727; state: ENABLED
2025-08-12 20:51:16.023 31419-31511 EngineFactory           org.telegram.messenger.regular       I  Provider GmsCore_OpenSSL not available
```