# 💸 UPI Tracker - Android App
## Complete Build Guide to Generate APK & AAB

---

## 📱 App Overview
**UPI Tracker** is a Made-in-India app to track all your UPI payments across GPay, PhonePe, Paytm, BHIM, and more.

### ✨ Features
- 📊 Monthly spending dashboard with ₹ formatting
- 🏷️ 11 categories: Food, Transport, Shopping, Bills, Health, Education, etc.
- 📈 Pie chart reports (MPAndroidChart)
- 🎯 Monthly budget setting per category
- 🔍 Search transactions by name or description
- 📱 Auto-detect UPI transactions from SMS
- 🗄️ Local Room database (no internet required, 100% private)
- 🌐 Supports GPay, PhonePe, Paytm, BHIM, Amazon Pay, WhatsApp Pay
- 🇮🇳 Hindi tagline: "आपके पैसों का हिसाब"

---

## 🛠️ Prerequisites

Install the following before building:

1. **Android Studio** (latest) → https://developer.android.com/studio
2. **JDK 17+** (bundled with Android Studio)
3. **Android SDK** (installed via Android Studio SDK Manager)
   - Compile SDK: 34
   - Min SDK: 24 (Android 7.0+)
   - Build Tools: 34.0.0

---

## 📂 Project Structure

```
UPITracker/
├── app/
│   ├── src/main/
│   │   ├── java/com/india/upitracker/
│   │   │   ├── model/         ← Transaction, Budget, Category
│   │   │   ├── data/          ← Room DB, DAOs, Repository
│   │   │   ├── ui/            ← All Activities + Adapters
│   │   │   └── utils/         ← SMSReceiver, CurrencyFormatter
│   │   └── res/               ← Layouts, colors, strings
│   ├── build.gradle
│   └── proguard-rules.pro
├── build.gradle
├── settings.gradle
└── gradle.properties
```

---

## 🚀 Step-by-Step Build Instructions

### Method 1: Android Studio (Recommended)

**Step 1: Open Project**
```
1. Open Android Studio
2. File → Open → Select this "UPITracker" folder
3. Wait for Gradle sync to complete (~2-3 minutes first time)
```

**Step 2: Add MPAndroidChart Dependency**
In `app/build.gradle`, the MPAndroidChart library is included:
```groovy
implementation 'com.github.PhilJay:MPAndroidChart:v3.1.0'
```

You need to add JitPack repository. In root `build.gradle`, add:
```groovy
allprojects {
    repositories {
        google()
        mavenCentral()
        maven { url 'https://jitpack.io' }  // ← ADD THIS LINE
    }
}
```

**Step 3: Generate DEBUG APK (for testing)**
```
Build → Build Bundle(s) / APK(s) → Build APK(s)
```
Output: `app/build/outputs/apk/debug/app-debug.apk`

**Step 4: Generate RELEASE APK**
```
1. Build → Generate Signed Bundle / APK
2. Select "APK"
3. Create new keystore (or use existing):
   - Key store path: Choose location (e.g., upitracker.jks)
   - Password: Set a strong password
   - Key alias: upitracker
   - Key password: Set password
   - Fill in: Name, Organization, Country Code (IN)
4. Click Next → Select "release" build variant
5. Click Finish
```
Output: `app/build/outputs/apk/release/app-release.apk`

**Step 5: Generate AAB (for Play Store)**
```
1. Build → Generate Signed Bundle / APK
2. Select "Android App Bundle" (AAB)
3. Use same keystore from Step 4
4. Click Next → Select "release" build variant
5. Click Finish
```
Output: `app/build/outputs/bundle/release/app-release.aab`

---

### Method 2: Command Line

```bash
# Navigate to project folder
cd UPITracker

# Make gradlew executable
chmod +x gradlew

# Build DEBUG APK
./gradlew assembleDebug

# Build RELEASE APK (unsigned)
./gradlew assembleRelease

# Build AAB (unsigned)
./gradlew bundleRelease

# Sign the APK manually (after generating keystore)
jarsigner -verbose -sigalg SHA256withRSA -digestalg SHA-256 \
  -keystore upitracker.jks \
  app/build/outputs/apk/release/app-release-unsigned.apk \
  upitracker

# Align APK
zipalign -v 4 \
  app/build/outputs/apk/release/app-release-unsigned.apk \
  app-release-signed.apk
```

---

## 📦 Output Files

| File | Use Case | Location |
|------|----------|----------|
| `app-debug.apk` | Testing / Sideloading | `app/build/outputs/apk/debug/` |
| `app-release.apk` | Direct distribution | `app/build/outputs/apk/release/` |
| `app-release.aab` | Google Play Store | `app/build/outputs/bundle/release/` |

---

## 📲 Installing APK on Phone

1. Enable **Unknown Sources** on your Android phone:
   `Settings → Security → Install Unknown Apps`
2. Transfer APK to phone via USB / WhatsApp / email
3. Tap the APK file to install
4. Grant SMS permission when prompted (for auto-detection)

---

## 🏪 Publishing to Play Store

1. Create Google Play Console account → https://play.google.com/console
2. Create new app → Fill in details
3. Upload `app-release.aab` (NOT apk) to Play Store
4. Fill in store listing: title, description, screenshots
5. Set content rating, pricing (Free)
6. Submit for review

---

## 🔧 Customization Tips

**Change app colors** → `res/values/colors.xml`
- Primary orange: `#FF6B35`

**Add more categories** → `model/Transaction.kt`
- Add to `Category` enum

**Change app name** → `res/values/strings.xml`

**Add more UPI apps** → `ui/AddTransactionActivity.kt`
- Add to `upiApps` list

---

## 🐛 Troubleshooting

| Issue | Fix |
|-------|-----|
| Gradle sync fails | Check internet, File → Invalidate Caches |
| MPAndroidChart not found | Add JitPack repo (see Step 2) |
| Build fails on Room | Check kapt is applied in build.gradle |
| APK won't install | Enable Unknown Sources on phone |

---

## 📋 App Permissions

| Permission | Reason |
|-----------|--------|
| RECEIVE_SMS | Auto-detect UPI transactions |
| READ_SMS | Read bank SMS for amounts |
| VIBRATE | Notification feedback |

*All data is stored locally. No internet connection required.*

---

## 🇮🇳 Made with ❤️ for India
*Helping 500+ crore UPI users track their money*
