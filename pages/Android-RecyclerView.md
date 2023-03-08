- Fruit.java
  collapsed:: true
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
- FruitAdapter.java
  collapsed:: true
	- ```java
	  public class FruitAdapter extends RecyclerView.Adapter<FruitAdapter.ViewHolder> {
	    List<Fruit> fruitList;
	    public FruitAdapter(List<Fruit> fruitList) {
	      this.fruitList = fruitList;
	    }
	    static class ViewHolder extends RecyclerView.ViewHolder {
	      private ImageView fruitImage;
	      private TextView fruitName;
	  
	      public ViewHolder(View view) {
	        super(view);
	        this.fruitImage = view.findViewById(R.id.fruitImage);
	        this.fruitName = view.findViewById(R.id.fruitName);
	      }
	    }
	    @NonNull
	    @Override
	    public ViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
	      View view = LayoutInflater.from(parent.getContext()).inflate(R.layout.fruit_item, parent, false);
	      ViewHolder viewHolder = new ViewHolder(view);
	      // 添加点击事件
	      viewHolder.itemView.setOnClickListener(view1 -> {
	        // 添加整体事件
	        Toast.makeText(parent.getContext(), "you clicked view", Toast.LENGTH_SHORT).show();
	      });
	      viewHolder.fruitImage.setOnClickListener(view1 -> {
	        // 添加某一个view事件
	        Toast.makeText(parent.getContext(), "you clicked image", Toast.LENGTH_SHORT).show();
	      });
	      return viewHolder;
	    }
	  
	    @Override
	    public void onBindViewHolder(@NonNull ViewHolder holder, int position) {
	      Fruit fruit = fruitList.get(position);
	      holder.fruitImage.setImageResource(fruit.imageId);
	      holder.fruitName.setText(fruit.name);
	    }
	    @Override
	    public int getItemCount() {
	      return fruitList.size();
	    }
	  }
	  ```
- MainActivity.java
  collapsed:: true
	- ```java
	  public class MainActivity extends AppCompatActivity {
	      private static final String TAG = "MainActivity";
	      ArrayList<Fruit> data = new ArrayList<Fruit>();
	  
	      @Override
	      protected void onCreate(Bundle savedInstanceState) {
	          super.onCreate(savedInstanceState);
	          setContentView(R.layout.activity_main);
	  
	          initFruits();
	  
	          LinearLayoutManager layoutManager = new LinearLayoutManager(this);
	          // 设置方向
	        	// layoutManager.setOrientation(LayoutManager.HORIZONTAL);
	        	RecyclerView recyclerView = findViewById(R.id.recylerView);
	          recyclerView.setLayoutManager(layoutManager);
	          FruitAdapter adapter = new FruitAdapter(data);
	          recyclerView.setAdapter(adapter);
	      }
	  
	  
	      private void initFruits() {
	          data.add(new Fruit("Apple", R.drawable.main_tab_icon_home));
	          data.add(new Fruit("Banana", R.drawable.main_tab_icon_home));
	          data.add(new Fruit("Orange", R.drawable.main_tab_icon_home));
	      }
	  }
	  ```
- main.xml
  collapsed:: true
	- ```xml
	  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	      android:layout_width="match_parent"
	      android:layout_height="match_parent">
	  
	      <androidx.recyclerview.widget.RecyclerView
	          android:id="@+id/recylerView"
	          android:layout_width="match_parent"
	          android:layout_height="match_parent" />
	  
	  </LinearLayout>
	  ```
- fruit_item.xml
  collapsed:: true
	- ```java
	  <?xml version="1.0" encoding="utf-8"?>
	  <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	      android:layout_width="match_parent"
	      android:layout_height="wrap_content"
	      android:padding="20dp">
	  
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
-