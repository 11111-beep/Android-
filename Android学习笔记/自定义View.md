# 自定义View学习



 ## 1.Canonical layouts  规范布局

### List-detail 列表详细信息

**List-Detail布局**（也叫**Master-Detail布局**）是一种经典的界面设计模式，特别适用于具有层级关系的数据展示场景。它的基本思想是将列表和详细内容分开显示，让用户通过选择列表中的某个项来查看该项的详细信息。

- **应用场景**：如联系人、邮件、商品列表等需要展示一系列项目的应用场景。
- **界面特点**：
  - **List（主列表）**：显示一组数据项，通常以列表形式呈现。用户可以在列表中滚动、选择某一项。
  - **Detail（详细信息）**：在用户选择列表项后，右侧（或下方）会显示该项的详细内容。这通常涉及文本、图片或其他详细信息。
- **平板设备上的表现**：在大屏幕上，列表和详细内容通常是并排显示的，提供更丰富的互动体验；而在手机上，则可能通过新的Activity或Fragment来显示详细信息，通常是单独的页面。

**示例**：

- 联系人应用：左侧是联系人列表，右侧则显示所选联系人的详细信息。

<video src="video/List-detail.mp4"></video>

---



### Feed 饲料

**Feed布局**（Feed-based Layout）通常用于呈现信息流形式的内容，最典型的例子是社交媒体应用或新闻应用。这种布局方式强调动态内容的展示，用户通过滚动或刷新，查看来自不同源的内容条目。

- **应用场景**：社交平台、新闻聚合、博客、视频流等。
- **界面特点**：
  - **内容项（Feed Item）**：每个条目（如一篇文章、一条社交动态、一条视频等）都有独立的展示区域。条目通常包括标题、图片、描述、时间戳等信息。
  - **垂直滚动**：Feed布局大多采用垂直滚动的方式，用户可以通过滑动手指快速浏览内容。
  - **无明确层次结构**：与List-Detail不同，Feed布局没有明确的子项和详细信息区域，而是所有条目都平等展示，点击后可能进入更深入的详情页面。

**示例**：

- Facebook、Twitter、Instagram等应用的动态信息流界面。

<video src="video/feed.mp4"></video>

---



### Supporting pane 支持窗格

**Supporting Pane布局**是一种辅助面板设计，用于将次要信息或功能以面板形式展示在主界面旁边。它通常与主内容区并排显示，可以是信息的补充、控制面板或选项设置。

- **应用场景**：主界面需要显示详细内容或提供一些附加操作的应用，如邮箱应用、设置页面或工作流管理工具。
- **界面特点**：
  - **主内容区**：展示核心信息或主要交互界面。
  - **辅助面板（Supporting Pane）**：在主内容的旁边，显示附加的信息或工具。这些面板通常是可展开、可收缩的，允许用户在需要时查看额外的选项、详细信息或交互。
- **平板设备上的表现**：Supporting Pane布局非常适合大屏设备，如平板电脑，主内容和辅助面板可以并排展示，提供丰富的双窗格体验。

**示例**：

- Gmail：主屏幕显示邮件列表，右侧显示选中的邮件内容或附加选项。
- 设置应用：左侧是设置类别，右侧显示所选类别的详细设置项。

<video src="video/Supporting pane.mp4"></video>

---



## 2.LayoutInflater



先来看一下LayoutInflater的基本用法吧，它的用法非常简单，首先需要获取到LayoutInflater的实例，有两种方法可以获取到，第一种写法如下：

```java
LayoutInflater layoutInflater = LayoutInflater.from(context);
```



当然，还有另外一种写法也可以完成同样的效果：

```java
LayoutInflater layoutInflater = (LayoutInflater) context
		.getSystemService(Context.LAYOUT_INFLATER_SERVICE);
```



其实第一种就是第二种的简单写法，只是Android给我们做了一下封装而已。得到了LayoutInflater的实例之后就可以调用它的inflate()方法来加载布局了，如下所示：

```java
layoutInflater.inflate(resourceId, root);
```



inflate()方法一般接收两个参数，第一个参数就是要加载的布局id，第二个参数是指给该布局的外部再嵌套一层父布局，如果不需要就直接传null。这样就成功成功创建了一个布局的实例，之后再将它添加到指定的位置就可以显示出来了。



