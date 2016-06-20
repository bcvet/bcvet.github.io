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
- [官方文档-Activity](https://developer.android.com/guide/components/activities.html)
- [官方问题-在manifest中配置Activity](https://developer.android.com/guide/topics/manifest/activity-element.html)

### 生命周期
#### Activity的三种状态
- `已继续-resumed` Activity位于屏幕前台并具有用户焦点。（有时也将此状态称作“运行中”）
- `已暂停-paused` 另一个Activity位于屏幕前台并具有用户焦点，但此Activity仍可见。也就是说，另一个 Activity 显示在此Activity上方，并且该Activity部分透明或未覆盖整个屏幕。已暂停的Activity处于完全Activity状态（Activity 对象保留在内存中，它保留了所有状态和成员信息，并与窗口管理器保持连接），但在内存极度不足的情况下，可能会被系统终止。
- `已停止-stopped` 该Activity被另一个Activity完全遮盖（该Activity目前位于“后台”）。已停止的Activity同样仍处于Activity状态（Activity 对象保留在内存中，它保留了所有状态和成员信息，但未与窗口管理器连接）。不过，它对用户不再可见，在他处需要内存时可能会被系统终止。
- 其它状态 (`created`与`started`)都是短暂的，系统快速的执行那些回调函数并通过执行下一阶段的回调函数移动到下一个状态。也就是说，在系统调用`onCreate()`, 之后会迅速调用`onStart()`, 之后再迅速执行`onResume(`)。

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

![金字塔模型](http://hukai.me/android-training-course-in-chinese/basics/activity-lifecycle/basic-lifecycle.png)

#### 生命周期详解
| 方法             | 说明                                                                                                                                                                                                                                                                  |
|-----------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **onCreate()**  | 首次创建Activity时调用。 您应该在此方法中执行所有正常的静态设置— 创建视图、将数据绑定到列表等等。系统向此方法传递一个Bundle对象，其中包含Activity的上一状态，不过前提是捕获了该状态。始终后接`onStart()`。                                                                                   |
| **onRestart()** | 在Activity已停止并即将再次启动前调用。始终后接`onStart()`                                                                                                                                                                                                              |
| **onStart()**   | 在Activity即将对用户可见之前调用。如果Activity转入前台，则后接`onResume()`，如果Activity转入隐藏状态，则后接`onStop()`。                                                                                                                                                     |
| **onResume()**  | 在Activity即将开始与用户进行交互之前调用。此时，Activity处于Activity堆栈的顶层，并具有用户输入焦点。始终后接`onPause()`。                                                                                                                                                      |
| **onPause()**   | 当系统即将开始继续另一个Activity时调用。此方法通常用于确认对持久性数据的未保存更改、停止动画以及其他可能消耗CPU的内容，诸如此类。 它应该非常迅速地执行所需操作，因为它返回后，下一个Activity才能继续执行。如果Activity返回前台，则后接`onResume()`，如果Activity转入对用户不可见状态，则后接`onStop()`。 |
| **onStop()**    | Activity对用户不再可见时调用。如果Activity被销毁，或另一个Activity（一个现有Activity或新Activity）继续执行并将其覆盖，就可能发生这种情况。如果Activity恢复与用户的交互，则后接`onRestart()`，如果Activity被销毁，则后接`onDestroy()`。                                                    |
| **onDestroy()** | 在Activity被销毁前调用。这是Activity将收到的最后调用。 当Activity结束（有人调用Activity上的`finish()`），或系统为节省空间而暂时销毁该Activity实例时，可能会调用它。 您可以通过`isFinishing()`方法区分这两种情形。                                                                        |

#### 示例
生命周期回调的顺序经过明确定义，当两个Activity位于同一进程，并且由一个Activity启动另一个Activity时，其定义尤其明确。
以下是当 Activity A 启动 Activity B 时一系列操作的发生顺序：
1. Activity A的`onPause()`方法执行。
2. Activity B的`onCreate()`、`onStart()`和`onResume()`方法依次执行。（Activity B现在具有用户焦点。）
3. 如果Activity A在屏幕上不再可见，则其`onStop()`方法执行。
可以利用这种可预测的生命周期回调顺序管理从一个Activity到另一个Activity的信息转变。
例如，如果必须在第一个Activity停止时向数据库写入数据，以便下一个Activity能够读取该数据，则应在`onPause()`而不是`onStop()`执行期间向数据库写入数据。

#### 保存/恢复Activity状态
当我们的Activity开始Stop，系统会调用`onSaveInstanceState()`，Activity可以用键值对的集合来保存状态信息。这个方法会默认保存Activity视图的状态信息，如在EditText组件中的文本或ListView的滑动位置。
为了给Activity保存额外的状态信息，你必须实现`onSaveInstanceState()`并增加key-value pairs到Bundle对象中:

    static final String STATE_SCORE = "playerScore";
    static final String STATE_LEVEL = "playerLevel";
    ...

    @Override
    public void onSaveInstanceState(Bundle savedInstanceState) {
        // Save the user's current game state
        savedInstanceState.putInt(STATE_SCORE, mCurrentScore);
        savedInstanceState.putInt(STATE_LEVEL, mCurrentLevel);

        // Always call the superclass so it can save the view hierarchy state
        super.onSaveInstanceState(savedInstanceState);
    }

当Activity从Destory中重建，我们可以从系统传递的Activity的Bundle中恢复保存的状态。`onCreate()`与`onRestoreInstanceState()`回调方法都接收到了同样的Bundle，里面包含了同样的实例状态信息。
由于`onCreate()`方法会在第一次创建新的Activity实例与重新创建之前被Destory的实例时都被调用，我们必须在尝试读取Bundle对象前检测它是否为null。如果它为null，系统则是创建一个新的Activity实例，而不是恢复之前被Destory的Activity。

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState); // Always call the superclass first

        // Check whether we're recreating a previously destroyed instance
        if (savedInstanceState != null) {
            // Restore value of members from saved state
            mCurrentScore = savedInstanceState.getInt(STATE_SCORE);
            mCurrentLevel = savedInstanceState.getInt(STATE_LEVEL);
        } else {
            // Probably initialize members with default values for a new instance
        }
        ...
    }

