# SFSDK 使用指南

## Unity 配置步骤

### 1️⃣ 设置 `minSdkVersion` 为 23 ，屏幕方向为竖屏

在 `unityLibrary/build.gradle` 中将 `minSdkVersion` 设置为 **23** 或更高版本。

### 2️⃣ 添加 SFSDK.aar 文件

将提供的 **`SFSDK.aar`** 文件复制到以下目录：
```
unityLibrary/libs
```
### 3️⃣ 修改 `unityLibrary/build.gradle`

在 `dependencies` 中添加以下依赖项：

```gradle
dependencies {
    implementation("com.google.code.gson:gson:2.10.1")
    implementation("com.squareup.okhttp3:okhttp:4.11.0")
    implementation("pl.droidsonroids.gif:android-gif-drawable:1.2.27")
    implementation(name: 'SFSDK', ext: 'aar')
}
```
### 4️⃣ 开启硬件加速 
##### 查找unityLibrary的AndroidManifest.xml，设置hardwareAccelerated值为true
```
    <activity android:hardwareAccelerated="true">
```
### 5️⃣ 在 `UnityPlayerActivity` 中导入 SFSDK

在 `UnityPlayerActivity` 中添加导入语句：

```java
import com.sfsdk.Manager;
```

### 6️⃣ 在 `UnityPlayerActivity onCreate` 中初始化 SFSDK 管理器

```java
protected Manager wm;

@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    wm = new Manager(this, url); // url 为我方提供的字符串加载地址
}

```

📌 **注意：**
- ✅ WebView 会默认显示在 Unity 的最上层，请根据需要使用传入参数设定显示区域。
- 👁 根据调用方式添加需要的方法。
- 🔁 屏幕方向为竖屏
