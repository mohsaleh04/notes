برای نصب ابتدا اومدیم apt رو آپدیت و آپگرید کردیم.
بعد از اون اومدیم پکیج های مورد نیاز از حمله jdk رو نصب کردیم:

`sudo apt update && sudo apt upgrade -y sudo apt install -y openjdk-11-jdk wget unzip libvirt-daemon-system qemu-kvm virt-manager bridge-utils libvirt-clients`

---

## 📦 مرحله 2: نصب Android SDK + Emulator


`mkdir -p ~/android-sdk/cmdline-tools cd ~/android-sdk/cmdline-tools  # دانلود ابزار SDK wget https://dl.google.com/android/repository/commandlinetools-linux-11076708_latest.zip -O sdk-tools.zip unzip sdk-tools.zip mv cmdline-tools latest`

---

## 🔧 مرحله 3: اضافه کردن Android SDK به PATH

این دستورات رو در انتهای فایل `~/.bashrc` قرار بده (با `nano ~/.bashrc`) یا مستقیم در ترمینال بزن:

bash

CopyEdit

`echo 'export ANDROID_SDK_ROOT=$HOME/android-sdk' >> ~/.bashrc echo 'export PATH=$PATH:$ANDROID_SDK_ROOT/cmdline-tools/latest/bin:$ANDROID_SDK_ROOT/platform-tools:$ANDROID_SDK_ROOT/emulator' >> ~/.bashrc source ~/.bashrc`

---

## 📦 مرحله 4: نصب SDK، Emulator، و سیستم ایمیج

bash

CopyEdit

`sdkmanager --sdk_root=$ANDROID_SDK_ROOT "platform-tools" "emulator" "platforms;android-30" "system-images;android-30;google_apis;x86_64"`

---

## 🛠️ مرحله 5: ساخت AVD (Android Virtual Device)

bash

CopyEdit

`echo "no" | avdmanager create avd -n test_emulator -k "system-images;android-30;google_apis;x86_64" --device "pixel"`

---

## 🖥️ مرحله 6: نصب X11 و اجرای محیط گرافیکی برای مشاهده Emulator

bash

CopyEdit

`sudo apt install -y x11-apps x11-utils xfce4 xfce4-goodies tightvncserver`

راه‌اندازی اولیه VNC:

bash

CopyEdit

`vncserver # رمز دلخواه وارد کن`

---

## 🔁 مرحله 7: متوقف‌سازی و پیکربندی VNC

bash

CopyEdit

`vncserver -kill :1`

سپس فایل زیر رو بساز یا ویرایش کن:

bash

CopyEdit

`nano ~/.vnc/xstartup`

و این محتوا رو داخلش بذار:

bash

CopyEdit

`#!/bin/bash xrdb $HOME/.Xresources startxfce4 &`

و مجوز اجرا بده:

bash

CopyEdit

`chmod +x ~/.vnc/xstartup`

---

## ▶️ مرحله 8: اجرای VNC و Android Emulator

اجرای VNC:

bash

CopyEdit

`vncserver :1`

حالا یه پورت فوروارد با MobaXterm بساز از `localhost:5901` روی سیستم خودت.

سپس ترمینال جدید باز کن و Emulator رو اینجوری اجرا کن:

bash

CopyEdit

`emulator -avd test_emulator -gpu on -no-snapshot -accel on` 

اگر خطا گرفتی در مورد شتاب GPU، میشه از `-gpu swiftshader_indirect` یا `-gpu off` استفاده کرد.


برای فعال شدن حتماً باید VT-x  (KVM, Hardware Acceleration) فعاال باشه.

اگه باز نشد، مموری avd رو روی 4096 بزارید.

جاوا (JDK) رو حتماً روی 17 بزارید
سیستم ایمیج پیشنهادی جهت نصب:
system-images;android-30;default;x86_64

دیوایس پیشنهادی جهت اجرا `pixel`


کامند نهایی جهت اجرا:
```shell
emulator -avd test_x86 -accel on -gpu swiftshader_indirect -no-snapshot -no-audio -no-boot-anim
```

برای اجرا به صورت headless باید 
```shell
emulator -avd test_x86 -no-window -no-audio -no-boot-anim -gpu off -accel on
```