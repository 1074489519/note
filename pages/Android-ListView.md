- 简单使用
  collapsed:: true
	- ```xml
	  <?xml version="1.0" encoding="utf-8"?>
	  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	      android:id="@+id/tab_host"
	      android:layout_width="match_parent"
	      android:layout_height="match_parent"
	      android:orientation="vertical">
	  
	  
	      <ListView
	          android:id="@+id/listView"
	          android:layout_width="match_parent"
	          android:layout_height="match_parent" />
	  </LinearLayout>
	  ```
	- ```java
	  protected void onCreate(Bundle savedInstanceState) {
	    super.onCreate(savedInstanceState);
	    setContentView(R.layout.activity_main);
	    String[] data = { "Apple", "Banana", "Orange" };
	    ListView listView = findViewById(R.id.listView);
	    ArrayAdapter adapter = new ArrayAdapter<String>(this, android.R.layout.simple_dropdown_item_1line, data);
	    listView.setAdapter(adapter);
	  }
	  ```
- 定制
  collapsed:: true
	- Fruit.java
		- ```java
		  public class Fruit {
		      String name;
		      int imageId;
		  
		      public Fruit(String name, int imageId) {
		          this.name = name;
		          this.imageId = imageId;
		      }
		  }
		  ```
	- fruit_item.xml
		- ```xml
		  <?xml version="1.0" encoding="utf-8"?>
		  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
		      android:layout_width="match_parent"
		      android:layout_height="match_parent">
		  
		      <ImageView
		          android:id="@+id/fruitImage"
		          android:layout_width="40dp"
		          android:layout_height="40dp"
		          android:layout_gravity="center_horizontal"
		          android:layout_marginLeft="10dp" />
		  
		      <TextView
		          android:id="@+id/fruitName"
		          android:layout_width="wrap_content"
		          android:layout_height="wrap_content"
		          android:layout_gravity="center_vertical"
		          android:layout_marginLeft="10dp" />
		  </LinearLayout>
		  ```
	- FruitAdapter.java
		- ```java
		  public class FruitAdapter extends ArrayAdapter<Fruit> {
		      private final Context context;
		      private final int resourceId;
		      private final List<Fruit> data;
		  
		      public FruitAdapter(@NonNull Context context, int resource, @NonNull List<Fruit> data) {
		          super(context, resource, data);
		  
		          this.context = context;
		          this.resourceId = resource;
		          this.data = data;
		  
		      }
		  
		      class ViewHolder {
		          ImageView fruitImage;
		          TextView fruitName;
		          public ViewHolder(ImageView fruitImage, TextView fruitName) {
		              this.fruitImage = fruitImage;
		              this.fruitName = fruitName;
		          }
		      }
		  
		      @NonNull
		      @Override
		      public View getView(int position, @Nullable View convertView, @NonNull ViewGroup parent) {
		          View view;
		          ViewHolder viewHolder;
		          if (convertView == null) {
		              view = LayoutInflater.from(context).inflate(resourceId, parent, false);
		              ImageView fruitImage = view.findViewById(R.id.fruitImage);
		              TextView fruitName = view.findViewById(R.id.fruitName);
		              // 将ViewHolder对象存储在View中
		              viewHolder = new ViewHolder(fruitImage, fruitName);
		              view.setTag(viewHolder);
		          } else {
		              view = convertView;
		              viewHolder = (ViewHolder) view.getTag();
		          }
		  
		          Fruit fruit = getItem(position);
		          if (fruit != null) {
		              viewHolder.fruitImage.setImageResource(fruit.imageId);
		              viewHolder.fruitName.setText(fruit.name);
		          }
		          return view;
		      }
		  }
		  
		  ```
	- MainActivity.java
		- ```java
		  public class MainActivity extends AppCompatActivity {
		      private static final String TAG = "MainActivity";
		  
		      ArrayList<Fruit> data = new ArrayList<Fruit>();
		  
		      @Override
		      protected void onCreate(Bundle savedInstanceState) {
		          super.onCreate(savedInstanceState);
		          setContentView(R.layout.activity_main);
		  
		          initFruits();
		          ListView listView = findViewById(R.id.listView);
		          FruitAdapter adapter = new FruitAdapter(this, R.layout.fruit_item, data);
		          listView.setAdapter(adapter);
		          listView.setOnItemClickListener((parent, view, position, id) -> {
		              Fruit fruit = data.get(position);
		              Toast.makeText(this, fruit.name, Toast.LENGTH_SHORT).show();
		          });
		      }
		  
		      private void initFruits() {
		          data.add(new Fruit("Apple", R.drawable.main_tab_icon_home));
		          data.add(new Fruit("Banana", R.drawable.main_tab_icon_home));
		          data.add(new Fruit("Orange", R.drawable.main_tab_icon_home));
		      }
		  
		  }
		  ```