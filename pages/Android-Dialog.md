- 基础对话框
  collapsed:: true
	- 常用属性
		- setTitle
		- setMessage
		- create
		- show
	- 示例
		- ```java
		  btn.setOnClickListener(view -> {
		    AlertDialog.Builder builder = new AlertDialog.Builder(MainActivity.this);
		    builder.setTitle("警告");
		    builder.setMessage("你点击了按钮");
		    builder.setPositiveButton("确定", new DialogInterface.OnClickListener() {
		      @Override
		      public void onClick(DialogInterface dialogInterface, int i) {
		        Log.e("TAG", "点击了确定按钮");
		      }
		    });
		    builder.setNegativeButton("取消", null);
		    builder.show(); // 等于下面
		    //  AlertDialog dialog = builder.create();
		    //  dialog.show();
		  });
		  ```
- 自定义对话框
  collapsed:: true
	- 步骤：
		- 1. 设计自定义对话框样式 -> dialog_layout.xml
		  2. 设计style（去标题栏，去背景）
		  3. 讲第一步的布局应用到当前自定义对话框
		  4. 实例化对话框并展示(参数1:环境上下文，参数2:样式文件,R.style.dialog)
	- ```xml
	  // dialog_layout.xml
	  <?xml version="1.0" encoding="utf-8"?>
	  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	                android:layout_width="match_parent"
	                android:layout_height="match_parent"
	                android:orientation="vertical"
	                android:gravity="center_horizontal"
	                android:background="@color/purple_200">
	  
	    <TextView
	              android:layout_width="wrap_content"
	              android:layout_height="wrap_content"
	              android:text="确定要退出吗"
	              android:layout_marginTop="265dp"/>
	  
	    <LinearLayout
	                  android:layout_width="wrap_content"
	                  android:layout_height="wrap_content"
	                  android:orientation="horizontal"
	                  android:layout_margin="25dp">
	      <Button
	              android:id="@+id/cancel"
	              android:layout_width="120dp"
	              android:layout_height="120dp"
	              android:background="@color/black"
	              android:layout_marginRight="20dp"
	              android:text="取消"/>
	      <Button
	              android:id="@+id/ok"
	              android:layout_width="120dp"
	              android:layout_height="120dp"
	              android:background="@color/black"
	              android:text="确定"/>
	    </LinearLayout>
	  </LinearLayout>
	  ```
	- ```xml
	  // values/style.xml
	  <?xml version="1.0" encoding="utf-8"?>
	  <resources>
	    <style name="mydialog" parent="android:style/Theme.Dialog">
	      <item name="windowNoTitle">true</item>
	      <item name="android:windowBackground">@android:color/transparent</item>
	    </style>
	  </resources>
	  ```
	- ```java
	  // MyDialog.java
	  public class MyDialog extends Dialog {
	    public MyDialog(@NonNull Context context, int themeResId) {
	      super(context, themeResId);
	      setContentView(R.layout.dialog_layout);
	  
	      findViewById(R.id.cancel).setOnClickListener(view -> {
	        // 关闭
	        dismiss();
	      });
	      findViewById(R.id.ok).setOnClickListener(view -> {
	        Toast.makeText(context, "确定", Toast.LENGTH_SHORT).show();
	      });
	    }
	  }
	  ```
	- ```java
	  // MainActivity.java
	  btn.setOnClickListener(view -> {
	    // 复制样式id
	    MyDialog myDialog = new MyDialog(this, R.style.mydialog);
	    myDialog.show();
	  });
	  ```