下面我们就通过一个非常简单的小例子，来更加直观地看一下LayoutInflater的用法。比如说当前有一个项目，其中MainActivity对应的布局文件叫做activity_main.xml，代码如下所示：

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/main_layout"
    android:layout_width="match_parent"
    android:layout_height="match_parent" >

</LinearLayout>
```

这个布局文件的内容非常简单，只有一个空的LinearLayout，里面什么控件都没有，因此界面上应该不会显示任何东西。



那么接下来我们再定义一个布局文件，给它取名为button_layout.xml，代码如下所示：

```xml
<Button xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Button" >

</Button>
```



这个布局文件也非常简单，只有一个Button按钮而已。现在我们要想办法，如何通过LayoutInflater来将button_layout这个布局添加到主布局文件的LinearLayout中。根据刚刚介绍的用法，修改MainActivity中的代码，如下所示：


```java
public class MainActivity extends Activity {
private LinearLayout mainLayout;
 
@Override
protected void onCreate(Bundle savedInstanceState) {
	super.onCreate(savedInstanceState);
	setContentView(R.layout.activity_main);
	mainLayout = (LinearLayout) findViewById(R.id.main_layout);
	LayoutInflater layoutInflater = LayoutInflater.from(this);
	View buttonLayout = layoutInflater.inflate(R.layout.button_layout, null);
	mainLayout.addView(buttonLayout);
}
}
```



可以看到，这里先是获取到了LayoutInflater的实例，然后调用它的inflate()方法来加载button_layout这个布局，最后调用LinearLayout的addView()方法将它添加到LinearLayout中。

---



## 3.onMeasure()、onLayout()和onDraw()

### 1. onMeasure() —— 测量阶段

#### 目的与原理

- **测量自身尺寸：**
   在 onMeasure() 方法中，View 根据父视图传递下来的测量约束（MeasureSpec）来计算并确定自己的宽度和高度。这些约束信息中包含了具体的模式（MeasureSpec.EXACTLY、AT_MOST、UNSPECIFIED）和具体的尺寸数值。
- **设置测量结果：**
   View 计算完需要的宽高之后，需调用 `setMeasuredDimension(width, height)` 将结果传递给系统。这个结果将直接影响接下来的布局和绘制阶段。



```kotlin
class CustomView(context: Context, attrs: AttributeSet? = null) : View(context, attrs) {

    override fun onMeasure(widthMeasureSpec: Int, heightMeasureSpec: Int) {
        // 通过 MeasureSpec 获取模式和尺寸
        val widthMode = MeasureSpec.getMode(widthMeasureSpec)
        val widthSize = MeasureSpec.getSize(widthMeasureSpec)
        val heightMode = MeasureSpec.getMode(heightMeasureSpec)
        val heightSize = MeasureSpec.getSize(heightMeasureSpec)

        // 定义希望的默认尺寸（例如200x200）
        val desiredWidth = 200
        val desiredHeight = 200

        // 根据模式判断最终的宽度
        val width = when (widthMode) {
            MeasureSpec.EXACTLY -> {
                // 当父视图要求精确值时，直接采用父视图给予的宽度
                widthSize
            }
            MeasureSpec.AT_MOST -> {
                // 允许的最大值，取希望宽度与最大值中的较小者
                desiredWidth.coerceAtMost(widthSize)
            }
            else -> {
                // 未指定模式时，使用默认尺寸
                desiredWidth
            }
        }

        // 根据模式判断最终的高度
        val height = when (heightMode) {
            MeasureSpec.EXACTLY -> heightSize
            MeasureSpec.AT_MOST -> desiredHeight.coerceAtMost(heightSize)
            else -> desiredHeight
        }

        // 将测量结果传递给系统
        setMeasuredDimension(width, height)
    }
}
```

### 注意事项

- **Padding 与内容尺寸：** 如果 View 设置了 padding，实际内容区域的尺寸需要在测量时进行相应的减法处理。
- **子 View 测量（自定义 ViewGroup）：** 当自定义 ViewGroup 时，需要遍历所有子 View 并调用 measureChild() 或 measureChildWithMargins() 方法来测量子 View 的尺寸，然后综合计算出最终的尺寸。

------

### 2. onLayout() —— 布局阶段

#### 目的与原理

- **定位子 View：**
   对于继承自 ViewGroup 的容器控件，onLayout() 方法负责根据先前测量获得的尺寸和父视图的约束，为所有的子 View 分配具体的位置和区域。这里通常会遍历所有子 View，并调用子 View 的 layout() 方法，指定子 View 在父容器中的 left、top、right、bottom 坐标。
- **普通 View 的 onLayout()：**
   如果继承的是普通 View 而非 ViewGroup，通常无需重写 onLayout() 方法，因为这个阶段已经由父布局来确定 View 的位置。

#### 自定义 ViewGroup

```kotlin
class CustomViewGroup(context: Context, attrs: AttributeSet? = null) : ViewGroup(context, attrs) {

