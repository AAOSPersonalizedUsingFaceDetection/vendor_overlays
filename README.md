# Cairomotive – Custom AOSP Automotive Overlays

This repository includes **runtime resource overlays (RROs)** designed to customize multi-user behavior and system UI layout in an Android Automotive OS (AAOS) environment, specifically targeted for devices like the **Raspberry Pi 4**.

These overlays enhance system behavior and visuals, particularly for automotive scenarios where driver identity and minimal UI are essential.

---

## 🧭 Project Structure

```

cairomotive/
├── DriverID\_UserConfigs\_StaticOverlay/     # Multi-user configuration overlay
│   ├── res/values/config.xml               # Multi-user behavior flags
│   ├── Android.bp
│   └── AndroidManifest.xml
│
├── SystemBarBottom/                        # Bottom system bar layout and visuals
│   ├── res/
│   │   ├── drawable/                       # Custom system bar background
│   │   ├── values/                         # Configs and dimensions
│   │   └── xml/                            # RRO mapping for SystemUI
│   ├── Android.bp
│   └── AndroidManifest.xml
│
└── aosp\_rpi4\_car\_cm.mk                     # AOSP makefile additions

````

---

## 🧩 Overlay Modules

### 1. 🔐 DriverID_UserConfigs_StaticOverlay

An **RRO module** that statically configures Android's multi-user environment.

#### Key Features:
- Enables multi-user UI (`config_enableMultiUserUI`)
- Sets max number of users to 10
- Sets maximum running users to 9
- Hides the user switcher from the UI

#### Target Package:
```xml
<overlay android:targetPackage="android" android:isStatic="true" />
````

#### Source:

* `res/values/config.xml`

#### Build File:

* `Android.bp` using `runtime_resource_overlay`

---

### 2. 📱 SystemBarBottom

An **RRO module** that customizes the **CarSystemUI** by:

* Displaying only the **bottom** system bar
* Applying a **rounded rectangle** background with a custom color
* Configuring z-order, padding, and heads-up notification (HUN) behavior

#### Key Configurations:

* `config_enableBottomSystemBar = true`
* Other system bars (top, left, right) are disabled
* Z-order logic ensures no UI conflicts
* Custom drawable: `system_bar_background.xml` with rounded corners

#### Overlay Mapping:

Uses `res/xml/car_sysui_overlays.xml` to remap target values:

```xml
<overlay android:resourcesMap="@xml/car_sysui_overlays" />
```

#### Target Package:

```xml
<overlay android:targetPackage="com.android.systemui" />
```

#### Build File:

* `Android.bp` with `runtime_resource_overlay`

---

## 🛠️ Integration with AOSP

To integrate these overlays into your AOSP build for **Raspberry Pi 4** or other AAOS targets:

### 📦 Include in Build

Add to your device/product makefile:

```makefile
PRODUCT_PACKAGES += DriverIDUserConfigsStaticOverlay
PRODUCT_PACKAGES += CustomCarSystemUIBottomRoundedRRO
```

File: `aosp_rpi4_car_cm.mk`

### 🔒 Permissions and Compatibility

Ensure SELinux permissions and overlay targets are compatible with your system image.

### 🔧 Notes

* Overlays are applied **at runtime** without rebuilding core system packages.
* Suitable for **CarLauncher**, **SystemUI**, or other AAOS-based customizations.
* All components are Apache-licensed for integration flexibility.

---

