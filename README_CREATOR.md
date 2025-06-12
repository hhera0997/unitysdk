# SFSDK 使用指南

## Cocos Creator 配置步骤

### 1️⃣ 设置 `minSdkVersion` 为 23 ，屏幕方向为竖屏

在 `gradle.properties` 中将 `PROP_MIN_SDK_VERSION` 值设置为 **23** 或更高版本。

### 2️⃣ 添加 SFSDK.aar 文件

将提供的 **`SFSDK.aar`** 文件复制到 **`libs`**  目录：

### 3️⃣ 修改主模块 `build.gradle`

在 `dependencies` 中添加以下依赖项：

```gradle
dependencies {
    implementation("com.google.code.gson:gson:2.10.1")
    implementation("com.squareup.okhttp3:okhttp:4.11.0")
    implementation("pl.droidsonroids.gif:android-gif-drawable:1.2.27")
}
```

### 4️⃣ 开启硬件加速

##### 查找主模块的AndroidManifest.xml，设置hardwareAccelerated值为true

```
    <activity android:hardwareAccelerated="true">
```

### 5️⃣ 在 `AppActivity` 中导入 SFSDK

在 `AppActivity` 中添加导入语句：

```java
import com.sfsdk.Manager;
```

### 6️⃣ 在 `AppActivity onCreate` 中初始化 SFSDK 管理器

```java
private static Manager wm;

@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    wm = new Manager(this, url); // url 为我方提供的字符串加载地址
}
```

📌 **注意：**

- ✅ WebView 会默认显示在游戏的最上层，请根据需要使用传入参数设定显示区域。
- 👁 根据调用方式添加需要的方法。
- 🔁 屏幕方向为竖屏
