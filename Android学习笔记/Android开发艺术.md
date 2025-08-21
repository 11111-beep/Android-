# Android开发艺术





## Activity的生命周期和启动模式



### Android生命周期全面分析

主要的 Activity 生命周期回调方法包括：

1. **`onCreate()`**
   - **何时调用：** 当 Activity 第一次被创建时调用。这是 Activity 生命周期的第一个回调，也是唯一必须实现的回调。
   - **用途：** 进行 Activity 的初始化设置，例如设置布局（`setContentView()`）、初始化视图、绑定数据到列表、创建数据库连接等。在此方法中，可以从 `Bundle` 参数中恢复 Activity 之前的保存状态。
   - **状态：** Activity 进入 **Created** 状态。
2. **`onStart()`**
   - **何时调用：** 当 Activity 即将对用户可见时调用。
   - **用途：** 准备 Activity 对用户可见，例如注册广播接收器、启动动画等。
   - **状态：** Activity 进入 **Started** 状态。
3. **`onResume()`**
   - **何时调用：** 当 Activity 即将与用户开始交互时调用。此时 Activity 位于 Activity 栈的顶部，并且能够接收用户输入。
   - **用途：** 启动需要与用户交互的组件，例如启动相机预览、播放视频、恢复动画等。这是一个 Activity 处于前台并可交互的状态。
   - **状态：** Activity 进入 **Resumed**（或 Running）状态。
4. **`onPause()`**
   - **何时调用：** 当 Activity 失去焦点或即将被另一个 Activity 部分覆盖时调用（例如，弹出对话框或启动另一个 Activity，但新 Activity 不是全屏）。
   - **用途：** 暂停不必要的 UI 更新、释放 CPU 密集型资源（例如停止动画、保存数据到永久存储）、解除注册广播接收器等。**此方法应该执行得非常快，因为下一个 Activity 不会创建，直到当前 Activity 的 `onPause()` 方法返回。**
   - **状态：** Activity 进入 **Paused** 状态。
5. **`onStop()`**
   - **何时调用：** 当 Activity 完全不可见时调用。这可能是因为用户导航到另一个 Activity、返回主屏幕或销毁了 Activity。
   - **用途：** 释放 Activity 不再需要的资源，例如停止后台线程、关闭数据库游标等。系统可能会在内存不足的情况下直接杀死进程，而不再调用 `onStop()`，所以重要的持久化数据应该在 `onPause()` 中保存。
   - **状态：** Activity 进入 **Stopped** 状态。
6. **`onRestart()`**
   - **何时调用：** 当 Activity 停止后，用户又重新回到该 Activity 时调用。它总是在 `onStart()` 之前调用。
   - **用途：** 执行一些只有在 Activity 从停止状态重新启动时才需要的操作。
7. **`onDestroy()`**
   - **何时调用：** 在 Activity 被销毁之前调用。这是 Activity 生命周期的最后一个回调。
   - **用途：** 释放 Activity 持有的所有资源，例如解除所有绑定、关闭所有线程等。
   - 销毁的原因：
     - 用户按下了返回键，或者调用了 `finish()` 方法。
     - 系统由于配置变更（如屏幕旋转）而临时销毁并重建 Activity。
     - 系统为了回收内存而销毁 Activity 所在的进程。
   - **状态：** Activity 进入 **Destroyed** 状态。

**Activity 生命周期图示：**

通常可以想象为一个流程图，Activity 在不同的状态之间转换：



```表
                                +---------------------+
                                |      App 启动        |
                                +---------------------+
                                          |
                                          V
                                +---------------------+
            (首次创建)           |    onCreate()       |
                                +---------------------+
                                          |
                                          V
                                +---------------------+
                                |     onStart()       |
                                +---------------------+
                                          |
                                          V
                                +---------------------+
                                |    onResume()       |
                                +---------------------+
                                          |
                            (Activity 运行中，处于前台，可交互)
                                          |
                  --------------------------------------------------
                  |                                                |
                  V                                                V
        +---------------------+                          +---------------------+
        |     onPause()       | <------------------------|     onRestart()     |
        +---------------------+                          +---------------------+
                  |                                                ^
                  | (Activity 部分不可见或失去焦点)                   | (用户从停止状态返回)
                  V                                                |
        +---------------------+                                    |
        |     onStop()        |------------------------------------
        +---------------------+
                  |
                  | (Activity 完全不可见，可能被系统回收)
                  V
        +---------------------+
        |    onDestroy()      |
        +---------------------+
                  |
                  V
        +---------------------+
        |     Activity 销毁    |
        +---------------------+
```





1.`onStart`的时候Activity在后台,`onResume`时Activity才显示到前台.

2.`onStart`和`onStop`是从Activity是否可见这个角度来回调的,而`onResume`和`onPause`是从Activity是否位于前台这个角度来回调的.

3.当启动一个Activity的时候,旧Activity的`onPause`会先执行,然后才会启动新的Activity.



---



### 异常情况下的生命周期分析



#### `onSaveInstanceState(Bundle outState)`

1. **何时调用：**

   - 这个方法是在 Activity 即将**被系统杀死或销毁**时调用，目的是为了给 Activity 一个机会来保存其 UI 状态，以便在将来 Activity 被重新创建时能够恢复这些状态。
   - 它总是在 `onStop()` 之前调用。具体来说，通常在 `onPause()` 之后、`onStop()` 之前。
   - 触发场景：
     - **配置变更：** 最常见的场景，例如屏幕旋转、键盘可用性变化、语言切换、多窗口模式进入等。
     - **内存不足：** 当系统内存不足时，可能会杀死后台的 Activity 进程以释放内存。
     - **Activity 被覆盖：** 例如，用户从你的 Activity 启动了一个新的全屏 Activity，并且新 Activity 完全覆盖了你的 Activity。如果系统需要回收内存，你的 Activity 可能被杀死。
     - **用户按下 Home 键：** Activity 进入后台，系统可能在内存紧张时杀死它。

2. **作用：**  

   - 通过将数据存储在  `Bundle` 对象 (`outState`) 中onSaveInstanceState()允许你保存 Activity 的瞬时状态信息。这些信息可以是：
     - 用户在 `EditText` 中输入的数据（系统会自动保存大部分 View 的状态，但自定义 View 可能需要手动处理）。
     - 列表的滚动位置。
     - 当前选中的 Tab 页。
     - 应用程序的临时数据，例如用户正在编辑的草稿内容。
   - **它不是用来保存持久化数据（如数据库数据、用户设置）的。** 持久化数据应该在 `onPause()` 或 `onStop()` 中保存到数据库、文件或 SharedPreferences。

3. **使用示例：**

   

   ```Java
   @Override
   protected void onSaveInstanceState(Bundle outState) {
       super.onSaveInstanceState(outState); // 总是调用父类方法来保存视图层次结构的状态
   
       // 保存一个自定义的整型变量
       outState.putInt("my_integer_value", myIntegerValue);
   
       // 保存一个字符串
       outState.putString("user_input_text", myEditText.getText().toString());
   
       // 保存一个布尔值
       outState.putBoolean("is_checkbox_checked", myCheckbox.isChecked());
   
       // 如果有自定义的Parcelable或Serializable对象，也可以保存
       // outState.putParcelable("my_custom_object", myCustomObject);
   }
   ```

#### `onRestoreInstanceState(Bundle savedInstanceState)`

1. **何时调用：**

   - 这个方法只会在 Activity **被系统销毁后又重新创建时**调用（即 `onSaveInstanceState()` 被调用过，并且 Activity 进程被杀死后又恢复）。
   - 它在 `onStart()` 之后、`onResume()` 之前被调用。
   - 如果 Activity 是用户正常退出（例如按返回键，或调用 `finish()`），那么 `onSaveInstanceState()` 不会被调用，`onRestoreInstanceState()` 当然也不会被调用。

2. **作用：**

   - 从传入的 `Bundle` (`savedInstanceState`) 中恢复之前在 `onSaveInstanceState()` 中保存的数据。
   - **注意：** 通常情况下，**更推荐在 `onCreate()` 方法中恢复状态**，因为 `onCreate()` 总是会在 Activity 重新创建时被调用（包括首次创建和系统恢复）。在 `onCreate()` 中，你可以检查 `savedInstanceState` 是否为 `null` 来判断 Activity 是首次创建还是被系统恢复。

