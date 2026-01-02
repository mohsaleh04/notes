ایده‌ی بسیار خوبی است. انتقال اطلاعات روی شبکه محلی (LAN) بدون نیاز به سرور واسط اینترنتی، هم سرعت بالایی دارد و هم از نظر امنیتی (Privacy) بسیار مطمئن‌تر است.

برای پیاده‌سازی این سناریو، بهترین معماری این است که **موبایل اندرویدی شما نقش "سرور" (Server)** را بازی کند و **اکستنشن کروم نقش "کلاینت" (Client)** را داشته باشد.

چرا؟ چون اجرای یک سرور HTTP/WebSocket روی اکستنشن‌های کروم (با توجه به محدودیت‌های Manifest V3) بسیار دشوار است، اما روی اندروید به راحتی قابل انجام است.

در اینجا نقشه راه (Roadmap) و نمونه کد برای پیاده‌سازی این سیستم را برایتان می‌نویسم:

---

### معماری کلی سیستم
1.  **Android App:**
    یک سرور WebSocket لوکال (مثلاً روی پورت 8080) اجرا می‌کند.
2.  **Chrome Extension:**
    به IP لوکال گوشی (مثلاً `192.168.1.5:8080`) متصل می‌شود.
3.  **Flow:** 
   وقتی SMS جدید می‌آید، اپلیکیشن اندروید آن را پردازش کرده و از طریق WebSocket برای کلاینت متصل (لپ‌تاپ) ارسال می‌کند.

---

### گام ۱: سمت اندروید (Server)

برای راه‌اندازی سرور روی اندروید، بهترین و سبک‌ترین گزینه استفاده از کتابخانه **Ktor** است.

**۱. وابستگی‌ها (build.gradle):**
```kotlin
dependencies {
    implementation("io.ktor:ktor-server-netty:2.3.x")
    implementation("io.ktor:ktor-server-websockets:2.3.x")
    implementation("io.ktor:ktor-server-core:2.3.x")
}
```

**۲. سرویس فورگراند (Foreground Service):**
چون سرور باید همیشه باز باشد (حتی وقتی برنامه بسته می‌شود)، باید سرور را داخل یک `Service` اجرا کنید.

```kotlin
class LocalServerService : Service() {
    private var server: ApplicationEngine? = null
    // لیستی از کلاینت‌های متصل (مرورگرها)
    private val connections = Collections.synchronizedSet(LinkedHashSet<DefaultWebSocketSession>())

    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        startServer()
        // نمایش نوتیفیکیشن برای زنده نگه داشتن سرویس (Foreground) الزامی است
        startForeground(1, createNotification())
        return START_STICKY
    }

    private fun startServer() {
        CoroutineScope(Dispatchers.IO).launch {
            server = embeddedServer(Netty, port = 8080) {
                install(WebSockets)
                routing {
                    webSocket("/otp") {
                        connections += this
                        try {
                            for (frame in incoming) {
                                // اینجا می‌توان پیام‌های دریافتی از سمت کروم را هندل کرد (مثلا پینگ)
                            }
                        } finally {
                            connections -= this
                        }
                    }
                }
            }.start(wait = true)
        }
    }
    
    // متدی که از BroadcastReceiver صدا زده می‌شود تا OTP را ارسال کند
    fun broadcastOtp(otp: String) {
        CoroutineScope(Dispatchers.IO).launch {
            connections.forEach { session ->
                session.send(otp)
            }
        }
    }
    
    override fun onDestroy() {
        server?.stop(1000, 10000)
        super.onDestroy()
    }
    
    override fun onBind(intent: Intent?): IBinder? = LocalBinder()

    inner class LocalBinder : Binder() {
        fun getService(): LocalServerService = this@LocalServerService
    }
}
```

**۳. ارسال پیام:**
زمانی که `BroadcastReceiver` شما SMS را دریافت کرد، باید به این سرویس متصل شود و متد `broadcastOtp` را صدا بزند.

---

### گام ۲: سمت اکستنشن کروم (Client)

اکستنشن فقط نیاز دارد به IP گوشی شما وصل شود.

**۱. فایل `manifest.json`:**
شما نیاز به پرمیشن خاصی برای شبکه لوکال ندارید، اما باید ساختار را درست بچینید.
```json
{
  "manifest_version": 3,
  "name": "LAN OTP Receiver",
  "version": "1.0",
  "permissions": ["notifications", "clipboardWrite"],
  "background": {
    "service_worker": "background.js"
  },
  "action": {
    "default_popup": "popup.html"
  }
}
```

