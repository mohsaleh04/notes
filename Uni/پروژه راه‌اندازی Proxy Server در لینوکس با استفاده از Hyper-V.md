## معرفی پروژه

هدف این پروژه راه‌اندازی یک سرور پروکسی (Proxy Server) روی یک ماشین مجازی لینوکسی و تست آن از طریق یک کلاینت دیگر در یک محیط شبکه‌ای داخلی با استفاده از Hyper-V ویندوز است.

---

## پراکسی سرور چیست؟

پراکسی سرور برای ایجاد یک اتصال بین یک کلاینت و یک سرور استفاده میشود به طوری که ترافیک کلاینت ، به صورت مخفی شده، از سمت سرور به اینترنت هدایت میشود و برعکس، اطلاعات از اینترنت ، ابتدا به سرور و بعد به کلاینت پورت میشوند.

فواید پراکسی سرور: برای مخفی کردن هویت حین ارتباط با اینترنت، که خیلی در سازمان ها استفاده میشود.
برای رفع محدودیت های اینترنتی و ترافیکی از جمله تحریم و فیلترینگ 


---

## مراحل اجرا

### 1. ساخت سوییچ مجازی در Hyper-V

1. باز کردن **Hyper-V Manager**
    
2. از منوی سمت راست، گزینه **Virtual Switch Manager** را انتخاب کنید.
    
3. ساخت یک سوییچ جدید از نوع **Internal**
    
4. نام‌گذاری (مثلاً `MyInternalNet`)
    
5. ذخیره و خروج
    

### 2. ساخت دو ماشین مجازی

- VM1: با نقش **Proxy Server**
    
- VM2: با نقش **Client**
    

تنظیمات پیشنهادی:

- 2 گیگ رم برای هر ماشین
    
- اتصال هر VM به سوییچ `MyInternalNet`
    
- نصب سیستم عامل لینوکس روی هر VM
    

### 3. تنظیم IP دستی برای هر ماشین

در هر ماشین دستور زیر را اجرا کنید (بسته به سیستم‌عامل ممکن است کمی متفاوت باشد):

**Proxy Server:**

- IP: `192.168.100.10`
    
- Subnet: `255.255.255.0`
    

**Client:**

- IP: `192.168.100.20`
    
- Subnet: `255.255.255.0`
    

(نیازی به Gateway نیست چون فقط داخل شبکه داخلی کار می‌کنیم)

---

## نصب و تنظیم Squid روی Proxy Server

### نصب Squid

#### روی Ubuntu یا debian بیس ها:

```bash
sudo apt update
sudo apt install squid
```

#### روی Fedora یا redhat  بیس ها:

```bash
sudo dnf install squid
```

### فعال‌سازی و اجرای سرویس

```bash
sudo systemctl enable --now squid
```

### ویرایش تنظیمات Squid

فایل تنظیمات:

```bash
sudo nano /etc/squid/squid.conf
```

برای اجازه دادن به همه کلاینت‌ها (در محیط تست):

```conf
http_access allow all
http_port 3128
```

### باز کردن پورت در فایروال

#### UFW:

```bash
sudo ufw allow 3128
```

#### firewalld:

```bash
sudo firewall-cmd --add-port=3128/tcp --permanent
sudo firewall-cmd --reload
```

---

## تست عملکرد از سمت کلاینت

### با استفاده از curl:

```bash
curl -x http://192.168.100.10:3128 http://example.com
```

### تنظیم متغیرهای محیطی برای استفاده کلی‌تر:

```bash
export http_proxy=http://192.168.100.10:3128
export https_proxy=http://192.168.100.10:3128
```

برای دائمی شدن، این خطوط را به فایل `~/.bashrc` یا `~/.bash_profile` اضافه کنید.

---

## نکات تکمیلی (برای ارائه بهتر پروژه)

- فعال‌سازی لاگ در Squid: بررسی کنید کدام کلاینت‌ها چه سایت‌هایی را باز کرده‌اند.
    
- استفاده از ACL در squid.conf برای محدودسازی دسترسی فقط به کلاینت مشخص
    
- بلاک کردن سایت‌های خاص
    

---

## نتیجه‌گیری

با استفاده از این پروژه، ما توانستیم یک ساختار پایه‌ای شبکه مجازی با یک سرور پروکسی واقعی ایجاد کرده و کاربرد آن در مدیریت و کنترل ترافیک شبکه را به صورت عملی تجربه کنیم.