3. **使用示例（推荐在 `onCreate` 中恢复）：**

   

   ```Java
   @Override
   protected void onCreate(Bundle savedInstanceState) {
       super.onCreate(savedInstanceState);
       setContentView(R.layout.activity_main);
   
       // 首次创建时 savedInstanceState 为 null
       if (savedInstanceState != null) {
           // 从 Bundle 中恢复数据
           int restoredValue = savedInstanceState.getInt("my_integer_value");
           String restoredText = savedInstanceState.getString("user_input_text");
           boolean restoredChecked = savedInstanceState.getBoolean("is_checkbox_checked");
   
           // 将恢复的数据应用到 UI 或变量中
           myIntegerValue = restoredValue;
           myEditText.setText(restoredText);
           myCheckbox.setChecked(restoredChecked);
       }
   
       // 其他初始化代码
       // ...
   ```

#### 异常情况下的 Activity 生命周期图表



当 Android Activity 遇到配置变更（如屏幕旋转）或因系统内存不足被销毁并重建时，其生命周期会遵循一个特定的流程。以下是这个异常情况下的生命周期图表，着重展示了 `onSaveInstanceState()` 和 `onRestoreInstanceState()` 的位置和作用。



```表
                                      +---------------------+
                                      |    Activity A       |
                                      |    (正在运行中)       |
                                      +---------------------+
                                                |
                                                | (用户旋转屏幕 / 系统内存不足)
                                                V
                                      +---------------------+
                                      |     onPause()       |
                                      +---------------------+
                                                |
                                                V
                                      +---------------------+
                                      |onSaveInstanceState()|  <-- 保存临时 UI 状态到 Bundle
                                      +---------------------+
                                                |
                                                V
                                      +---------------------+
                                      |     onStop()        |
                                      +---------------------+
                                                |
                                                | (系统销毁旧 Activity 实例)
                                                V
                                      +---------------------+
                                      |     onDestroy()     |
                                      |(isFinishing() = false)|
                                      +---------------------+
                                                |
                                                | (系统创建新的 Activity 实例)
                                                V
                                      +---------------------+
                                      |     onCreate()      |  <-- 从 Bundle 恢复状态 (推荐)
                                      | (参数 savedInstanceState 非 null) |
                                      +---------------------+
                                                |
                                                V
                                      +---------------------+
                                      |     onStart()       |
                                      +---------------------+
                                                |
                                                | (可选，如果未在 onCreate 恢复)
                                                V
                                      +---------------------+
                                      | onRestoreInstanceState()| <-- 从 Bundle 恢复状态
                                      +---------------------+
                                                |
                                                V
                                      +---------------------+
                                      |     onResume()      |
                                      +---------------------+
                                      |    Activity A       |
                                      |   (在新配置下运行)    |
                                      +---------------------+
```







当Activity被重新创建后,系统会调用`onRestoreInStance`,并且把Activity销毁时`onSaveInstanceState方法`所保存的`Bundle`对象作为参数同时传递给`onRestoreInstanceState`和`onCreate`方法.



当系统内容发生改变后,如果我们不想系统重新创建Activity,可以给Activity指定`configChanges`属性.



---



### Activity的启动模式



#### standard (标准模式)

- 特点：这是 Activity 的默认启动模式。
  - **每次启动都会创建新的实例。** 无论目标 Activity 是否已经在任务栈中存在，系统都会为它创建一个新的实例，并将其推入启动它的任务栈的顶部。
  - **多实例：** 一个任务栈中可以有多个同一 Activity 的实例。
  - **私有栈：** 每个实例都有自己的生命周期，并且都位于启动它的任务栈中。
- **回退栈行为：** 当用户按下返回键时，会依次销毁栈顶的 Activity 实例，直到栈空。
- **适用场景：** 绝大多数 Activity 都应该使用 `standard` 模式。例如，显示文章内容、图片详情、用户表单等，每次打开都希望是一个独立的实例。
- 示例：
  - 应用 A 启动 Activity B (standard) -> 栈：A -> B
  - Activity B 再次启动 Activity B (standard) -> 栈：A -> B -> B' (B的另一个实例)
  - Activity B 启动 Activity C (standard) -> 栈：A -> B -> C

#### singleTop (栈顶单例模式)

- 特点：
  - **如果目标 Activity 实例已经在任务栈的顶部**，系统将不会创建新的实例，而是直接重用栈顶的现有实例。
  - 系统会调用该现有实例的 `onNewIntent(Intent intent)` 方法，并将新的 Intent 对象传递给它。
  - **如果目标 Activity 实例不在栈顶**（即使它在栈中其他位置存在），系统仍然会创建新的实例并推入栈顶。
  - **多实例可能：** 即使是 `singleTop` 模式，如果实例不在栈顶，仍然可能创建多个实例。
- **回退栈行为：** 与 `standard` 类似，按返回键销毁栈顶 Activity。
- **适用场景：** 接收推送通知后打开的 Activity、消息中心 Activity、搜索结果页等。当你希望避免重复显示相同的界面时，但又不希望强制该 Activity 只有唯一实例时使用。
- 示例：
  - 栈：A -> B -> C (C 是 singleTop)
  - Activity C 再次启动 Activity C -> 栈：A -> B -> C (不会创建新实例，C 的 `onNewIntent()` 被调用)
  - 栈：A -> B -> C -> D (C 是 singleTop)
  - Activity D 启动 Activity C -> 栈：A -> B -> C -> D -> C' (C 不在栈顶，创建新实例 C')

#### singleTask (任务栈单例模式)

- 特点：
  - 如果目标 Activity 实例已经在任何任务栈中存在，系统会先寻找包含该实例的任务栈。
    - 如果找到了，系统会将该任务栈移到前台。
    - 然后，该 Activity 实例上方的所有 Activity 都将被销毁（`onPause()` -> `onStop()` -> `onDestroy()`），使该 Activity 实例成为栈顶。
    - 系统会调用该现有实例的 `onNewIntent(Intent intent)` 方法。
  - **如果目标 Activity 实例不存在**，系统会创建一个新的实例，并将其放置在一个新的任务栈中（如果指定了 `taskAffinity` 且与当前任务栈不同），或者放置在当前任务栈中（如果 `taskAffinity` 相同）。
  - **保证单例：** 在整个系统中，同一 `singleTask` 模式的 Activity 只会存在一个实例。
  - **新任务栈：** 如果启动时没有找到现有的实例，并且 Activity 有一个与当前任务栈不同的 `taskAffinity`，那么它可能会在一个新的任务栈中创建。
- **回退栈行为：** 当 `singleTask` Activity 成为栈顶时，其上方的 Activity 都会被清除。按返回键会销毁它，并显示它下面的 Activity。
- **适用场景：** 应用的主页（Home Screen）、某些全局设置页面、只有一个实例的特定功能页面（如播放器、购物车等）。当你希望该 Activity 总是作为其任务栈的根 Activity，并且在整个应用中只有一个实例时使用。
- 示例：
  - 假设应用 A 启动 Activity X (singleTask)
    - 栈：A -> B -> C
    - C 启动 X -> 栈：A -> B -> X (C 被销毁，X 上方的都被销毁)
    - 如果 X 不在栈中，并且 X 属于一个新的任务栈，则会创建新栈：X (并作为根 Activity)
  - 假设 Activity X 有一个不同的` taskAffinity1`
    - 栈1: A -> B
    - B 启动 X -> 栈1: A -> B, 栈2: X (如果 X 的 taskAffinity 不同，且没有找到现有实例)

#### singleInstance (全局单例模式)

- 特点：
  - **最严格的单例模式。** 该 Activity 实例将是整个系统中唯一的实例，并且它**只会在一个新的任务栈中创建**，该任务栈中将且仅会包含这一个 Activity 实例。
  - **独立任务栈：** 无论从哪个应用或 Activity 启动它，它都将独立在一个新的任务栈中。
  - **保证唯一实例和唯一任务栈：** 它的任务栈里永远只有它自己一个 Activity。
  - 如果再次启动，它会调用现有实例的 `onNewIntent(Intent intent)` 方法。