### Task & Back Stack
**任务**是指在执行特定作业时与用户交互的一系列Activity。这些Activity按照各自的打开顺序排列在堆栈（即`返回栈`）中。

![堆栈的运行方式](https://developer.android.com/images/fundamentals/diagram_backstack.png)

由于返回栈中的Activity永远不会重新排列，因此如果应用允许用户从多个Activity中启动特定Activity，则会创建该Activity的新实例并推入堆栈中（而不是将Activity的任一先前实例置于顶部）。
因此，应用中的一个Activity可能会多次实例化（即使Activity来自不同的任务）。
如果用户使用“返回”按钮向后导航，则会按Activity每个实例的打开顺序显示这些实例。如果不希望Activity多次实例化，则可修改此行为。

![](https://developer.android.com/images/fundamentals/diagram_multiple_instances.png)

### Activity的启动方式
在AndroidManifest文件中,可以通过`launchMode`属性指定Activity通过哪种方式启动:
- `standard`
- `singleTop` 当启动Activity时,如果发现返回栈的栈顶已经是该活动,则可以直接使用它,不会创建新的活动实例.当Activity并未处于栈顶时,启动Activity,仍会创建新的实例.
- `singleTask` 每次启动Activity时,会检查返回栈中是否存在该Activity的实例,如果发现已经存在则直接使用该实例,并把这个Activity之上的Activity全部出栈.如果没有发现,则创建一个新的Activity.
- `singleInstance` 如果需要创建一个多个进程共享的Activity,那么上述三种方式都不合适.`singleInstance`会创建一个单独的返回栈,栈中只有一个这一个Activity.

---

---

## Intent
Intent 是一个消息传递对象，您可以使用它从其他应用组件请求操作。尽管 Intent 可以通过多种方式促进组件之间的通信，但其基本用例主要包括以下三个：
- 启动Activity
- 启动服务
- 传递广播

### 显式Intent
按名称（完全限定类名）指定要启动的组件。通常，您会在自己的应用中使用显式Intent来启动组件，这是因为您知道要启动的Activity或服务的类名。
例如，启动新Activity以响应用户操作，或者启动服务以在后台下载文件。

    Intent intent = new Intent(MainActivity.this, NextActivity.class);
    intent.putExtra("message", editMessage);
    startActivity(intent);

### 隐式Intent
不会指定特定的组件，而是声明要执行的常规操作，从而允许其他应用中的组件来处理它。
例如，如需在地图上向用户显示位置，则可以使用隐式Intent，请求另一具有此功能的应用在地图上显示指定的位置。

例如，如果您希望用户与他人共享您的内容，请使用`ACTION_SEND`操作创建Intent，并添加指定共享内容的Extra。使用该Intent调用`startActivity()`时，用户可以选取共享内容所使用的应用。

> 警告：用户可能没有任何应用处理您发送到`startActivity()`的隐式Intent。如果出现这种情况，则调用将会失败，且应用会崩溃。要验证Activity是否会接收Intent，请对Intent对象调用`resolveActivity()`。如果结果为非空，则至少有一个应用能够处理该Intent，且可以安全调用`startActivity()`。如果结果为空，则不应使用该Intent。如有可能，您应禁用发出该Intent的功能。

    // Create the text message with a string
    Intent sendIntent = new Intent();
    sendIntent.setAction(Intent.ACTION_SEND);
    sendIntent.putExtra(Intent.EXTRA_TEXT, textMessage);
    sendIntent.setType("text/plain");

    // Verify that the intent will resolve to an activity
    if (sendIntent.resolveActivity(getPackageManager()) != null) {
        startActivity(sendIntent);
    }

调用`startActivity()`时，系统将检查已安装的所有应用，确定哪些应用能够处理这种Intent。如果只有一个应用能够处理，则该应用将立即打开并提供给Intent。如果多个Activity接受 Intent，则系统将显示一个对话框，使用户能够选取要使用的应用。

![隐式Intent如何通过系统传递以启动其他Activity的图解](https://developer.android.com/images/components/intent-filters@2x.png)

#### 接收隐式Intent
要公布应用可以接收哪些隐式Intent，请在清单文件中使用`<intent-filter>`元素为每个应用组件声明一个或多个Intent过滤器。每个Intent过滤器均根据Intent的操作、数据和类别指定自身接受的Intent类型。
仅当隐式Intent可以通过Intent过滤器之一传递时，系统才会将该Intent传递给应用组件。

在<intent-filter>内部，可以使用以下三个元素中的一个或多个指定要接受的Intent类型：
- `action` 声明操作类型
- `data` 声明接受的数据类型
- `category` 声明接受的Intent类别

    <activity android:name="MainActivity">
        <!-- This activity is the main entry, should appear in app launcher -->
        <intent-filter>
            <action android:name="android.intent.action.MAIN" />
            <category android:name="android.intent.category.LAUNCHER" />
        </intent-filter>
    </activity>

    <activity android:name="ShareActivity">
        <!-- This activity handles "SEND" actions with text data -->
        <intent-filter>
            <action android:name="android.intent.action.SEND"/>
            <category android:name="android.intent.category.DEFAULT"/>
            <data android:mimeType="text/plain"/>
        </intent-filter>
        <!-- This activity also handles "SEND" and "SEND_MULTIPLE" with media data -->
        <intent-filter>
            <action android:name="android.intent.action.SEND"/>
            <action android:name="android.intent.action.SEND_MULTIPLE"/>
            <category android:name="android.intent.category.DEFAULT"/>
            <data android:mimeType="application/vnd.google.panorama360+jpg"/>
            <data android:mimeType="image/*"/>
            <data android:mimeType="video/*"/>
        </intent-filter>
    </activity>

第一个Activity MainActivity是应用的主要入口点。当用户最初使用启动器图标启动应用时，该 Activity 将打开：
- `ACTION_MAIN`操作指示这是主要入口点，且不要求输入任何Intent数据。
- `CATEGORY_LAUNCHER`类别指示此`Activity`的图标应放入系统的应用启动器。如果<activity>元素未使用`icon`指定图标，则系统将使用<application/>元素中的图标。
这两个元素必须配对使用，Activity才会显示在应用启动器中。

第二个Activity ShareActivity旨在便于共享文本和媒体内容。尽管用户可以通过从MainActivity导航进入此Activity，但也可以从发出隐式Intent（与两个Intent过滤器之一匹配）的另一应用中直接进入ShareActivity。

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
