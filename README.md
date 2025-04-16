# SFSDK 使用指南

## Unity 配置
### 1. 下载并放置 SFSDK.aar  
将 **SFSDK.aar** 下载后，放入 `unityLibrary/libs` 目录。

### 2. 修改 `unityLibrary/build.gradle`  
在 `dependencies` 部分添加以下依赖项：

```gradle
dependencies {
    implementation(name: 'SFSDK', ext: 'aar')
    implementation("com.google.code.gson:gson:2.10.1")
    implementation("com.squareup.okhttp3:okhttp:4.11.0")
}
```

### 3. 在 `UnityPlayerActivity` 中导入 SFSDK  
在 `UnityPlayerActivity` 类中添加导入：

```java
import com.sfsdk.Manager;
```

### 4. 在 `onCreate` 方法中初始化 `Manager`  
在 `onCreate` 方法中添加以下代码：

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    
    Manager wm = new Manager(this, url);
}
```
#### url是一个字符串类型加载地址， 由我方提供，请联系运营

### 5. minSdkVersion 23