- **回退栈行为：** 当用户从这个 `singleInstance` Activity 返回时，会回到启动它的 Activity 所在的任务栈。该 `singleInstance` Activity 不会被销毁，除非通过 `finish()` 显式关闭。
- **适用场景：** 系统级别的应用（如来电显示界面），或任何你需要确保在整个系统中只有一个实例且完全独立于其他 Activity 的功能。由于其强隔离性，使用场景相对较少。
- 示例：
  - 栈1: A -> B
  - B 启动 Activity X (singleInstance)
    - 栈1: A -> B
    - 栈2: X (X 独立在一个新栈中)
  - 再从 Activity C (在任何其他应用或任务栈中) 启动 Activity X
    - 栈1: A -> B
    - 栈2: X (X 的 `onNewIntent()` 被调用，不会创建新实例)



| 启动模式           | 创建新实例   | 允许多实例   | 影响回退栈            | onNewIntent() 调用 | 适用场景                             |
| ------------------ | ------------ | ------------ | --------------------- | ------------------ | ------------------------------------ |
| **standard**       | 总是         | 是           | 推入栈顶              | 否                 | 默认行为，大部分 Activity            |
| **singleTop**      | 栈顶不存在时 | 栈顶以外可能 | 栈顶存在时重用        | 栈顶存在时         | 避免重复显示栈顶界面，如消息通知     |
| **singleTask**     | 实例不存在时 | 整个系统唯一 | 清除其上所有 Activity | 实例存在时         | 应用主页，唯一的功能入口，如购物车   |
| **singleInstance** | 实例不存在时 | 整个系统唯一 | 创建新独立任务栈      | 实例存在时         | 系统级组件，需完全隔离的唯一功能实例 |



---





### IntentFilter的匹配规则(隐式Intent)



`IntentFilter` 是 Android 中一个非常重要的概念，它定义了一个组件（如 Activity、Service、BroadcastReceiver）能够响应哪些类型的 `Intent`。当一个 `Intent` 被发送出去时，系统会尝试找到能够匹配该 `Intent` 的组件。`IntentFilter` 的匹配规则就是系统进行匹配的依据。

一个 `IntentFilter` 通常包含以下几种元素，它们共同决定了匹配规则：

1. **`<action>` (动作)**
2. **`<category>` (类别)**
3. **`<data>` (数据)**

要使一个 `Intent` 能够成功启动一个组件，该 `Intent` 必须**同时通过**组件 `IntentFilter` 中所有这三个元素的匹配规则。如果一个 `IntentFilter` 中不声明某个元素（例如没有 `<data>` 标签），则表示该元素对匹配没有限制。

#### Action (动作) 匹配规则

- **定义：** `action` 描述了 `Intent` 要执行的通用动作。例如，`ACTION_VIEW` (查看)、`ACTION_EDIT` (编辑)、`ACTION_MAIN` (作为应用入口)。

- 匹配要求：

  - `Intent` 中声明的 `action` 必须**至少有一个**与 `IntentFilter` 中声明的 `action` **完全相同**。
  - 如果 `Intent` **没有指定 action**，那么只有当 `IntentFilter` 中**没有声明任何 action** 时，才能匹配成功。（实际开发中，Intent 通常会指定 action）。

- 示例：

  ```XML
  <intent-filter>
      <action android:name="android.intent.action.VIEW" />
      <action android:name="com.example.MY_CUSTOM_ACTION" />
      </intent-filter>
  ```

  - `Intent` 带有 `android.intent.action.VIEW` → **匹配成功**
  - `Intent` 带有 `com.example.MY_CUSTOM_ACTION` → **匹配成功**
  - `Intent` 带有 `android.intent.action.DIAL` → **匹配失败**

#### Category (类别) 匹配规则

- **定义：** `category` 为 `Intent` 提供了额外的分类信息，表明它所处的环境或特性。例如，`CATEGORY_DEFAULT` (默认类别)、`CATEGORY_BROWSABLE` (可被浏览器安全调用的)、`CATEGORY_LAUNCHER` (应用启动器入口)。

- 匹配要求：

  - `Intent` 中声明的**所有 category** 都必须在 `IntentFilter` 中有对应的声明。换句话说，`Intent` 中有多少个 category，`IntentFilter` 中就必须至少有多少个完全匹配的 category。
  - `IntentFilter` 中可以有比 `Intent` 更多的 category，这没关系。
  - **特殊规则：** 即使 `Intent` 对象中没有显式设置任何 category，系统也会自动为所有隐式 `Intent` 添加 `CATEGORY_DEFAULT` 这个 category。因此，为了能够接收隐式 `Intent`，**Activity 的 `IntentFilter` 必须包含 `CATEGORY_DEFAULT` 类别。** （注意：`startActivity()` 方法会为隐式 `Intent` 自动添加 `CATEGORY_DEFAULT`；`startActivityForResult()` 不会自动添加。）

- 示例：

  ```XML
  <intent-filter>
      <category android:name="android.intent.category.DEFAULT" />
      <category android:name="android.intent.category.BROWSABLE" />
      </intent-filter>
  ```

  - `Intent` 没有设置 category (被系统添加 `CATEGORY_DEFAULT`) → **匹配成功** (因为 `IntentFilter` 包含 `CATEGORY_DEFAULT`)
  - `Intent` 包含 `CATEGORY_DEFAULT` 和 `CATEGORY_BROWSABLE` → **匹配成功**
  - `Intent` 包含 `CATEGORY_DEFAULT` 和 `CATEGORY_ALTERNATIVE` → **匹配失败** (因为 `IntentFilter` 没有 `CATEGORY_ALTERNATIVE`)
  - `Intent` 只包含 `CATEGORY_BROWSABLE` (没有 `CATEGORY_DEFAULT`) → **匹配失败** (因为隐式 Intent 始终携带 `CATEGORY_DEFAULT`，而 IntentFilter 没有它)

#### Data (数据) 匹配规则

- **定义：** `data` 描述了 `Intent` 要操作的数据。它通常由两部分组成：

  - **URI (统一资源标识符)：** 包括 `scheme` (协议，如 `http`, `content`, `file`)、`host` (主机名)、`port` (端口)、`path` (路径)、`pathPrefix` (路径前缀)、`pathPattern` (路径模式)。
  - **MIME 类型 (媒体类型)：** 如 `image/jpeg`, `text/plain`, `application/pdf`。

- **匹配要求：**

  - 如果 `IntentFilter` 没有指定任何 `<data>` 标签，则只有当 `Intent` 也**没有指定数据 URI 和 MIME 类型**时才能匹配成功。
  - 如果 `IntentFilter` 指定了 `<data>` 标签，那么 `Intent` 必须至少满足 `IntentFilter` 中某一个 `<data>` 标签的所有子部分的匹配。
  - URI 部分匹配：
    - `scheme`：必须完全匹配。如果 `IntentFilter` 没有声明 `scheme`，则 `Intent` 必须也没有 `scheme`。
    - `host`：如果 `scheme` 存在，`host` 也必须存在并完全匹配。
    - `port`：如果 `host` 和 `port` 都存在，则必须完全匹配。
    - `path`/`pathPrefix`/`pathPattern`：如果 `IntentFilter` 声明了这些，则 `Intent` 的 path 必须符合这些规则。
  - MIME 类型匹配：
    - `Intent` 的 MIME 类型必须与 `IntentFilter` 中声明的 MIME 类型**完全匹配**。
    - `IntentFilter` 可以使用通配符 `*`，例如 `image/*` 可以匹配 `image/jpeg` 和 `image/png`。
      - 如果 `Intent` **没有指定 MIME 类型**，那么只有当 `IntentFilter` 中也**没有声明 MIME 类型**，或者 `IntentFilter` 声明了 MIME 类型但其 scheme 是 `file://` 或 `content://` 且 Intent 只有 `scheme` 没有数据时，才能匹配。（复杂，通常 Intent 会同时带 URI 和 MIME 类型）。

