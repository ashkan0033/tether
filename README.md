# کلارک | Clark

**تماس و پیامک سیم‌کارت اندروید روی آیفون، آیپد یا گوشی اندرویدی دوم**

کلارک، گوشی اندرویدیِ سیم‌کارت‌دار را از طریق بلوتوث به دستگاه دوم وصل می‌کند. بعد از جفت‌سازی با کد QR، می‌توانید از دستگاه دوم تماس بگیرید، پیامک بفرستید و مخاطبان و سوابق تماس گوشی میزبان را ببینید. ارتباط بین دستگاه‌ها رمزنگاری سرتاسری دارد و به حساب کاربری یا سرور واسط نیاز ندارد.

## وضعیت توسعه

| بخش | آخرین وضعیت تأییدشده |
| --- | --- |
| اندروید | هر دو APK آزمایشی ساخته شده‌اند و ۳۳۱ تست خودکار هسته، میزبان، کلاینت و ماژول کمکی پاس شده‌اند |
| آیفون | بیلد بدون امضا با Xcode و تست‌های خودکار اپ در شبیه‌ساز پاس شده‌اند |
| رابط فارسی آیفون | صفحهٔ اتصال، اسکن QR و ورود دستی کد بازطراحی شده‌اند. صفحهٔ اسکن برای دوربین غیرفعال یا غیرقابل‌دسترس مسیر جایگزین دارد |
| گوشی واقعی | جفت‌سازی بلوتوث، تماس واقعی، صدای دوطرفه، پیامک و اتصال دوباره باید روی دستگاه‌های واقعی تأیید شوند |

توسعهٔ اپ در شاخهٔ [`clark/app-branding`](https://github.com/ashkan0033/tether/tree/clark/app-branding) و [Pull Request شمارهٔ ۱](https://github.com/ashkan0033/tether/pull/1) جریان دارد. [نتیجهٔ CI نسخهٔ بررسی‌شده](https://github.com/ashkan0033/tether/actions/runs/45600836462) را می‌توانید در GitHub ببینید. برای آیفون هنوز نسخهٔ امضاشدهٔ قابل نصب روی گوشی منتشر نشده است.

## دستگاه‌های مورد نیاز

| دستگاه | حداقل نیاز | نقش |
| --- | --- | --- |
| گوشی اندرویدی سیم‌کارت‌دار | Android 8 و پشتیبانی از تبلیغ BLE | میزبان سیم‌کارت، تماس و پیامک |
| آیفون یا آیپد | iOS یا iPadOS 16 | اتصال به میزبان |
| گوشی اندرویدی دوم | Android 10 | جایگزین آیفون یا آیپد به‌عنوان دستگاه دوم |

صدای دوطرفه روی دستگاه دوم به نسخهٔ اندروید، مدل میزبان و امکانات صوتی آن بستگی دارد. [جدول سازگاری اندروید](docs/devices/android-versions.md)

## راه‌اندازی روی دو گوشی

1. نسخهٔ میزبان را روی اندروید سیم‌کارت‌دار نصب کنید. مراحل داخل اپ را انجام دهید و کلارک را به‌عنوان برنامهٔ پیش‌فرض تلفن انتخاب کنید.
2. بلوتوث هر دو دستگاه را روشن کنید. روی میزبان، گزینهٔ «جفت کردن آیفون» را بزنید تا اپ کد QR را نمایش دهد. این کد پنج دقیقه اعتبار دارد.
3. روی آیفون یا گوشی اندرویدی دوم، صفحهٔ اتصال را باز کنید و QR را اسکن کنید. در آیفون می‌توانید کد یا پیوند جفت‌سازی را دستی هم وارد کنید.

برای جزئیات مجوزها و راه‌اندازی صدا، [راهنمای اتصال](docs/SETUP.md) و [راهنمای کلاینت اندروید](docs/ANDROID-CLIENT.md) را ببینید.

## ساخت از سورس

ابتدا شاخهٔ توسعه را بگیرید:

```bash
git clone --branch clark/app-branding https://github.com/ashkan0033/tether.git clark
cd clark
```

### اندروید

JDK 17 و Android SDK Platform 36 را نصب کنید. مسیر SDK را با Android Studio یا در `android/local.properties` تنظیم کنید. Gradle Wrapper در اولین اجرا نسخهٔ Gradle و وابستگی‌ها را دریافت می‌کند.

```bash
cd android
./gradlew :app:assembleDebug :client:assembleDebug
./gradlew :app:testDebugUnitTest :client:testDebugUnitTest :helper:testDebugUnitTest
```

روی ویندوز، در PowerShell از `.\gradlew.bat` استفاده کنید. فایل‌های خروجی در مسیرهای زیر هستند:

- `android/app/build/outputs/apk/debug/app-debug.apk` برای گوشی سیم‌کارت‌دار
- `android/client/build/outputs/apk/debug/client-debug.apk` برای گوشی دوم

### آیفون و آیپد

برای ساخت اپ از سورس به macOS، Xcode و XcodeGen نیاز دارید. از ریشهٔ پروژه اجرا کنید:

```bash
cd ios
xcodegen generate
xcodebuild -project Tether.xcodeproj -scheme Tether \
  -destination 'generic/platform=iOS' -skipMacroValidation CODE_SIGNING_ALLOWED=NO build
```

برای بررسی ظاهر، پروژهٔ `Tether.xcodeproj` را در Xcode باز کنید، یک شبیه‌ساز آیفون انتخاب کنید و Run را بزنید. آرگومان‌های `-TetherDemoPair` و `-TetherDemoPairManual` در نسخهٔ Debug مسیر اسکن و ورود دستی را باز می‌کنند. برای نصب روی آیفون واقعی، تنظیمات امضا را طبق [راهنمای بیلد](docs/BUILD.md) انجام دهید.

## ساختار پروژه

| مسیر | کاربرد |
| --- | --- |
| `android/app` | اپ میزبان اندروید |
| `android/client` | اپ گوشی دوم اندرویدی |
| `ios/Tether` | اپ آیفون و آیپد |
| `android/core`، `ios/TetherCore` و `protocol` | کتابخانه‌های مشترک، رمزنگاری و تعریف پروتکل |

بعضی نام‌های فنی و راهنماها هنوز نام Tether را دارند. [سؤالات رایج](docs/FAQ.md) · [ثبت گزارش خطا](https://github.com/ashkan0033/tether/issues)

## About Clark

Clark connects a SIM-equipped Android phone to an iPhone, iPad, or second Android device over an end-to-end encrypted Bluetooth link. It brings calls, SMS, contacts, and call history to the second device. Android debug builds and the unsigned iOS build have passed CI. Signed iPhone installation and physical-device verification are pending.

Clark is based on [Tether by amirwmr and contributors](https://github.com/amirwmr/tether) and is distributed under the [MIT License](LICENSE).
