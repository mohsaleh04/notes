```log
=============== VoIPService STOPPING ===============
Davey! duration=9223367461417ms; Flags=2, FrameTimelineVsyncId=80212, IntendedVsync=4575437158286, Vsync=4575437158286, InputEventId=0, HandleInputStart=4575437158286, AnimationStart=4575437158286, PerformTraversalsStart=4575437158286, DrawStart=4575437158286, FrameDeadline=4575453824952, FrameInterval=0, FrameStartTime=16666666, SyncQueued=4575442071377, SyncStart=4575442073669, IssueDrawCommandsStart=4575442267836, SwapBuffers=4575449129346, FrameCompleted=9223372036854775807, DequeueBufferDuration=3326146, QueueBufferDuration=946718, GpuCompleted=9223372036854775807, SwapBuffersCompleted=4575450797159, DisplayPresentTime=0, 
--> POST https://inews.local.boshrapardaz.ir/apiservice/public/api/v1/video-call/state-notify http/1.1
Content-Type: application/json; charset=utf-8
Content-Length: 147
Authorization: Bearer 1407547|QrXc14jgYQsSR6frOAE9Q3m9ZBc4HH6xST5dhTAj
Clversion: 4.4.1
Accept: application/json
resource: AndroidI24271747812248274
client_type: android
Appname: Intragram
Cltype: android
info: {"CL":"android","VN":"4.4.1","VC":"109","ip":"192.168.20.176","device":"CAP_sprout","brand":"Nokia","SDK":31,"op":"Undefined","IMEI":"secId-2b023a0b4b7e3253"}
{"room":"2025_5_25_a9e38c0b-8f74-4dd9-9a58-8ec71ec7e259","chat_id":990000003834,"user_role":2,"sipPhone":"","resource":"AndroidI24271747812248274"}
--> END POST (147-byte body)
179 MasterKeyGetToken: 
<-- 200 OK https://inews.local.boshrapardaz.ir/apiservice/public/api/v1/call/send (28ms)
Server: nginx/1.21.3
Date: Sun, 25 May 2025 08:36:47 GMT
Content-Type: application/json
Connection: keep-alive
Status: 200
Cache-Control: private, must-revalidate
Pragma: no-cache
Expires: -1
X-Ratelimit-Limit: 50000
X-Ratelimit-Remaining: 49996
Vary: Origin
{"data":1,"error":0,"message":null}
<-- END HTTP (35-byte body)
2633 :call info inserted 
<-- 400 Bad Request https://inews.local.boshrapardaz.ir/apiservice/public/api/v1/video-call/state-notify (30ms)
Server: nginx/1.21.3
Date: Sun, 25 May 2025 08:36:47 GMT
Content-Type: application/json
Connection: keep-alive
Status: 400
Cache-Control: private, must-revalidate
Pragma: no-cache
Expires: -1
X-Ratelimit-Limit: 50000
X-Ratelimit-Remaining: 49995
Vary: Origin
{"data":null,"error":1,"message":"NOT_VALID_USER_ROLE"}
<-- END HTTP (55-byte body)
Davey! duration=9223367461467ms; Flags=0, FrameTimelineVsyncId=80204, IntendedVsync=4575387086350, Vsync=4575470419680, InputEventId=0, HandleInputStart=4575482278409, AnimationStart=4575482284555, PerformTraversalsStart=4575509348721, DrawStart=4575528753773, FrameDeadline=4575420419682, FrameInterval=4575482256377, FrameStartTime=16666666, SyncQueued=4575529755596, SyncStart=4575529848773, IssueDrawCommandsStart=4575530413096, SwapBuffers=4575533028304, FrameCompleted=9223372036854775807, DequeueBufferDuration=30000, QueueBufferDuration=691823, GpuCompleted=9223372036854775807, SwapBuffersCompleted=4575535076534, DisplayPresentTime=563027262832640, 
Davey! duration=9223367461300ms; Flags=2, FrameTimelineVsyncId=80229, IntendedVsync=4575554075919, Vsync=4575554075919, InputEventId=0, HandleInputStart=4575554075919, AnimationStart=4575554075919, PerformTraversalsStart=4575554075919, DrawStart=4575554075919, FrameDeadline=4575587409251, FrameInterval=0, FrameStartTime=16666666, SyncQueued=4575558488409, SyncStart=4575558490804, IssueDrawCommandsStart=4575558682054, SwapBuffers=4575567066273, FrameCompleted=9223372036854775807, DequeueBufferDuration=29167, QueueBufferDuration=1174896, GpuCompleted=9223372036854775807, SwapBuffersCompleted=4575569101846, DisplayPresentTime=5118136497408573440, 
call state notify -> error (onFailure): code:400, msg: {"data":null,"error":1,"message":"NOT_VALID_USER_ROLE"}
346 onError: CodeError 400
```
