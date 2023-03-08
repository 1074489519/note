- 选项菜单(OptionMenu)
	- ```xml
	  <?xml version="1.0" encoding="utf-8"?>
	  <menu xmlns:android="http://schemas.android.com/apk/res/android">
	      <item 
	            android:id="@+id/save"
	            android:title="保存" 
	            android:icon="@mipmap/ic_launche" // 显示图标
	            app:showAsAction="always|withText" // always:直接显示在操作栏中, withText:图标和文本同时存在，isRoom:有空间就展示
	      />
	      <item android:id="@+id/setting" android:title="设置" />
	      <item android:title="更多操作">
	          <menu>
	              <item android:title="子菜单1" />
	              <item android:title="子菜单2" />
	          </menu>
	      </item>
	  </menu>
	  ```
	- ```java
	  public class MainActivity extends AppCompatActivity {
	    @Override
	    protected void onCreate(Bundle savedInstanceState) {
	      super.onCreate(savedInstanceState);
	      ...
	      }
	    @Override
	    public boolean onCreateOptionsMenu(Menu menu) {
	      // 加载菜单资源
	      getMenuInflater().inflate(R.menu.options, menu);
	      return true;
	    }
	    @Override
	    public boolean onOptionsItemSelected(@NonNull MenuItem item) {
	      // 选中菜单后的调用方法
	      switch (item.getItemId()) {// 获取菜单项id
	        case R.id.save:
	          Toast.makeText(this, "保存", Toast.LENGTH_SHORT).show();
	        case R.id.setting:
	          Toast.makeText(this, "设置", Toast.LENGTH_SHORT).show();
	      }
	      return super.onOptionsItemSelected(item);
	    }
	  }
	  ```
- 上下文菜单(ContextMenu)
	- 在中间展示：
	- ```java
	  public class MainActivity extends AppCompatActivity {
	      @Override
	      protected void onCreate(Bundle savedInstanceState) {
	          super.onCreate(savedInstanceState);
	          setContentView(R.layout.activity_main);
	          // 1. 注册
	          registerForContextMenu(findViewById(R.id.contextMenuBtn));
	          // 2. 覆盖 onCreateContextMenu
	          // 3. 菜单项的操作 覆盖 onContextItemSelected
	      }
	  
	      @Override
	      public void onCreateContextMenu(ContextMenu menu, View v, ContextMenu.ContextMenuInfo menuInfo) {
	          getMenuInflater().inflate(R.menu.context, menu);
	      }
	      @Override
	      public boolean onContextItemSelected(@NonNull MenuItem item) {
	          switch (item.getItemId()) {
	              case R.id.copy:
	                  Toast.makeText(this, "复制", Toast.LENGTH_SHORT).show();
	              case R.id.del:
	                  Toast.makeText(this, "删除", Toast.LENGTH_SHORT).show();
	              case R.id.rename:
	                  Toast.makeText(this, "重命名", Toast.LENGTH_SHORT).show();
	          }
	          return super.onContextItemSelected(item);
	      }
	  }
	  ```
	- 在顶部展示操作栏
	- ```java
	  public class MainActivity extends AppCompatActivity {
	      @Override
	      protected void onCreate(Bundle savedInstanceState) {
	          super.onCreate(savedInstanceState);
	          setContentView(R.layout.activity_main);
	          // 1. 为按钮设置上下文操作模式
	          //  1) 实现ActionMode CallBack
	          //  2) 在view的长安事件中去启动上下文操作模式
	          findViewById(R.id.contextMenuBtn).setOnLongClickListener(view -> {
	              startActionMode(cb);
	              return false;
	          });
	      }
	  
	      ActionMode.Callback cb = new ActionMode.Callback() {
	          // 创建，在启动上下文操作模式（startActionMode(Callback))时调用
	          @Override
	          public boolean onCreateActionMode(ActionMode actionMode, Menu menu) {
	              Log.e("TAG", "创建");
	              getMenuInflater().inflate(R.menu.context, menu);
	              return true;
	          }
	  
	          // 在创建方法后进行调用
	          @Override
	          public boolean onPrepareActionMode(ActionMode actionMode, Menu menu) {
	              Log.e("TAG", "准备");
	              return false;
	          }
	  
	          // 点击时调用
	          @Override
	          public boolean onActionItemClicked(ActionMode actionMode, MenuItem menuItem) {
	              Log.e("TAG", "点击");
	              switch (menuItem.getItemId()) {
	                  case R.id.copy:
	                      Toast.makeText(MainActivity.this, "复制", Toast.LENGTH_SHORT).show();
	                  case R.id.del:
	                      Toast.makeText(MainActivity.this, "删除", Toast.LENGTH_SHORT).show();
	                  case R.id.rename:
	                      Toast.makeText(MainActivity.this, "重命名", Toast.LENGTH_SHORT).show();
	              }
	              return true;
	          }
	  
	          // 上下文操作模式结束时被调用
	          @Override
	          public void onDestroyActionMode(ActionMode actionMode) {
	              Log.e("TAG", "结束");
	          }
	      };
	  }
	  ```
- 弹出菜单(PopupMenu)
	- ```java
	  protected void onCreate(Bundle savedInstanceState) {
	    super.onCreate(savedInstanceState);
	    setContentView(R.layout.activity_main);
	  
	    Button btn = findViewById(R.id.popupBtn);
	    btn.setOnClickListener(view -> {
	      // 1. 实例化PopupMenu对象(参数2：被锚定的view)
	      PopupMenu menu = new PopupMenu(MainActivity.this, view);
	      // 2. 加载菜单资源：利用MenuInflater讲Menu资源加载到PopupMenu.getMenu()所返回的Menu对象
	      // 讲R.menu.xxx对应的资源加载到弹出式菜单中
	      menu.getMenuInflater().inflate(R.menu.context, menu.getMenu());
	      // 3. 为PopupMenu设置点击监听器
	      menu.setOnMenuItemClickListener(menuItem -> {
	        switch (menuItem.getItemId()) {
	          case R.id.copy:
	            Toast.makeText(MainActivity.this, "拷贝", Toast.LENGTH_SHORT).show();
	          default:
	            Toast.makeText(MainActivity.this, "点了其他", Toast.LENGTH_SHORT).show();
	        }
	        return false;
	      });
	      // 4. 显示
	      menu.show();
	    });
	  }
	  ```
-