- **示例：**

  ```XML
  <intent-filter>
      <data android:scheme="http" android:host="www.example.com" android:pathPrefix="/users/"                    android:mimeType="text/html" />
      <data android:scheme="content" android:mimeType="image/*" />
  </intent-filter>
  ```
  
  - `Intent` data: `http://www.example.com/users/profile.html`, MIME: `text/html` → **匹配成功**
  - `Intent` data: `content://media/external/images/media/123`, MIME: `image/jpeg` → **匹配成功**
  - `Intent` data: `http://www.anothersite.com`, MIME: `text/html` → **匹配失败** (host 不匹配)
  - `Intent` data: `http://www.example.com/products/`, MIME: `text/html` → **匹配失败** (pathPrefix 不匹配)
  - `Intent` data: `file:///sdcard/image.png`, MIME: `image/png` → **匹配失败** (scheme 不匹配第二个 `<data>` 标签，MIME 不匹配第一个 `<data>` 标签)

```java
 @SuppressLint("UnsafeImplicitIntentLaunch") 
 Intent intent = new Intent("android.intent.action.FACTORY_TEST");
 intent.addCategory("android.intent.category.DEFAULT");
 intent.setDataAndType(Uri.parse("content://media/external/images/media/123"), "image/jpeg");
 startActivity(intent);
```





1. **Action 匹配：** `Intent` 的 Action 必须与 `IntentFilter` 中的至少一个 Action 完全匹配。
2. **Category 匹配：** `Intent` 中所有的 Category 都必须在 `IntentFilter` 中存在。**并且，对于隐式 Intent，系统会自动添加 `CATEGORY_DEFAULT`，所以 `IntentFilter` 必须包含 `CATEGORY_DEFAULT`。**
3. **Data 匹配：**
   - 如果 `IntentFilter` 声明了 `<data>`，则 `Intent` 必须有匹配的 URI（`scheme`、`host`、`port`、`path`）和 MIME 类型。
   - 如果 `IntentFilter` 没有声明 `<data>`，则 `Intent` 也不能有 URI 和 MIME 类型。

只有当一个 `Intent` **同时满足** `IntentFilter` 中所有声明的 action、category 和 data 规则时，该 `IntentFilter` 才算匹配成功。如果一个 `IntentFilter` 没有声明某个元素，则表示该元素对匹配没有限制。

`IntentFilter` 是实现 Android 组件间通信和模块化设计的基础，理解其匹配规则对于正确使用 `Intent` 和构建灵活的应用程序至关重要。



---



## 自定义View

自定义View是一个综合的技术体系,他涉及View的层次结构,事件分发机制和View的工作原理等技术细节.

*自定义View总体可以分为四类*

1. 继承View重写onDraw方法
2. 继承ViewGroup派生特殊的Layout
3. 继承特定的View(比如TextView)
4. 继承特定的ViewGroup(比如LinearLayout)



*注意事项*

1. **让View支持wrap_content: **直接继承View或ViewGroup的控件要`在onMeasure`中对`wrap_content`做特殊处理.
2. **如果有必要,让你的View支持padding:** 继承View的控件在draw方法中处理`padding`,`padding`属性才能起作用.继承ViewGroup的控件要在`onMeasure`和`onLayout`中考虑`padding`和子元素的`margin`对其造成的影响( `Padding `是 View 内部的事情,而 `Margin` 是 View 外部由其父布局决定).
3. **尽量不要在View中使用`Handler`: **View内部提供了`post`系列方法.
4. **View中如果有线程或者动画,需要及时停止: **参考`View`#`onDetachedFromWindow`.
5. **View带有滑动嵌套情形时,需要处理好滑动冲突: 重写`onInterceptTouchEvent`(外部拦截法) 或`dispatchTouchEvent`(内部拦截法)**

### measure过程

#### onMeasure 的作用是什么？



在 Android 的视图体系中，每个 View 和 ViewGroup 都需要知道自己应该占据多大的空间。这个确定大小的过程就发生在 **Measure (测量)** 阶段。

`onMeasure` 方法就是这个阶段的核心。它的职责是：

1. **接收来自父容器的尺寸约束 (Constraints)。**
2. **根据这些约束和自身的特性 (内容、背景等)，计算出自己期望的宽度和高度。**
3. **通过调用 `setMeasuredDimension(width, height)` 方法，将最终计算出的尺寸保存起来。**

整个视图树的绘制流程分为三个主要阶段：

1. **Measure (测量)**: 从根视图开始，递归地调用所有子视图的 `onMeasure`，确定每个视图的大小。
2. **Layout (布局)**: 从根视图开始，递归地调用所有子视图的 `onLayout`，确定每个视图在屏幕上的具体位置（左、上、右、下坐标）。
3. **Draw (绘制)**: 从根视图开始，递归地调用所有子视图的 `onDraw`，将它们的内容绘制到屏幕上。

`onMeasure` 是这三大流程的第一个，它为后续的布局和绘制奠定了基础。

------



#### 理解核心概念：MeasureSpec



在深入 `onMeasure` 之前，必须先理解 `MeasureSpec`。`onMeasure` 方法接收两个参数：



```java
@Override
protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
    // ...
}
```

这两个 `int` 类型的参数并不是直接的像素值，而是 `MeasureSpec`。它是一个 32 位的整数，其中高 2 位代表 **模式 (Mode)**，低 30 位代表 **尺寸 (Size)**。

`MeasureSpec` 是父容器传递给子视图的“尺寸要求”或“约束条件”。它告诉子视图：“你应该在这个规则下测量自己”。



#### MeasureSpec 的三种模式 (Mode)

1. **`MeasureSpec.EXACTLY` (精确模式)**
   - **含义**: 父容器已经为子视图确定了切确的尺寸。子视图的最终尺寸就是 `MeasureSpec` 中指定的 `size` 值。
   - **对应场景**:
     - 布局文件中设置了具体的尺寸，如 `android:layout_width="100dp"`。
     - 布局文件中设置了 `android:layout_width="match_parent"`，并且父容器的尺寸是确定的。
2. **`MeasureSpec.AT_MOST` (最大模式)**
   - **含义**: 父容器为子视图指定了一个最大可用尺寸。子视图的尺寸不能超过这个 `size` 值，但可以根据自身内容调整为比它更小的值。
   - **对应场景**:
     - 布局文件中设置了 `android:layout_width="wrap_content"`。此时 `size` 就是父容器剩余的可用空间。
3. **`MeasureSpec.UNSPECIFIED` (不指定模式)**
   - **含义**: 父容器对子视图的尺寸没有任何限制，子视图可以根据需要设置为任意大小。
   - **对应场景**:
     - 通常用在可滚动的容器中，如 `ScrollView`、`RecyclerView`。因为这些容器可以在一个维度上无限延伸，所以它们在测量子视图时不会限制其高度（或宽度）。

我们可以使用 `MeasureSpec` 类的静态方法来解析这两个值：



```java
int specMode = MeasureSpec.getMode(measureSpec);
int specSize = MeasureSpec.getSize(measureSpec);
```

------



#### 自定义 View的 onMeasure



对于一个独立的、没有子视图的自定义 `View`（例如，继承自 `View`、`TextView`、`ImageView` 等），`onMeasure` 的实现相对简单。它的目标是根据父容器的 `MeasureSpec` 计算出自己的尺寸。

**基本步骤：**

1. 重写 `onMeasure(int widthMeasureSpec, int heightMeasureSpec)` 方法。
2. 在方法内部，分别解析宽度和高度的 `mode` 和 `size`。
3. 针对不同的 `mode`，计算出最终的宽度和高度。
   - **`EXACTLY`**: 直接使用 `specSize` 作为最终尺寸。
   - **`AT_MOST`**: 计算出自己内容所需的尺寸（比如文本的宽度、图片的高度等），然后与 `specSize` 比较，取较小值。`Math.min(contentSize, specSize)`。
   - **`UNSPECIFIED`**: 直接使用自己内容所需的尺寸。
4. **【最重要的一步】** 调用 `setMeasuredDimension(measuredWidth, measuredHeight)` 保存计算结果。**如果不调用这个方法，系统会抛出异常。**



#### 代码示例：一个简单的正方形 View



假设我们要创建一个自定义 View，它总是尝试将自己渲染成一个正方形。



