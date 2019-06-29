## Android系统五大布局详解Layout

* Android中任何可视化的控件都是从***android.veiw.View***继承而来的

* 在xml中为创建控件时，需要为控件指定id,如：android:id="@+id/名字"系统会自动**在gen目录下**创建相应的R资源类变量

* **TableLayout表格布局**

  * TableLayout和TableRow都是LineLayout线性布局的**子类**

  * TableRow实际是一个横向的线性布局，且所以子元素宽度和高度一致。

  * 在TableLayout布局中，一列的宽度由该列中**最宽的**那个单元格指定

  * TableLayout中的特有属性：

    * android:collapseColumns

    *  android:shrinkColumns

    *  android:stretchColumns = "0,1,2,3"// 表示产生4个可拉伸的列,如下

    * ```xml
      <?xml version="1.0" encoding="utf-8"?>
      <TableLayout xmlns:android="http://schemas.android.com/apk/res/android"
          android:orientation="vertical"
          android:shrinkColumns="0,1,2"  
                   // 设置三列都可以收缩  
          android:stretchColumns="0,1,2"
                   // 设置三列都可以拉伸 如果不设置这个，那个显示的表格将不能填慢整个屏幕
          android:layout_width="fill_parent"
          android:layout_height="fill_parent" >  
      
      </TableLayout>
      ```

- **layout_padding** ----用于设置控件内容相对于控件边缘的边距

- 

  ![img](https://upload-images.jianshu.io/upload_images/944365-d85fdfd5efd9e5be.png?imageMogr2/auto-orient/)

  ConstraintLayout类似与relativeLayout,但是更加强大

- `layout_gravity` 一般作用于 **LeanerLayout** 和 FrameLayout，非relativeLayout

* **选择器的使用**

​     在`drawable`添加 `selector.xml` 资源文件

​     **button_selector.xml:**

```xml
<?xml version="1.0" encoding="UTF-8"?>
< selector xmlns:android="http://schemas.android.com/apk/res/android">

 < !-- 指定按钮按下时的图片 -->
 <item android:state_pressed="true"  
       android:drawable="@drawable/start_down"
 />

 < !-- 指定按钮松开时的图片 --> 
 <item android:state_pressed="false"
       android:drawable="@drawable/start"
 />

< /selector>
```

**在布局文件main.xml中控件的属性设置:**

```xml
<Button
  android:id="@+id/startButton"
  android:layout_width="wrap_content"
  android:layout_height="wrap_content"
  android:background="@drawable/button_selector" 
/>
```

* **ConstraintLayout**

  见印象笔记









