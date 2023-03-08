- 布局分类
	- 线性布局(LinearLayout)
		- 属性：
			- 方向：`android:orientation`
				- 垂直：`vertical`
				- 水平：`horizontal`
			- 权重：`android:layout_weight`
				- 相当于flex: 1，注意：layout_width设置为0dp
			- 重力：`android:layout_gravity`
				- 值：bottom,center,top,left...(可组合)
	- 相对布局(RelativeLayout)
	  collapsed:: true
		- 相对于父容器
			- 取值：`true`/`false`
			- 如：
				- `android:layout_centerInParent`
				- `android:layout_alignParentRight`
				- `android:layout_alignParentLeft`
				- `android:layout_alignParentTop`
				- `android:layout_alignParentBottom`
				- 水平居中：`android:layout_centerHorizontal`
				- 垂直居中：`android:layout_centerVertical`
		- 相对于其他空间
			- 取值：其他控件id
			- 如：
				- 在参照物的某边
					- `android:layout_toRightOf`
					- `android:layout_toLeftOf`
					- 谁的下面：`layout_above`
					- 谁的下面：`layout_below`
				- 和参照物的某边线对齐
					- 和上边线对齐：`layout_alignTop`
					- `layout_alignBottom`
					- `layout_alignLeft`
					- `layout_alignRight`
	- 帧布局(FrameLayout)
	  collapsed:: true
		- 重要属性
			- 控制重力：`android:layout_gravity`
				- 多组合：`android:layout_gravity="right|bottom"`
			- 前景：`android:foreground`
			- 前景重力：`android:foregroundGravity`
	- 表格布局(TableLayout)
	  collapsed:: true
		- 属性
			- 可伸展的列：`android:stretchColumns`
			- 可收缩的列：`android:shrinkColumns`
			- 可隐藏的列：`android:collapseColumns`
		- 案例
			- ```xml
			  <TableLayout ...
			               android:stretchColumns="0,1,2,3" // *为所有
			               >
			    <TableRow>
			      <Button android:text="7">
			        <Button android:text="8">
			          ...
			      </TableRow>
			    </TableRow>
			  </TableLayout>
			  ```
	- 网格布局(GridLayout)
	  collapsed:: true
		- 属性
			- 行数量：`android:rowCount`
			- 列数量：`android:columnCount`
			- 位于第几行：`android:layout_row` / `android:layout_column`
			- 跨几行：`android:layout_rowSpan`/`android:layout_columnSpan`
				- 跨行搭配`android:layout_gravity="fill"`使用
	- 约束布局(ConstraintLayout)
- 布局重要属性
  collapsed:: true
	- 宽度：`android:layout_width`
	- 高度：`android:layout_height`
		- 值：
			- 根据夫大小适配：`match_parent`
			- 根据内容撑开：`wrap_content`
			- 数值：`200dp`
	- 内边距：`android:layout_padding`
	- 外边距：`android:layout_margin`
-