```java
public class SquareView extends View {

    private int defaultSize = 200; // 默认尺寸，单位 px

    public SquareView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        // 直接调用父类 onMeasure 是一个好习惯，它会处理一些默认情况
        // super.onMeasure(widthMeasureSpec, heightMeasureSpec);

        int width = getMySize(defaultSize, widthMeasureSpec);
        int height = getMySize(defaultSize, heightMeasureSpec);

        // 为了保证是正方形，我们取宽高的最小值
        int finalSize = Math.min(width, height);

        // 【关键】保存最终计算出的尺寸
        setMeasuredDimension(finalSize, finalSize);
    }

    private int getMySize(int defaultSize, int measureSpec) {
        int mySize = defaultSize;

        int mode = MeasureSpec.getMode(measureSpec);
        int size = MeasureSpec.getSize(measureSpec);

        switch (mode) {
            case MeasureSpec.EXACTLY:
                // match_parent 或具体 dp 值，父容器决定了大小，我们必须遵守
                mySize = size;
                break;
            case MeasureSpec.AT_MOST:
                // wrap_content，我们不能超过父容器给的最大值
                // 在这里，我们的内容大小就是 defaultSize
                mySize = Math.min(defaultSize, size);
                break;
            case MeasureSpec.UNSPECIFIED:
                // 父容器不关心，我们用自己的默认值
                mySize = defaultSize;
                break;
        }
        return mySize;
    }
}
```

**小技巧**: Android 提供了一个辅助方法 `resolveSize(size, measureSpec)`，它可以帮你简化 `AT_MOST` 和 `EXACTLY` 模式的处理。它等价于：



```java
public static int resolveSize(int size, int measureSpec) {
    int result = size;
    int specMode = MeasureSpec.getMode(measureSpec);
    int specSize = MeasureSpec.getSize(measureSpec);
    switch (specMode) {
    case MeasureSpec.UNSPECIFIED:
        result = size;
        break;
    case MeasureSpec.AT_MOST:
        result = Math.min(size, specSize);
        break;
    case MeasureSpec.EXACTLY:
        result = specSize;
        break;
    }
    return result;
}
```

------



#### 自定义 ViewGroup 的 onMeasure



自定义 `ViewGroup` 的 `onMeasure` 要复杂得多，因为它有双重职责：

1. **测量所有子视图 (Children)**: 它需要遍历自己的每一个子视图，调用它们的 `measure()` 方法，并为其生成合适的 `MeasureSpec`。
2. **确定自身尺寸**: 在测量完所有子视图后，它需要根据子视图的尺寸和自身的布局逻辑（例如，是线性排列还是层叠排列），计算出自己的总尺寸。

**基本步骤:**

1. 重写 `onMeasure(int widthMeasureSpec, int heightMeasureSpec)` 方法。
2. **遍历所有子视图**：使用 `for` 循环和 `getChildCount()`。
3. **为每个子视图生成 `MeasureSpec`**: 这是最复杂的一步。你需要结合父容器传给 `ViewGroup` 的 `MeasureSpec` 和子视图自身的 `LayoutParams` (例如 `layout_width="match_parent"` 或 `layout_width="wrap_content"`) 来为子视图创建一个新的 `MeasureSpec`。通常使用 `getChildMeasureSpec()` 这个静态辅助方法来完成。
4. **测量子视图**: 调用 `child.measure(childWidthMeasureSpec, childHeightMeasureSpec)`。这个调用会触发子视图的 `onMeasure` 方法。
5. **计算自身尺寸**: 测量完所有子视图后，通过 `child.getMeasuredWidth()` 和 `child.getMeasuredHeight()` 获取它们的尺寸。然后根据你的布局逻辑（例如，将所有子视图高度相加）计算出 `ViewGroup` 自身需要的总宽度和总高度。
6. **合并尺寸和约束**: 将计算出的自身所需尺寸与父容器传递的 `MeasureSpec` 进行协调（处理 `EXACTLY` 和 `AT_MOST` 模式）。
7. **保存结果**: 调用 `setMeasuredDimension()` 保存最终尺寸。



#### 代码示例：一个简单的垂直线性布局



```java
public class CustomVerticalLayout extends ViewGroup {

    public CustomVerticalLayout(Context context, AttributeSet attrs) {
        super(context, attrs);
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        // 1. 获取自身的 MeasureSpec 约束
        int widthMode = MeasureSpec.getMode(widthMeasureSpec);
        int widthSize = MeasureSpec.getSize(widthMeasureSpec);
        int heightMode = MeasureSpec.getMode(heightMeasureSpec);
        int heightSize = MeasureSpec.getSize(heightMeasureSpec);

        // 2. 用于计算 ViewGroup 自身尺寸的变量
        int totalHeight = 0; // 累计所有子 View 的高度
        int maxWidth = 0;    // 找出最宽的子 View 的宽度

        // 3. 遍历子 View
        for (int i = 0; i < getChildCount(); i++) {
            View child = getChildAt(i);
            if (child.getVisibility() == GONE) {
                continue;
            }

            // 4. 为子 View 生成 MeasureSpec 并测量它
            // measureChild 会自动处理 getChildMeasureSpec 的逻辑
            measureChild(child, widthMeasureSpec, heightMeasureSpec);
            
            // 5. 累加尺寸
            // 获取子 View 测量后的尺寸（包含 margin）
            MarginLayoutParams lp = (MarginLayoutParams) child.getLayoutParams();
            int childHeight = child.getMeasuredHeight() + lp.topMargin + lp.bottomMargin;
            int childWidth = child.getMeasuredWidth() + lp.leftMargin + lp.rightMargin;

            totalHeight += childHeight;
            maxWidth = Math.max(maxWidth, childWidth);
        }

        // 6. 根据自身约束，确定最终尺寸
        int measuredWidth;
        int measuredHeight;

        // 宽度处理
        if (widthMode == MeasureSpec.EXACTLY) {
            measuredWidth = widthSize;
        } else if (widthMode == MeasureSpec.AT_MOST) {
            measuredWidth = Math.min(maxWidth, widthSize);
        } else {
            measuredWidth = maxWidth;
        }

        // 高度处理
        if (heightMode == MeasureSpec.EXACTLY) {
            measuredHeight = heightSize;
        } else if (heightMode == MeasureSpec.AT_MOST) {
            measuredHeight = Math.min(totalHeight, heightSize);
        } else {
            measuredHeight = totalHeight;
        }
        
        // 7. 保存最终尺寸
        setMeasuredDimension(measuredWidth, measuredHeight);
    }
    
    // 为了能正确处理 margin，需要重写以下方法
    @Override
    public LayoutParams generateLayoutParams(AttributeSet attrs) {
        return new MarginLayoutParams(getContext(), attrs);
    }
}
```

**注意**: 在 `ViewGroup` 中，通常会使用 `measureChild()` 或 `measureChildWithMargins()` 这两个辅助方法，它们内部封装了 `getChildMeasureSpec()` 和 `child.measure()` 的调用，能简化代码，特别是处理 `padding` 和 `margin` 时。

------



#### Wrap_content解决方案

##### 定义默认尺寸



在你的 View 类中，最好将默认尺寸定义为成员变量，方便管理和转换。



```java
public class CircleView extends View {

    // 1. 定义默认尺寸 (以像素为单位)
    private int defaultWidth;
    private int defaultHeight;

    // ... 构造函数等 ...
    public CircleView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
        init(context);
    }

    private void init(Context context) {
        // 将 dp 转换为像素 (px)
        float density = context.getResources().getDisplayMetrics().density;
        defaultWidth = (int) (100 * density);
        defaultHeight = (int) (100 * density);
    }
    
    // ... 其他代码 ...
}
```

- **最佳实践**: 将 `100` 这个硬编码的数值定义在 `res/values/dimens.xml` 中会是更好的做法，这样更容易维护。



##### 重写 onMeasure() 方法



这是最关键的一步。你需要检查测量规格中的模式，并据此计算最终尺寸。

在很多情况下，你不需要 `Math.min` 那么复杂的逻辑，可以直接在 `AT_MOST` 模式下设置默认值。很多人也会使用 `resolveSize()` 这个辅助方法来简化代码，它内部已经包含了类似的逻辑。



```java
@Override
protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
    // resolveSize() 会帮你处理 EXACTLY 和 AT_MOST 模式
    // 在 AT_MOST 模式下，它会返回 Math.min(defaultSize, specSize)
    int measuredWidth = resolveSize(defaultWidth, widthMeasureSpec);
    int measuredHeight = resolveSize(defaultHeight, heightMeasureSpec);
    
    setMeasuredDimension(measuredWidth, measuredHeight);
    
}
```