**۲. فایل `popup.html` (برای تنظیم اتصال):**
کاربر باید یک بار IP گوشی را وارد کند (چون IP لوکال ممکن است تغییر کند، یا می‌توانید مکانیزم اسکن IP بگذارید که پیچیده است).
```html
<input type="text" id="ipInput" placeholder="192.168.1.X:8080">
<button id="connectBtn">Connect</button>
<div id="status">Disconnected</div>
```

**۳. فایل `popup.js` یا `background.js`:**
منطق WebSocket در جاوا اسکریپت بسیار ساده است.

```javascript
let socket;

function connectToPhone(ip) {
    // آدرس سرور وب‌سوکت
    const url = `ws://${ip}/otp`;
    socket = new WebSocket(url);

    socket.onopen = () => {
        console.log("Connected to Android!");
        document.getElementById("status").innerText = "Connected";
    };

    socket.onmessage = (event) => {
        const otpCode = event.data;
        console.log("OTP Received:", otpCode);
        
        // 1. نمایش نوتیفیکیشن در ویندوز/مک
        chrome.notifications.create({
            type: 'basic',
            iconUrl: 'icon.png',
            title: 'OTP Received',
            message: `Code: ${otpCode}`
        });

        // 2. کپی در کلیپ‌بورد (نیاز به پرمیشن دارد و باید در کانتکست فعال اجرا شود)
        navigator.clipboard.writeText(otpCode).then(() => {
            console.log('OTP copied to clipboard');
        });
    };

    socket.onclose = () => {
        console.log("Disconnected");
        document.getElementById("status").innerText = "Disconnected";
    };
    
    socket.onerror = (error) => {
        console.error("WebSocket Error:", error);
    };
}

// فراخوانی با دکمه
document.getElementById('connectBtn').addEventListener('click', () => {
    const ip = document.getElementById('ipInput').value;
    connectToPhone(ip);
});
```

---

### چالش‌ها و نکات مهم

**۱. پیدا کردن IP گوشی (Discovery):**
*   **راه حل ساده:** در اپلیکیشن اندروید، IP لوکال گوشی را به کاربر نشان دهید (مثلاً بالای صفحه بنویسید `IP: 192.168.1.5`). کاربر این را در اکستنشن وارد می‌کند.
*   **راه حل پیشرفته:** اپلیکیشن اندروید می‌تواند یک **QR Code** شامل IP و پورت تولید کند. اکستنشن کروم نمی‌تواند مستقیم دوربین را باز کند اما می‌توانید کاربر را به صفحه‌ای هدایت کنید که QR کد گوشی را اسکن کند تا اتصال برقرار شود.

**۲. مدیریت IPهای متغیر (DHCP):**
IP گوشی در شبکه خانگی ممکن است با هر بار اتصال تغییر کند. راه حل "نمایش IP در اپلیکیشن" ساده‌ترین راه برای مدیریت این موضوع است.

**۳. امنیت (Security):**
چون شبکه LAN است، ترافیک شما رمزنگاری نشده (ws:// است نه wss://). اگر نگران شنود در شبکه لوکال هستید:
*   می‌توانید دیتای ارسالی (OTP) را با یک کلید ساده (که کاربر دستی در هر دو طرف وارد کرده) رمزنگاری AES کنید.
*   یا قبل از ارسال OTP، یک "Handshake" ساده انجام دهید تا مطمئن شوید اکستنشن متصل شده، اکستنشن خودتان است (مثلاً اکستنشن یک پسورد بفرستد و اگر درست بود سرور تایید کند).

**۴. مصرف باتری:**
سرویس‌های Foreground در اندروید باتری مصرف می‌کنند. بهتر است در اپلیکیشن یک دکمه "Start PC Sync" و "Stop" بگذارید تا کاربر فقط زمانی که پشت سیستم است سرور را روشن کند.

### خلاصه مراحل
1.  پروژه اندروید: اضافه کردن Ktor و ساخت سرویس WebSocket.
2.  پروژه اندروید: نمایش IP گوشی به کاربر.
3.  اکستنشن کروم: ساخت رابط کاربری برای گرفتن IP و اتصال به WebSocket.
4.  تست: اتصال هر دو به وای‌فای یکسان و ارسال SMS تست.

این روش کاملاً آفلاین است و هیچ دیتایی از اینترنت عبور نمی‌کند.