    override fun onMeasure(widthMeasureSpec: Int, heightMeasureSpec: Int) {
        var maxWidth = 0
        var totalHeight = 0

        // 测量所有子 View 并计算总高度以及最大的宽度
        for (i in 0 until childCount) {
            val child = getChildAt(i)
            // 测量子 View
            measureChild(child, widthMeasureSpec, heightMeasureSpec)
            maxWidth = maxWidth.coerceAtLeast(child.measuredWidth)
            totalHeight += child.measuredHeight
        }

        // 考虑 ViewGroup 自身的 padding
        maxWidth += paddingLeft + paddingRight
        totalHeight += paddingTop + paddingBottom

        // 根据测量规格解析最终的尺寸
        setMeasuredDimension(
            resolveSize(maxWidth, widthMeasureSpec),
            resolveSize(totalHeight, heightMeasureSpec)
        )
    }

    override fun onLayout(changed: Boolean, l: Int, t: Int, r: Int, b: Int) {
        var currentTop = paddingTop

        // 遍历子 View，从上到下依次排列
        for (i in 0 until childCount) {
            val child = getChildAt(i)
            val childWidth = child.measuredWidth
            val childHeight = child.measuredHeight

            // 为每个子 View 分配位置区域
            child.layout(
                paddingLeft,
                currentTop,
                paddingLeft + childWidth,
                currentTop + childHeight
            )
            currentTop += childHeight  // 更新 top 坐标，继续排列下一个子 View
        }
    }
}
```

### 注意事项

- **布局算法：** 自定义 ViewGroup 时，布局算法可以非常灵活，可以实现从简单的线性排列到复杂的层叠、对齐等效果。
- **边距 (Margin)：** 如果需要支持 Margin，那么除了 padding 外，还需要在布局时额外计算每个子 View 的 margin 值。

------

### 3. onDraw() —— 绘制阶段

#### 目的与原理

- **真正的绘制操作：**
   onDraw() 方法是在 Canvas 上执行实际绘制的过程，包括绘制背景、文本、图形以及其他装饰内容。此时视图的尺寸和位置均已经确定，开发者可以利用 Canvas 提供的 API 进行自定义绘制。
- **绘制顺序的重要性：**
   一般建议先绘制背景，再绘制主要内容，最后绘制前景（例如边框或阴影）。同时注意绘制过程中尽量避免复杂运算与频繁内存分配，以保证绘制过程高效流畅。

```kotlin
class CustomDrawingView(context: Context, attrs: AttributeSet? = null) : View(context, attrs) {
    private val bluePaint = Paint(Paint.ANTI_ALIAS_FLAG).apply {
        color = Color.BLUE
        style = Paint.Style.FILL
    }
    private val textPaint = Paint(Paint.ANTI_ALIAS_FLAG).apply {
        color = Color.WHITE
        textSize = 40f
        style = Paint.Style.FILL
    }