> **注意**: `resolveSize()` 在处理 `UNSPECIFIED` 模式时，会直接返回你提供的默认尺寸。这在大多数情况下是符合预期的。

#### 总结与关键点



1. **`onMeasure` 的目的是计算并设置 View 的尺寸。**

2. **`MeasureSpec` 是核心**，它封装了父容器对子视图的尺寸约束（模式 + 尺寸）。

3. **自定义 `View`**：根据 `MeasureSpec` 和自身内容计算尺寸，然后调用 `setMeasuredDimension()`。

4. **自定义 `ViewGroup`**：

   - 先遍历并测量所有子视图 (`child.measure()`)。
   - 再根据子视图的尺寸和布局逻辑计算自身尺寸。
   - 最后调用 `setMeasuredDimension()`。

5. **必须调用 `setMeasuredDimension()`**，否则程序会崩溃。

6. `onMeasure` 可能会被调用多次，所以其中的计算逻辑应该尽可能高效，避免重复或耗时的操作。

7. 当你需要重新触发布局流程时（例如，View 的内容变化导致尺寸可能改变），应该调用 `requestLayout()` 方法，它会使视图树重新执行 `onMeasure` 和 `onLayout`。

   

---



### layout过程

`onLayout` 是 View 绘制三大流程中的第二步（Measure -> **Layout** -> Draw）。在 `onMeasure` 确定了所有 View 的尺寸之后，`onLayout` 的职责就是确定这些 View 的 **具体位置**。

------



#### onLayout的作用和参数



`onLayout` 方法的签名如下：

```java
@Override
protected void onLayout(boolean changed, int left, int top, int right, int bottom) {
    // ...
}
```

它的作用是：**父容器调用这个方法来确定其所有子视图（Children）在屏幕上的最终位置**。



#### 参数解释



- `changed` (boolean): 表示 View 的尺寸或位置是否相较于上一次布局发生了变化。如果为 `true`，意味着需要重新布局。

- `left`, `top`, `right`, `bottom` (int): 这四个参数定义了 **当前 View 本身** 在其父容器坐标系中的位置和区域。它们是由当前 View 的父容器在调用 `child.layout()` 时传递过来的。

  - `left`: View 左边缘相对于父容器左边缘的距离。
  - `top`: View 上边缘相对于父容器上边缘的距离。
  - `right`: View 右边缘相对于父容器左边缘的距离。
  - `bottom`: View 下边缘相对于父容器上边缘的距离。

  你可以通过 `getWidth()` (`right - left`) 和 `getHeight()` (`bottom - top`) 获取到 View 在 `onMeasure` 后并经过布局确定的最终尺寸。

------



#### 自定义 View的 onLayout



对于一个不包含任何子视图的自定义 `View` (继承自 `View` 或 `TextView` 等)，**通常你不需要重写 `onLayout` 方法**。

**为什么？**

因为 `onLayout` 的核心职责是 **布局它的子视图**。一个普通的 `View` 没有子视图，所以它没有东西需要布局。它的位置完全由它的父 `ViewGroup` 来决定。

父 `ViewGroup` 在自己的 `onLayout` 方法中，会计算出这个 `View` 应该放在哪里，然后调用这个 `View` 的 `layout(l, t, r, b)` 方法。`View` 类中 `final` 的 `layout()` 方法会负责将这四个坐标值保存到 View 的内部变量中（`mLeft`, `mTop`, `mRight`, `mBottom`），然后它会调用 `onLayout()`。

`View.java` 中 `onLayout()` 的默认实现是空的：



```java
// in View.java
protected void onLayout(boolean changed, int left, int top, int right, int bottom) {
    // Empty by default
}
```

所以，对于一个简单的自定义 `View`，由于没有子 View 需要排列，我们让这个空方法被调用就可以了，无需重写。

------



#### 自定义 ViewGroup的 onLayout



`onLayout` 对于自定义 `ViewGroup` 来说是 **必须实现的核心方法**。这正是 `ViewGroup` 与普通 `View` 的本质区别所在——它需要管理和排列它的孩子们。

**核心职责：**

遍历所有的子视图，并为每一个子视图调用其 `layout(l, t, r, b)` 方法，从而将它们放置在 `ViewGroup` 内部的正确位置。

**基本步骤：**

1. 重写 `onLayout(boolean changed, int left, int top, int right, int bottom)` 方法。
2. 在方法内部，通常需要先考虑自身的 `padding`。子视图的布局区域应该在 `ViewGroup` 的 `padding` 内部。
3. **遍历所有子视图**：使用 `for` 循环和 `getChildCount()`。
4. **获取子视图的测量尺寸**：在 `onMeasure` 阶段结束后，可以通过 `child.getMeasuredWidth()` 和 `child.getMeasuredHeight()` 来获取每个子视图希望的尺寸。
5. **计算每个子视图的位置**：根据你的布局逻辑（例如垂直线性排列、水平排列、或者像 `FrameLayout` 一样层叠），计算出每个子视图的 `left`, `top`, `right`, `bottom` 坐标。**注意：这些坐标是相对于当前 `ViewGroup` 的，而不是相对于屏幕的。**
6. **【最重要的一步】** 为每个子视图调用 `child.layout(childLeft, childTop, childRight, childBottom)`，将计算出的位置应用到子视图上。这个调用会递归地触发子视图的布局流程。



#### 代码示例：继续完善 CustomVerticalLayout



我们接着 `onMeasure` 的例子，为 `CustomVerticalLayout` 实现 `onLayout` 方法。

```java
public class CustomVerticalLayout extends ViewGroup {

    // ... 构造函数和 onMeasure 方法 ...

    @Override
    protected void onLayout(boolean changed, int l, int t, int r, int b) { // 定义了 getWidth() 和 getHeight() 的值。
        // 1. 考虑自身的 padding作为初始坐标
        int currentTop = getPaddingTop();
        int currentLeft = getPaddingLeft();

        // 2. 遍历子 View
        for (int i = 0; i < getChildCount(); i++) {
            View child = getChildAt(i);
            if (child.getVisibility() == GONE) {
                continue;
            }
            
            // 3. 获取子 View 的测量尺寸和 margin
            MarginLayoutParams lp = (MarginLayoutParams) child.getLayoutParams();
            int childWidth = child.getMeasuredWidth();
            int childHeight = child.getMeasuredHeight();
            
            // 4. 计算子 View 的 left, top, right, bottom 坐标
            // 坐标系是相对于当前 ViewGroup 的,包括内边距
            int left = currentLeft + lp.leftMargin; // 左坐标+左外边距
            int top = currentTop + lp.topMargin;
            int right = left + childWidth;
            int bottom = top + childHeight;

            // 5. 【关键】调用 child.layout() 来放置子 View
            child.layout(left, top, right, bottom);

            // 6. 更新下一个子 View 的起始 top 位置,包括外边距
            currentTop += childHeight + lp.topMargin + lp.bottomMargin;
        }
    }
    
    // ... generateLayoutParams 方法 ...
}
```

在这个例子中：

- 我们维护一个 `currentTop` 变量，用于记录当前应当放置子视图的纵向位置。
- 对于每个子视图，我们从 `currentTop` 开始，加上它的 `topMargin`，确定它的 `top` 位置。
- 它的 `bottom` 位置就是 `top + childHeight`。
- 它的 `left` 和 `right` 位置则由 `ViewGroup` 的 `paddingLeft`、子视图的 `leftMargin` 和子视图的宽度共同决定。
- 放置好一个子视图后，我们将 `currentTop` 增加这个子视图的总高度（测量高度 + 上下 margin），为下一个子视图的放置做准备。

------



#### 四、onMeasure` vs `onLayout 核心区别对比



为了更清晰地理解，我们将这两个方法进行对比：

