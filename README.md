# FlexboxLayout
[ ![Circle CI](https://circleci.com/gh/google/flexbox-layout.svg?style=shield&circle-token=2a42716dfffab73d73c5ce7ed7b3ee620cfa137b) ](https://circleci.com/gh/google/flexbox-layout/tree/master)
[ ![Download](https://api.bintray.com/packages/google/flexbox-layout/flexbox/images/download.svg) ](https://bintray.com/google/flexbox-layout/flexbox/_latestVersion)

FlexboxLayout是一个为Android提供的和[CSS弹性框布局模块]（https://www.w3.org/TR/css-flexbox-1）有类似的功能库

## 安装
将下面的依赖添加到自己的 `build.gradle` 文件:

```
dependencies {
    compile 'com.google.android:flexbox:0.1.3'
}
```

## 用法
FlexboxLayout 像LinearLayout和RelativeLayout一样继承自ViewGroup。你可以在XML布局文件中指定属性，像下面这样:
```xml
<com.google.android.flexbox.FlexboxLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    app:flexWrap="wrap"
    app:alignItems="stretch"
    app:alignContent="stretch" >

    <TextView
        android:id="@+id/textview1"
        android:layout_width="120dp"
        android:layout_height="80dp"
        app:layout_flexBasisPercent="50%"
        />

    <TextView
        android:id="@+id/textview2"
        android:layout_width="80dp"
        android:layout_height="80dp"
        app:layout_alignSelf="center"
        />

    <TextView
        android:id="@+id/textview3"
        android:layout_width="160dp"
        android:layout_height="80dp"
        app:layout_alignSelf="flex_end"
        />
</com.google.android.flexbox.FlexboxLayout>
```

或者在java代码中编写，像这样:
```java
FlexboxLayout flexboxLayout = (FlexboxLayout) findViewById(R.id.flexbox_layout);
flexboxLayout.setFlexDirection(FlexboxLayout.FLEX_DIRECTION_COLUMN);

View view = flexboxLayout.getChildAt(0);
FlexboxLayout.LayoutParams lp = (FlexboxLayout.LayoutParams) view.getLayoutParams();
lp.order = -1;
lp.flexGrow = 2;
view.setLayoutParams(lp);
```

## 支持的属性
你可以给FlexboxLayout指定如下属性:
* flexDirection —— 弹性方向
  * Children items被放置在FlexboxLayout里面，它确定主轴的方向（横轴，与主轴垂直）。
  可用的值有:
    * row (default) —— 一横排
    * row_reverse —— 横排反向（从右到左）
    * column —— 一纵列（从上到下）
    * column_reverse —— 纵列反向（从下到上）

    ![Flex Direction explanation](/assets/flex-direction.gif)

* flexWrap —— 弹性包裹
    * 这个属性控制弹性容器是单行还是多行（单列or多列），以及横轴的方向。
        可用的值有：
        * nowrap (default) —— 默认值，不包裹内容，所有内容一列或一行显示，如果FlexboxLayou的父布局宽度固定，新添加的条目会和原来的挤在一起，平分宽度。
        * wrap —— 包裹内容。flexDirection为row模式下：如果FlexboxLayout的父布局是固定宽度的，那么一行放置不下，自动放到下一行。如果是类似HorizontalScrollView的，新加的条目会自动追加到末尾。
        flexDirection为column模式时，情况和上面类似。
        * wrap_reverse —— 内容反向

    ![Flex Wrap explanation](/assets/flex-wrap.gif)

* justifyContent —— 内容对齐（相对于主轴，row或column方向）
  * 此属性控制内容的对齐方式（相对于主轴）可用的值有：
    * flex_start (default) —— 对齐到开始（默认）
    * flex_end —— 末尾对齐（右对齐）
    * center —— 居中
    * space_between 两端对齐
    * space_around 分散对齐

    ![Justify Content explanation](/assets/justify-content.gif)

* alignItems —— 条目对齐
  * 此属性控制横轴（相对于主轴的轴，不是横向，下同）的对齐方式。可用的值有：
    * stretch (default) —— 伸展，使控件在横轴上填充
    * flex_start —— 对齐到横轴的开始
    * flex_end ——对齐到横轴的末尾
    * center —— 居于横轴居中
    * baseline —— 在主轴是column时，效果和flex_start类似。在主轴是row时，效果和center属性类似

    ![Align Items explanation](/assets/align-items.gif)

* alignContent —— 内容对齐
   * 控制整个控件内容对齐方式，当FlexboxLayout为RootView，且其宽和高都是固定值或填充屏幕时有效：
    * stretch (default) —— 伸展，内容在屏幕上填充
    * flex_start —— 全部堆在开始
    * flex_end —— 全部堆在结束
    * center —— 全部堆在中间
    * space_between 两端对齐
    * space_around 分散对齐

    ![Align Content explanation](/assets/align-content.gif)

也可以给FlexboxLayout的子控件指定以下属性。

* layout_order
  * 此属性能够改变已布局的子控件的顺序，默认情况下，子控件会按照XML文件中的顺序显示。如果没有指定， 默认值是`1` 

* layout_flexGrow
  *  此属性决定了
  * This attribute determines how much this child will grow if positive free space is
  distributed relative to the rest of other flex items included in the same flex line.
  If not specified, `0` is set as a default value.

* layout_flexShrink
  * This attribute determines how much this child will shrink if negative free space is
  distributed relative to the rest of other flex items included in the same flex line.
  If not specified, `1` is set as a default value.

* layout_alignSelf
  * This attribute determines the alignment along the cross axis (perpendicular to the
  main axis). The alignment in the same direction can be determined by the
  `alignItems` in the parent, but if this is set to other than
  `auto`, the cross axis alignment is overridden for this child. Possible values are:
    * auto (default)
    * flex_start
    * flex_end
    * center
    * baseline
    * stretch

* layout_flexBasisPercent
  * The initial flex item length in a fraction format relative to its parent.
  The initial main size of this child view is trying to be expanded as the specified
  fraction against the parent main size.
  If this value is set, the length specified from `layout_width`
  (or `layout_height`) is overridden by the calculated value from this attribute.
  This attribute is only effective when the parent's length is definite (MeasureSpec mode is
  `MeasureSpec.EXACTLY`). The default value is `-1`, which means not set.

## Known differences from the original CSS specification
This library tries to achieve the same capabilities of the original
[Flexible Box specification](https://www.w3.org/TR/css-flexbox-1) as much as possible,
but due to some reasons such as the way specifying attributes can't be the same between
CSS and Android XML, there are some known differences from the original specification.

(1) There is no [flex-flow](https://www.w3.org/TR/css-flexbox-1/#flex-flow-property)
equivalent attribute
  * Because `flex-flow` is a shorthand for setting the `flex-direction` and `flex-wrap` properties,
  specifying two attributes from a single attribute is not practical in Android.

(2) There is no [flex](https://www.w3.org/TR/css-flexbox-1/#flex-property) equivalent attribute
  * Likewise `flex` is a shorthand for setting the `flex-grow`, `flex-shrink` and `flex-basis`,
  specifying those attributes from a single attribute is not practical.

(3) `layout_flexBasisPercent` is introduced instead of
  [flexBasis](https://www.w3.org/TR/css-flexbox-1/#flex-basis-property)
  * Both `layout_flexBasisPercent` in this library and `flex-basis` property in the CSS are used to
  determine the initial length of an individual flex item. The `flex-basis` property accepts width
  values such as `1em`, `10px`, and `content` as strings as well as percentage values such as
  `10%` and `30%`, whereas `layout_flexBasisPercent` only accepts percentage values.
  But specifying initial fixed width values can be done by specifying width (or height) values in
  layout_width (or layout_height, varies depending on the `flexDirection`). Also, the same
  effect can be done by specifying "wrap_content" in layout_width (or layout_height) if
  developers want to achieve the same effect as 'content'. Thus, `layout_flexBasisPercent` only
  accepts percentage values, which can't be done through layout_width (or layout_height) for
  simplicity.

(4) min-width and min-height can't be specified
  * Which isn't implemented just yet.

## How to make contributions
Please read and follow the steps in [CONTRIBUTING.md](https://github.com/google/flexbox-layout/blob/master/CONTRIBUTING.md)

## License
Please see [License](https://github.com/google/flexbox-layout/blob/master/LICENSE)