    override fun onDraw(canvas: Canvas?) {
        super.onDraw(canvas)
        canvas?.let {
            // 绘制整个视图的背景矩形
            it.drawRect(0f, 0f, width.toFloat(), height.toFloat(), bluePaint)
            // 在中间绘制文本
            val text = "Hello, Kotlin!"
            // 计算文本绘制的位置 (这里以简单方式居中)
            val textWidth = textPaint.measureText(text)
            val x = (width - textWidth) / 2f
            val y = (height / 2f) + (textPaint.textSize / 2f)
            it.drawText(text, x, y, textPaint)
        }
    }
}
```

### 注意事项

- **Canvas 的状态保存：** 对于复杂的绘制操作，可以使用 `canvas.save()` 和 `canvas.restore()` 方法保存和还原 Canvas 状态，防止状态污染。
- **抗锯齿与性能：** 使用 `ANTI_ALIAS_FLAG` 可以提升绘制的平滑度，但需要在性能要求较高的场合慎重使用或做优化。
- **重绘：** 当视图需要刷新时，可调用 `invalidate()` 方法触发 onDraw() 重绘。

------

### 总结

1. **onMeasure() —— 测量阶段：**
   - 在该阶段，View 根据父视图提供的 MeasureSpec 约束（包括 EXACTLY、AT_MOST 和 UNSPECIFIED 三种模式）计算出所需的宽高，并通过 `setMeasuredDimension()` 通知系统。
2. **onLayout() —— 布局阶段：**
   - 对于 ViewGroup，这一阶段负责确定每个子 View 的位置（left、top、right、bottom），依据测量结果和布局规则（如 padding、margin）进行排列。对于普通 View，此阶段则由父布局决定，无需重写。
3. **onDraw() —— 绘制阶段：**
   - 在 onDraw() 中使用 Canvas 对象进行实际绘制操作，比如绘制背景、图形、文本等。这个阶段直接影响用户在屏幕上看到的显示效果。

---



![image-20250426190049802](image-20250426190049802.png)



```kotlin
@SuppressLint("AppCompatCustomView")
class GradientTextView(context: Context, attrs: AttributeSet?) : TextView(context, attrs) {
    init {

    }
    override fun onDraw(canvas: Canvas) {
        val text = text.toString()

        // 设置渐变
        val gradient = LinearGradient(
            0f, 0f, width.toFloat(), 0f, // 渐变的开始和结束坐标
            intArrayOf(0xFF2196F3.toInt(), 0xFF9C27B0.toInt()),  // 紫色到蓝色
            null, Shader.TileMode.CLAMP
        )

        // 将渐变效果应用到文本的 Paint 上
        paint.shader = gradient

        // 调用父类的 onDraw 方法来绘制文本
        super.onDraw(canvas)
    }
}
```

```kotlin
class WaveView @JvmOverloads constructor(
    context: Context,
    attrs: AttributeSet? = null,
    defStyleAttr: Int = 0
) : View(context, attrs, defStyleAttr) {

  /*  自定义 View 的构造函数和初始化。
    使用 Paint 配置绘制样式（颜色、填充、抗锯齿、透明度）。
    使用 Path 定义复杂的形状。
    利用 ValueAnimator 实现平滑的动画效果。
    在 onDraw 方法中根据动画值动态计算和绘制形状。
    在 onDetachedFromWindow 中进行资源清理，防止内存泄漏。*/

    private val paint = Paint().apply { // 创建画笔
        color = Color.parseColor("#2196F3") // 使用蓝色
        style = Paint.Style.FILL
        isAntiAlias = true
        alpha = 50 // 半透明
    }

    private val path = Path() // 用于绘制波浪路径
    private var offset = 0f // 波浪偏移量
    private var amplitude = 30f // 波浪振幅
    private var period = 400f // 波浪周期

    // 初始化 ValueAnimator(用于在一段时间内生成一系列值。这里用于驱动波浪的滚动。)
    private val animator = ValueAnimator.ofFloat(0f, period).apply {
        duration = 2000
        repeatCount = ValueAnimator.INFINITE
        interpolator = LinearInterpolator()
        addUpdateListener { animation ->
            offset = animation.animatedValue as Float
            invalidate()
        }
    }

    // 初始化动画
    init {
        // 启动动画
        animator.start() // 在 WaveView 被创建并初始化后，立即启动 ValueAnimator
    }

    override fun onDraw(canvas: Canvas) {
        super.onDraw(canvas)

        // 重置路径
        path.reset()
        path.moveTo(-period + offset, height.toFloat())

        var x = -period
        while (x <= width + period) {
            // 创建正弦波
            val y = height - amplitude * Math.sin(((x - offset) * 2 * Math.PI / period).toDouble()).toFloat()
            path.lineTo(x, y) // 绘制一条线段到指定的坐标
            x += 10
        }

        // 完成路径
        path.lineTo(width.toFloat(), height.toFloat())
        path.close()

        // 绘制波浪
        canvas.drawPath(path, paint)
    }

    // 当 View 从窗口中分离时，停止动画以避免内存泄漏
    override fun onDetachedFromWindow() {
        animator.cancel()
        super.onDetachedFromWindow()
    }
} 
```