| 特性         | `onMeasure`                                                  | `onLayout`                                                   |
| ------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| **核心职责** | 计算视图的 **尺寸** (Size)                                   | 确定视图的 **位置** (Position)                               |
| **工作对象** | **`View` 和 `ViewGroup` 都需要**。`ViewGroup` 测量子视图，并基于此计算自身尺寸。 | **主要是 `ViewGroup` 的职责**。`View` 通常不需要实现它。     |
| **关键调用** | 计算完尺寸后，必须调用 `setMeasuredDimension()` 来保存结果。 | 遍历子视图，为每个子视图调用 `child.layout()` 来应用位置。   |
| **输入参数** | `widthMeasureSpec`, `heightMeasureSpec` (父容器的尺寸约束)   | `changed`, `left`, `top`, `right`, `bottom` (自身在父容器中的位置) |
| **数据来源** | 依赖父容器的约束和自身内容。                                 | 依赖 `onMeasure` 的结果 (`getMeasuredWidth()`) 和自身的布局逻辑。 |
| **触发时机** | 在 `layout` 之前。                                           | 在 `measure` 之后，`draw` 之前。                             |



#### 总结与关键点



1. **`onLayout` 的核心使命是“放位置”**。它在 `onMeasure` 算出“有多大”之后，决定“放哪里”。
2. 对于**自定义 `View`**，因为没有子视图，所以**基本不用管 `onLayout`**。
3. 对于**自定义 `ViewGroup`**，`onLayout` 是你实现**自定义布局逻辑**的地方，**必须重写**。
4. 在 `onLayout` 中，你需要遍历子视图，获取它们测量好的尺寸，然后调用 `child.layout()` 将它们一一安放。
5. `onLayout` 中计算的坐标是 **相对于父容器（也就是当前这个 `ViewGroup`）的**。
6. `requestLayout()` 会触发布局的重新计算，它会依次调用 `onMeasure` 和 `onLayout`。而 `invalidate()` 只会触发重绘，调用 `onDraw`。如果你的 View 只是内容变了但尺寸和位置不变（比如改变画笔颜色），调用 `invalidate()`；如果尺寸可能发生变化，调用 `requestLayout()`。

---



### draw过程

`onDraw` 是 View 绘制三大流程中的最后一步（Measure -> Layout -> **Draw**）。在 `onMeasure` 确定了尺寸、`onLayout` 确定了位置之后，`onDraw` 的职责就是 **将视图的内容实际绘制到屏幕上**。

------



#### onDraw 的作用和核心工具



`onDraw` 方法的签名如下：

```java
@Override
protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);
    // ...
}
```

它的作用是：**提供一个 `Canvas` (画布) 对象，让你可以在上面进行任意的 2D 绘制操作。**



##### 核心工具



要进行绘制，通常需要两个核心类的配合：

1. **`Canvas` (画布)**
   - `onDraw` 方法会传给你一个 `Canvas` 对象。你可以把它想象成一块指定了尺寸和位置的画布。
   - 它提供了一系列 `draw...()` 方法，比如：
     - `drawRect(float left, float top, float right, float bottom, Paint paint)`: 绘制矩形。
     - `drawCircle(float cx, float cy, float radius, Paint paint)`: 绘制圆形。
     - `drawLine(float startX, float startY, float stopX, float stopY, Paint paint)`: 绘制线条。
     - `drawText(String text, float x, float y, Paint paint)`: 绘制文本。
     - `drawBitmap(Bitmap bitmap, float left, float top, Paint paint)`: 绘制位图 (Bitmap)。
     - `drawPath(Path path, Paint paint)`: 绘制自定义路径。
   - `Canvas` 定义了 **“画什么”** 和 **“在哪里画”**。
2. **`Paint` (画笔)**
   - `Paint` 对象定义了绘制的样式和效果。你可以把它想象成一支画笔。
   - 它控制着各种属性，比如：
     - `setColor(int color)`: 设置颜色。
     - `setStyle(Paint.Style style)`: 设置样式（`FILL` 填充、`STROKE` 描边、`FILL_AND_STROKE` 填充并描边）。
     - `setStrokeWidth(float width)`: 设置线条或描边的宽度。
     - `setTextSize(float textSize)`: 设置文本大小。
     - `setAntiAlias(boolean aa)`: 设置是否开启抗锯齿，能让图形边缘更平滑。
   - `Paint` 定义了 **“怎么画”** (颜色、粗细、样式等)。

**坐标系**：在 `onDraw` 方法中，画布 `Canvas` 的坐标系原点 `(0, 0)` 位于当前 `View` 的 **左上角**。

------



#### 自定义 View的 onDraw



对于一个不包含子视图的自定义 `View`，`onDraw` 是它的灵魂。这是你将所有视觉元素呈现给用户的地方。

**核心职责：** 使用 `Canvas` 和 `Paint` 对象，在 `View` 的边界内绘制出你想要的任何内容。

**基本步骤：**

1. **初始化 `Paint` 对象**：**这是一个至关重要的性能优化点**。`onDraw` 方法会被频繁调用（例如，在动画期间每秒调用 60 次）。如果在 `onDraw` 内部创建 `Paint` 等对象，会引发大量的内存分配和垃圾回收（GC），导致界面卡顿。因此，**务必在构造函数或其他一次性初始化方法中创建 `Paint` 对象**。
2. 重写 `onDraw(Canvas canvas)` 方法。
3. 在 `onDraw` 内部，你可以通过 `getWidth()` 和 `getHeight()` 获取当前 `View` 的最终宽高。
4. 使用 `canvas.draw...()` 方法和预先初始化好的 `Paint` 对象进行绘制。



##### 代码示例：一个绘制了背景和圆形的 SquareView



我们继续之前的 `SquareView` 例子，让它不再是透明的，而是在内部绘制内容。

```java
public class SquareView extends View {

    private Paint backgroundPaint;
    private Paint circlePaint;

    public SquareView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
        initPaints();
    }

    // 1. 在构造函数中初始化画笔
    private void initPaints() {
        backgroundPaint = new Paint();
        backgroundPaint.setColor(Color.parseColor("#8888FF")); // 淡蓝色背景
        backgroundPaint.setStyle(Paint.Style.FILL); // 填充样式

        circlePaint = new Paint(Paint.ANTI_ALIAS_FLAG); // 创建时就开启抗锯齿
        circlePaint.setColor(Color.RED);
        circlePaint.setStyle(Paint.Style.FILL);
    }

    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        // ... onMeasure 逻辑保持不变 ...
        // 假设它最终测量自己为一个正方形
        super.onMeasure(widthMeasureSpec, heightMeasureSpec); // 简化处理
    }

    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas); // 绘制 View 自身的背景等

        // 2. 获取 View 的尺寸
        int width = getWidth();
        int height = getHeight();
        
        // 3. 绘制背景
        // 绘制一个覆盖整个 View 的矩形
        canvas.drawRect(0, 0, width, height, backgroundPaint);

        // 4. 在中心绘制一个红色的圆
        float centerX = width / 2f;
        float centerY = height / 2f;
        float radius = Math.min(width, height) / 4f; // 半径为宽/高中较小值的 1/4
        canvas.drawCircle(centerX, centerY, radius, circlePaint);
    }
}
```

------



#### 自定义 ViewGroup 的 onDraw



默认情况下，`ViewGroup` 是不会调用 `onDraw` 方法的。

**为什么？**

因为 `ViewGroup` 的主要职责是管理和布局子 `View`，它本身通常被认为是透明的、不可见的容器。为了进行性能优化，系统假设它不需要绘制任何内容，从而跳过它的 `onDraw` 流程。子 `View` 的绘制是由系统在 `dispatchDraw` 方法中管理的。

**如何让 `ViewGroup` 调用 `onDraw`？**

如果你希望 `ViewGroup` 也绘制一些内容（例如，一个特殊的背景、子 `View` 之间的分割线），你必须明确告诉系统。有两种常用方法：

1. **在构造函数中调用 `setWillNotDraw(false)`** `ViewGroup` 内部有一个标志位，默认为 `true`，表示“我不会绘制任何东西”。将其设置为 `false` 后，系统在绘制流程中就会调用该 `ViewGroup` 的 `onDraw` 方法。

   ```java
   public CustomVerticalLayout(Context context, AttributeSet attrs) {
       super(context, attrs);
       // 告诉系统，这个 ViewGroup 需要执行 onDraw
       setWillNotDraw(false); 
   }
   ```

2. **在 XML 中设置 `android:background` 属性** 只要你为 `ViewGroup` 设置了任何背景（无论是颜色还是 `Drawable`），系统会自动为你调用 `setWillNotDraw(false)`，因为绘制背景本身就需要 `onDraw` 流程的参与。



#### ViewGroup 的绘制顺序



