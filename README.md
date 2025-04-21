# SFSDK 使用指南

## Unity 配置步骤

### 1️⃣ 设置 `minSdkVersion` 为 23

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
    implementation(name: 'SFSDK', ext: 'aar')
}
```

### 4️⃣ 在 `UnityPlayerActivity` 中导入 SFSDK

在 `UnityPlayerActivity` 中添加导入语句：

```java
import com.sfsdk.Manager;
```

### 5️⃣ 开启硬件加速 
##### 查找unityLibrary的AndroidManifest.xml，设置hardwareAccelerated值为true
```
    <activity android:hardwareAccelerated="true" android:name="com.unity3d.player.UnityPlayerActivity">
```

### 6️⃣ 在 `onCreate` 中初始化 SFSDK 管理器

```java
protected Manager wm;

@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    wm = new Manager(this, url); // url 为我方提供的字符串加载地址
}
/*
 * =============================================
 * WebView 前端展示功能（可选）
 * 以下方法为接入 SDK 的 WebView 展示控制接口
 * 如不需要展示 WebView，到此 SDK 接入即完成
 * =============================================
 */
// 判断是否可以显示创建 WebView 按钮，游戏启动10秒后从url地址获取，商店审核阶段不显示
public boolean IsConfigEnabled() {
    return wm.isConfigEnabled();
}

// 判断 WebView 是否已创建
public boolean HasWVIn() {
    return wm.hasWVIn();
}

// 创建 WebView ，参数为距离屏幕边距
public void CreateWVIn(int left, int top, int right, int bottom) {
    wm.createWVIn(left, top, right, bottom);
}

// 销毁 WebView
public void DestroyWVIn() {
    wm.destroyWVIn();
}

// 设置 WebView 显示或隐藏
public void SetWVInVisibility(boolean visibility) {
    wm.setWVInVisibility(visibility);
}

// 重新加载 WebView 的 URL
public void ReloadWVInURL() {
    wm.reloadWVInURL();
}
```

✅ **url** 为字符串类型地址，由我方提供，请联系运营获取。

## Unity 侧调用 Java 方法（C# 脚本）

```csharp
using UnityEngine;

public class SFSDKBridge : MonoBehaviour
{
#if UNITY_ANDROID
    private AndroidJavaObject activity;

    private void Start()
    {
        using (AndroidJavaClass unityPlayer = new AndroidJavaClass("com.unity3d.player.UnityPlayer"))
        {
            activity = unityPlayer.GetStatic<AndroidJavaObject>("currentActivity");
        }
    }

    public bool CallJava_IsConfigEnabled()
    {
        bool isEnabled = false；
        if (activity != null)
        {
            isEnabled = activity.Call<bool>("IsConfigEnabled");
            Debug.Log("Java返回的 IsConfigEnabled: " + isEnabled);
        }
        else
        {
            Debug.LogWarning("activity is null");
        }
        return isEnabled;
    }

    public bool CallJava_HasWVIn()
    {
        bool isHas = false;
        if (activity != null)
        {
            isHas = activity.Call<bool>("HasWVIn");
            Debug.Log("Java返回的 HasWVIn: " + isHas);
        }
        else
        {
            Debug.LogWarning("activity is null");
        }
        return isHas;
    }

    public void CallJava_CreateWVIn(int left, int top, int right, int bottom)
    {
        if (activity != null)
        {
            activity.Call("CreateWVIn", left, top, right, bottom);
            Debug.Log("调用 Java 的 CreateWVIn");
        }
        else
        {
            Debug.LogWarning("activity is null");
        }
    }

    public void CallJava_DestroyWVIn()
    {
        if (activity != null)
        {
            activity.Call("DestroyWVIn");
            Debug.Log("调用 Java 的 DestroyWVIn");
        }
        else
        {
            Debug.LogWarning("activity is null");
        }
    }

    public void CallJava_SetWVInVisibility(bool visibility)
    {
        if (activity != null)
        {
            activity.Call("SetWVInVisibility", visibility);
            Debug.Log("调用 Java 的 SetWVInVisibility: " + visibility);
        }
        else
        {
            Debug.LogWarning("activity is null");
        }
    }

    public void CallJava_ReloadWVInURL()
    {
        if (activity != null)
        {
            activity.Call("ReloadWVInURL");
            Debug.Log("调用 Java 的 ReloadWVInURL");
        }
        else
        {
            Debug.LogWarning("activity is null");
        }
    }
#endif
}
```

## 使用说明与调用步骤

### ✅ 创建 WebView 前的配置判断

在调用 `CallJava_CreateWVIn()` 创建 WebView 之前，务必先调用：

```csharp
CallJava_IsConfigEnabled();
```

以确保配置加载完成。

### 💡 调用逻辑推荐（两种方式）

#### ✔️ 方式一（推荐）：

- ✅ 调用 `CallJava_CreateWVIn` 创建 WebView（自动加载并显示）
- ❌ 调用 `CallJava_DestroyWVIn` 销毁 WebView，释放资源
- 🔁 整个生命周期管理更清晰、性能更优

#### 🔁 方式二（更灵活控制）：

- ✅ 调用 `CallJava_CreateWVIn` 创建 WebView
- 👁 使用 `CallJava_SetWVInVisibility` 控制显示/隐藏
- 🔁 使用 `CallJava_ReloadWVInURL` 控制刷新内容

📌 **注意：**
- ✅ WebView 会默认显示在 Unity 的最上层，请根据需要使用传入参数设定显示区域。
- 👁 根据调用方式添加需要的方法。
- 🔁 屏幕方向为竖屏
