### 自定义View

1. 自定义View我们大部分时候只需重写**两个**函数：onMeasure()、onDraw()。onMeasure负责对当前View的尺寸进行测量，onDraw负责把当前这个View绘制出来。

2. ```java
   protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) 
   ```

​       widthMeasureSpec,heightMeasureSpec都是int整数，但这个整数是测量模式（2bit）和尺寸大小（30bit）合成的，测量模式有3种：`UNSPECIFIED`，`EXACTLY`，`AT_MOST`，用2bit表示足以。可以用以下方式获取测量模式和尺寸的大小

```java
int widthMode = MeasureSpec.getMode(widthMeasureSpec);
int widthSize = MeasureSpec.getSize(widthMeasureSpec);
```

3. ViewGroup是个View容器，它装纳child View并且负责把child View放入指定的位置。

