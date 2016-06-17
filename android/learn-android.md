## Android环境的安装与配置
### 步骤
- 下载并安装[Android Studio](https://developer.android.com/studio/index.html)
- 安装SDK
- 配置环境变量(可选)

### 资料
- [官方文档中文版-Android入门基础：从这里开始](http://hukai.me/android-training-course-in-chinese/basics/index.html)
- [官方文档-Android Studio官方文档](https://developer.android.com/studio/intro/index.html)
- [Android Studio 操作手册](http://wiki.jikexueyuan.com/project/android-studio-guide/)
- [stormzhang的博客](http://stormzhang.com/)

---

## Hello Android
### 项目结构
- manifests
- java
- res
    - drawable
    - layout
    - values

### 资源
- `drawable` 图片资源
- `layout` 布局文件
- `values` 包含字符串、整型数和颜色等简单值的XML文件
- `R文件` 资源文件的使用分为在代码中使用和在其他资源文件中引用该资源文件。在我们编译一个Android应用时，Android会自动生成一个R类，在该类中根据不同的资源类型又生成了相应的内部类，该类包含了系统中使用到的所有资源文件的标示
- [官方文档-使用资源](https://developer.android.com/guide/topics/resources/providing-resources.html)

### AndroidManifest
- 声明组件
        <manifest>
            <application android:icon="@drawable/app_icon.png">
                <activity android:name="com.example.project.ExampleActivity"
                    android:label="@string/example_label">
                </activity>
                ...
            </application>
        </manifest>
- 确定应用需要的任何用户权限，如互联网访问权限或对用户联系人的读取权限
- 根据应用使用的API，声明应用所需的最低API级别
- 声明应用使用或需要的硬件和软件功能，如相机、蓝牙服务或多点触摸屏幕
        <manifest ... >
            <uses-feature android:name="android.hardware.camera.any"
                android:required="true" />
            <uses-sdk android:minSdkVersion="7" android:targetSdkVersion="19" />
            ...
        </manifest>
- 应用需要链接的API库，如Google Maps API库
- [官方文档-应用清单文件说明](https://developer.android.com/guide/topics/manifest/manifest-intro.html)

### Log
- verbose => Log.v() : 最低粒度最为琐碎的日志
- debug => Log.d() : 调试信息日志
- info => Log.i() : 相对更为重要的数据
- warn => Log.w() : 警告信息
- error => Log.e() : 错误信息
- tag & message
- 使用Log类的优点
    - 自动分级
    - 可以使用logcat方便的进行过滤
- [官方文档-Log](https://developer.android.com/reference/android/util/Log.html)

### 资料
- [官方文档-建立第一个App](http://hukai.me/android-training-course-in-chinese/basics/firstapp/index.html)
- [官方文档-应用基础知识](https://developer.android.com/guide/components/fundamentals.html)

---

## Activity
`Activity`是一个应用组件，用户可与其提供的屏幕进行交互，以执行拨打电话、拍摄照片、发送电子邮件或查看地图等操作。
每个`Activity`都会获得一个用于绘制其用户界面的窗口。窗口通常会充满屏幕，但也可小于屏幕并浮动在其他窗口之上。

### Overview
[官方文档-Activity](https://developer.android.com/guide/components/activities.html)

### 生命周期
#### Activity的三种状态
- `已继续` 此 Activity 位于屏幕前台并具有用户焦点。（有时也将此状态称作“运行中”）
- `已暂停` 另一个 Activity 位于屏幕前台并具有用户焦点，但此 Activity 仍可见。也就是说，另一个 Activity 显示在此 Activity 上方，并且该 Activity 部分透明或未覆盖整个屏幕。 已暂停的 Activity 处于完全 Activity 状态（Activity 对象保留在内存中，它保留了所有状态和成员信息，并与窗口管理器保持连接），但在内存极度不足的情况下，可能会被系统终止。
- `已停止` 该 Activity 被另一个 Activity 完全遮盖（该 Activity 目前位于“后台”）。 已停止的 Activity 同样仍处于 Activity 状态（Activity 对象保留在内存中，它保留了所有状态和成员信息，但未与窗口管理器连接）。 不过，它对用户不再可见，在他处需要内存时可能会被系统终止。

#### 生命周期的方法
        public class ExampleActivity extends Activity {
            @Override
            public void onCreate(Bundle savedInstanceState) {
                super.onCreate(savedInstanceState);
                // The activity is being created.
            }

            @Override
            protected void onStart() {
                super.onStart();
                // The activity is about to become visible.
            }

            @Override
            protected void onResume() {
                super.onResume();
                // The activity has become visible (it is now "resumed").
            }

            @Override
            protected void onPause() {
                super.onPause();
                // Another activity is taking focus (this activity is about to be "paused").
            }

            @Override
            protected void onStop() {
                super.onStop();
                // The activity is no longer visible (it is now "stopped")
            }

            @Override
            protected void onDestroy() {
                super.onDestroy();
                // The activity is about to be destroyed.
            }
        }

#### 生命周期说明
- Activity的整个生命周期发生在`onCreate()`调用与`onDestroy()`调用之间。您的Activity应在`onCreate()`中执行“全局”状态设置（例如定义布局），并在`onDestroy()`中释放的所有其余资源。例如，如果您的Activity有一个在后台运行的线程，用于从网络上下载数据，它可能会在`onCreate()`中创建该线程，然后在`onDestroy()`中停止该线程。
- Activity的可见生命周期发生在`onStart()`调用与`onStop()`调用之间。在这段时间，用户可以在屏幕上看到Activity并与其交互。 例如，当一个新Activity启动，并且此Activity不再可见时，系统会调用`onStop(`)。您可以在调用这两个方法之间保留向用户显示Activity所需的资源。 例如，您可以在`onStart()`中注册一个BroadcastReceiver以监控影响UI的变化，并在用户无法再看到您显示的内容时在`onStop()`中将其取消注册。在Activity的整个生命周期，当Activity在对用户可见和隐藏两种状态中交替变化时，系统可能会多次调用`onStart()`和`onStop()`。
- Activity的前台生命周期发生在`onResume()`调用与`onPause()`调用之间。在这段时间，Activity位于屏幕上的所有其他Activity之前，并具有用户输入焦点。Activity可频繁转入和转出前台—例如，当设备转入休眠状态或出现对话框时，系统会调用onPause()。由于此状态可能经常发生转变，因此这两个方法中应采用适度轻量级的代码，以避免因转变速度慢而让用户等待。

#### 生命周期图示
![生命周期图示](https://developer.android.com/images/activity_lifecycle.png)

#### 生命周期详解
| 方法        | 说明                                                                                                                                                                                                                                                              |
|-------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **onStart()**   | 首次创建Activity时调用。 您应该在此方法中执行所有正常的静态设置— 创建视图、将数据绑定到列表等等。系统向此方法传递一个Bundle对象，其中包含Activity的上一状态，不过前提是捕获了该状态。始终后接`onStart()`。                                                                                   |
| **onRestart()** | 在Activity已停止并即将再次启动前调用。始终后接`onStart()`                                                                                                                                                                                                              |
| **onStart()**   | 在Activity即将对用户可见之前调用。如果Activity转入前台，则后接`onResume()`，如果Activity转入隐藏状态，则后接`onStop()`。                                                                                                                                                     |
| **onResume()**  | 在Activity即将开始与用户进行交互之前调用。此时，Activity处于Activity堆栈的顶层，并具有用户输入焦点。始终后接`onPause()`。                                                                                                                                                      |
| **onPause()**   | 当系统即将开始继续另一个Activity时调用。此方法通常用于确认对持久性数据的未保存更改、停止动画以及其他可能消耗CPU的内容，诸如此类。 它应该非常迅速地执行所需操作，因为它返回后，下一个Activity才能继续执行。如果Activity返回前台，则后接`onResume()`，如果Activity转入对用户不可见状态，则后接`onStop()`。 |
| **onStop()**   | Activity对用户不再可见时调用。如果Activity被销毁，或另一个Activity（一个现有Activity或新Activity）继续执行并将其覆盖，就可能发生这种情况。如果Activity恢复与用户的交互，则后接`onRestart()`，如果Activity被销毁，则后接`onDestroy()`。                                                    |
| **onDestroy()** | 在Activity被销毁前调用。这是Activity将收到的最后调用。 当Activity结束（有人调用Activity上的`finish()`），或系统为节省空间而暂时销毁该Activity实例时，可能会调用它。 您可以通过`isFinishing()`方法区分这两种情形。                                                                        |

### 启动方式

### Task & Back Stack

---

## Intent
Intent 是一个消息传递对象，您可以使用它从其他应用组件请求操作。尽管 Intent 可以通过多种方式促进组件之间的通信，但其基本用例主要包括以下三个：
- 启动Activity
- 启动服务
- 传递广播

### 显式Intent
按名称（完全限定类名）指定要启动的组件。通常，您会在自己的应用中使用显式Intent来启动组件，这是因为您知道要启动的Activity或服务的类名。
例如，启动新Activity以响应用户操作，或者启动服务以在后台下载文件。

### 隐式Intent
不会指定特定的组件，而是声明要执行的常规操作，从而允许其他应用中的组件来处理它。
例如，如需在地图上向用户显示位置，则可以使用隐式Intent，请求另一具有此功能的应用在地图上显示指定的位置。

![隐式Intent如何通过系统传递以启动其他Activity的图解](https://developer.android.com/images/components/intent-filters@2x.png)

---

## UI
### 布局
#### LinearLayout

#### RelativeLayout

#### TableLayout

#### FrameLayout

## 组件
#### Input
- Button
- EditText
- CheckBox
- ToggleButton
- Spinner
- TimePickerDialog

#### Menu

#### Dialog

#### Toast

#### 自定义组件