- PopupWindow
  collapsed:: true
	- ```xml
	  <?xml version="1.0" encoding="utf-8"?>
	  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	      android:layout_width="wrap_content"
	      android:layout_height="wrap_content"
	      android:background="#00ffff"
	      android:padding="2dp">
	  
	      <TextView
	          android:id="@+id/sel"
	          android:layout_width="60dp"
	          android:layout_height="30dp"
	          android:text="选择"
	          android:textColor="#ffffff"
	          android:gravity="center"
	          android:background="#000000" />
	      <TextView
	          android:id="@+id/copy"
	          android:layout_width="60dp"
	          android:layout_height="30dp"
	          android:text="复制"
	          android:textColor="#ffffff"
	          android:gravity="center"
	          android:background="#000000" />
	      <TextView
	          android:id="@+id/paste"
	          android:layout_width="60dp"
	          android:layout_height="30dp"
	          android:text="粘贴"
	          android:textColor="#ffffff"
	          android:gravity="center"
	          android:background="#000000" />
	  </LinearLayout>
	  ```
	- ```java
	  public class MainActivity extends AppCompatActivity {
	      @Override
	      protected void onCreate(Bundle savedInstanceState) {
	          super.onCreate(savedInstanceState);
	          setContentView(R.layout.activity_main);
	  
	          Button btn = findViewById(R.id.popupBtn);
	          btn.setOnClickListener(view -> {
	              showPopupWindow(view);
	          });
	      }
	      public void showPopupWindow(View view) {
	          // 1. 初始化实例
	          // 参数1：用在弹窗中的view, 参数2、3：弹窗的宽高，参数4(focusable)：能否获取焦点
	          View v = LayoutInflater.from(this).inflate(R.layout.popup_layout, null);
	          PopupWindow pw = new PopupWindow(v, 480, 100, true);
	          // 2. 设置
	          // 设置背景、设置能相应外部的点击事件
	          pw.setBackgroundDrawable(new ColorDrawable(Color.TRANSPARENT));
	          pw.setOutsideTouchable(true);
	          pw.setTouchable(true);
	          // 3. 显示
	          // 参数1(anchor)：锚，参数2、3：相对于x、y方向上的偏移量
	          pw.showAsDropDown(view, 100, 50);
	          v.findViewById(R.id.sel).setOnClickListener(vw -> {
	              Toast.makeText(MainActivity.this, "选择", Toast.LENGTH_SHORT).show();
	          });
	          v.findViewById(R.id.copy).setOnClickListener(vw -> {
	              Toast.makeText(MainActivity.this, "复制", Toast.LENGTH_SHORT).show();
	          });
	      }
	  
	  }
	  ```
- PopupMenu(弹出式菜单)
  collapsed:: true
	- ```java
	  public class MainActivity extends AppCompatActivity {
	      @Override
	      protected void onCreate(Bundle savedInstanceState) {
	          super.onCreate(savedInstanceState);
	          setContentView(R.layout.activity_main);
	  
	          Button btn = findViewById(R.id.popupBtn);
	          btn.setOnClickListener(view -> {
	              showPopupMenu(view);
	          });
	      }
	  
	      public void showPopupMenu(View view) {
	          // 1. 实例化PopupMenu对象
	          PopupMenu menu = new PopupMenu(MainActivity.this, view);
	          // 2. 加载菜单资源：利用MenuInflater将Menu资源加载到PopupMenu.getMenu()所返回的Menu对象中
	          menu.getMenuInflater().inflate(R.menu.popup, menu.getMenu());
	          // 3. 为PopupMenu设置点击监听器
	          menu.setOnMenuItemClickListener(menuItem -> {
	              switch (menuItem.getItemId()) {
	                  case R.id.copy:
	                      Toast.makeText(MainActivity.this, "复制", Toast.LENGTH_SHORT).show();
	              }
	              return false;
	          });
	          // 显示
	          menu.show();
	  
	      }
	  
	  }
	  ```
- 适配器(Adapter)
	- ```java
	  public class MainActivity extends AppCompatActivity {
	  
	      @Override
	      protected void onCreate(Bundle savedInstanceState) {
	          super.onCreate(savedInstanceState);
	          setContentView(R.layout.activity_main);
	  
	          Button btn = findViewById(R.id.popupBtn);
	          btn.setOnClickListener(view -> {
	              showArrayAdaper();
	          });
	      }
	  
	      public void showArrayAdaper() {
	          final String[] items = {"java", "javascript", "go"};
	          // 参数1：环境
	          // 参数2：布局资源索引,每一项数据呈现的样式，android.R.layout.xxx
	          // 参数3：数据源
	  //        ArrayAdapter arrayAdapter = new ArrayAdapter(this, android.R.layout.simple_dropdown_item_1line, items);
	  
	          // 参数3：int textViewId指定文本需要放在布局中对应id文本控制的位置
	          ArrayAdapter arrayAdapter = new ArrayAdapter(this, R.layout.array_item_layout, R.id.item_text, items);
	          AlertDialog.Builder builder = new AlertDialog.Builder(this)
	                  .setTitle("请选择")
	                  // 参数1：适配器对象（对数据显示样式的规则制定器
	                  // 参数2：监听器
	                  .setAdapter(arrayAdapter, new DialogInterface.OnClickListener() {
	                      @Override
	                      public void onClick(DialogInterface dialogInterface, int i) {
	                          Toast.makeText(MainActivity.this, "点击:" + items[i], Toast.LENGTH_SHORT).show();
	                          dialogInterface.dismiss();
	                      }
	                  });
	          builder.show();
	      }
	  }
	  ```