当一个 `ViewGroup` 需要绘制时，其绘制层级如下：

1. **绘制背景**: 如果设置了 `background`，先绘制它。
2. **绘制自身内容 (`onDraw`)**: 调用 `onDraw` 方法，绘制你在其中指定的任何内容（例如分割线）。
3. **绘制子视图 (`dispatchDraw`)**: `onDraw` 执行完毕后，系统会调用 `dispatchDraw(Canvas canvas)` 方法。这个方法会遍历所有子 `View`，并调用每个子 `View` 的 `draw()` 方法，从而让子 `View` 绘制自己。
4. **绘制前景和滚动条**: 最后，绘制滑动边缘效果、滚动条和前景（`Foreground`）。

这意味着，在 `onDraw` 中绘制的内容会被 **“压在”**所有子 `View` 的 **下面**。如果你想在子 `View` 的上面绘制内容，你需要重写 `dispatchDraw` 方法，并在调用 `super.dispatchDraw(canvas)` 之后进行绘制。

------



#### 代码示例：继续完善 CustomVerticalLayout



```java
public class CustomVerticalLayout extends ViewGroup {

    // 1. 定义一个画笔用于绘制分割线
    private Paint mDividerPaint;

    public CustomVerticalLayout(Context context, AttributeSet attrs) {
        super(context, attrs);
        init();
    }

    private void init() {
        // 2. 初始化画笔
        mDividerPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
        mDividerPaint.setColor(Color.RED);
        mDividerPaint.setStyle(Paint.Style.STROKE);
        mDividerPaint.setStrokeWidth(4); // 设置分割线厚度为 4px

        // 3. 【关键】告诉系统这个 ViewGroup 需要执行 onDraw 方法
        setWillNotDraw(false);
    }
    
    // onMeasure 和 onLayout 方法与之前的例子完全相同...
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        // ... 此处省略 onMeasure 的完整代码 ...
        // 关键是调用 measureChildWithMargins() 或者类似逻辑
        // 并最终调用 setMeasuredDimension()
    }

    @Override
    protected void onLayout(boolean changed, int l, int t, int r, int b) {
        // ... 此处省略 onLayout 的完整代码 ...
        // 关键是循环调用 child.layout()
    }


    // 4. 实现 onDraw 方法来绘制内容
    @Override
    protected void onDraw(Canvas canvas) {
        // 先调用父类的方法，这是一个好习惯
        super.onDraw(canvas);

        // 5. 遍历子视图，在它们之间画线
        for (int i = 0; i < getChildCount() - 1; i++) { // 注意是 getChildCount() - 1
            View child = getChildAt(i);
            
            // 如果子视图是 GONE，则跳过
            if (child.getVisibility() == GONE) {
                continue;
            }

            // 6. 计算分割线的 Y 坐标
            MarginLayoutParams lp = (MarginLayoutParams) child.getLayoutParams();
            // 将线画在子视图底部和其 bottomMargin 之间的区域
            float dividerY = child.getBottom() + lp.bottomMargin / 2.0f;

            // 7. 计算分割线的 X 坐标范围
            float startX = getPaddingLeft();
            float stopX = getWidth() - getPaddingRight();

            // 8. 【关键】使用 canvas 画线
            canvas.drawLine(startX, dividerY, stopX, dividerY, mDividerPaint);
        }
    }

     @Override
    protected void dispatchDraw(Canvas canvas) {
        // 首先，让系统完成所有子视图的绘制
        super.dispatchDraw(canvas);

        // 然后，在所有子视图都画完之后，我们再画上我们的顶层内容
        if (mSelectedIndex >= 0 && mSelectedIndex < getChildCount()) {
            View selectedChild = getChildAt(mSelectedIndex);
            if (selectedChild != null && selectedChild.getVisibility() != GONE) {
                // 获取选中子视图的边界
                int left = selectedChild.getLeft();
                int top = selectedChild.getTop();
                int right = selectedChild.getRight();
                int bottom = selectedChild.getBottom();
                // 在其上层绘制一个半透明的红色矩形作为遮罩
                canvas.drawRect(left, top, right, bottom, mOverlayPaint);
            }
        }
    }

    // ... generateLayoutParams 方法 ...
    // 为了能正确处理 margin，需要重写 generateLayoutParams 相关方法
    @Override
    public LayoutParams generateLayoutParams(AttributeSet attrs) {
        return new MarginLayoutParams(getContext(), attrs);
    }
    // ... 其他 generateLayoutParams 重载 ...
}
```



在这个例子中：



- **`Paint mDividerPaint`**: 我们首先定义了一个 `Paint` 对象作为成员变量。这个“画笔”将专门用来绘制我们的分割线。
- **`init()` 方法**: 我们在构造函数中调用 `init()` 来进行初始化。将初始化逻辑集中在一起是个好习惯。在这里，我们创建了 `Paint` 对象并设置了它的颜色、样式（描边）和粗细。
- **`setWillNotDraw(false)`**: 这是让 `ViewGroup` 能够绘制的关键一步。默认情况下，为了优化性能，`ViewGroup` 不会调用 `onDraw` 方法。我们通过这个调用来告诉系统：“这个 `ViewGroup` 有自己的内容需要绘制，请调用我的 `onDraw` 方法”。
- **`onDraw(Canvas canvas)`**: 这是实际进行绘制的地方。系统会传入一个 `Canvas`（画布）对象，我们所有的绘制操作都在这个画布上进行。
- **遍历子视图**: 我们使用一个 `for` 循环来遍历子视图。请注意循环条件是 `i < getChildCount() - 1`，因为我们只需要在子视图“之间”画线，所以最后一个子视图下面不需要再画。
- **计算Y坐标**: 我们需要确定水平分割线应该画在哪里。一个比较好的位置是在当前子视图的底部（`child.getBottom()`）与其下外边距（`lp.bottomMargin`）所构成的空间内。这里我们取其中点，使得视觉上更均衡。
- **计算X坐标**: 我们希望分割线能横跨整个 `ViewGroup` 的内容区域，所以线的起点是 `ViewGroup` 的左内边距（`getPaddingLeft()`），终点是 `ViewGroup` 的宽度减去右内边距（`getWidth() - getPaddingRight()`）。
- **`canvas.drawLine(...)`**: 最后，我们调用 `canvas` 的 `drawLine` 方法，传入计算好的起点和终点坐标，并指定使用我们预设好的 `mDividerPaint` 画笔，从而在屏幕上绘制出红色的分割线。

#### 如何触发重绘



当你的 `View` 的外观需要更新时（例如，用户点击后改变颜色），你需要通知系统重新绘制它。这通过调用 `invalidate()` 方法来完成。

- `invalidate()`: 请求重绘当前 `View`。它会使 `View` 的 `onDraw` 方法在未来的某个时间点（通常是下一帧）被UI线程调用。它只会触发绘制流程，不会触发布局和测量。
- `postInvalidate()`: `invalidate()` 的线程安全版本。如果你在非 UI 线程想刷新 `View`，应调用此方法。

**再次对比 `requestLayout()` 和 `invalidate()`**

- **`requestLayout()`**: 当 `View` 的 **尺寸或位置** 可能发生变化时调用。它会触发 `onMeasure`, `onLayout`, `onDraw` 的完整流程。这是一个重量级操作。
- **`invalidate()`**: 当 `View` 的 **外观** 发生变化，但尺寸和位置不变时调用。它只触发 `onDraw`。这是一个相对轻量级的操作。



#### 总结与关键点



1. **`onDraw` 是绘制流程的最后一步，负责将视图渲染到屏幕上。**
2. **自定义 `View`** 的核心就是实现 `onDraw`，使用 `Canvas`(画什么,在哪画) 和 `Paint`(怎么画) 来创造视觉内容。
3. **自定义 `ViewGroup`** 默认不执行 `onDraw`。若要绘制，需调用 `setWillNotDraw(false)` 或设置背景。`ViewGroup` 的 `onDraw` 内容会显示在所有子 `View` 的下方。
4. **性能至上**: **绝对不要** 在 `onDraw` 方法中创建新对象（`new Paint()`, `new Path()` 等），应在构造函数中预先创建并复用。
5. **触发重绘**: 使用 `invalidate()` 或 `postInvalidate()` 来请求系统重新调用 `onDraw`。



---

























































