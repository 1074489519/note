- 静态加载
	- ```xml
	  <?xml version="1.0" encoding="utf-8"?>
	  <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
	      android:layout_width="match_parent"
	      android:layout_height="match_parent"
	      android:background="@color/purple_200">
	  
	      <fragment
	          android:id="@+id/listFragment"
	          android:name="com.example.demo.ListFragment"
	          android:layout_width="300dp"
	          android:layout_height="200dp"
	          android:layout_centerInParent="true"
	          />
	  </RelativeLayout>
	  ```
	- ```java
	  // Fragment.java
	  public class ListFragment extends Fragment {
	      @Nullable
	      @Override
	      public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
	          // 1:布局文件，2：fragment所在的viewGroup，3：是否绑定在根布局
	          View view = inflater.inflate(R.layout.test_fragment, container, false);
	          return view;
	      }
	  }
	  ```
- 动态加载
	- ```java
	  LinearLayout view = findViewById(R.id.listContainer);
	  ListFragment listFragment = new ListFragment();
	  // 开启事务，把fragment提交到container中
	  getSupportFragmentManager()
	    .beginTransaction()
	    .add(R.id.listContainer, listFragment)
	    // .remove .replace
	    .commit();
	  ```
- Activity向Fragment传惨
	- ```java
	  // Fragment.java
	  public class ListFragment extends Fragment {
	      public static final String BUNDLE_TITLE = "bundle_title";
	      private String title = "";
	  
	      public static ListFragment newInstance(String title) {
	          ListFragment listFragment = new ListFragment();
	          Bundle bundle = new Bundle();
	          bundle.putString(BUNDLE_TITLE, title);
	          listFragment.setArguments(bundle);
	          return listFragment;
	      }
	  
	      @Override
	      public void onCreate(@Nullable Bundle savedInstanceState) {
	          super.onCreate(savedInstanceState);
	          if (getArguments() != null) {
	              title = getArguments().getString(BUNDLE_TITLE);
	          }
	      }
	  
	      @Nullable
	      @Override
	      public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
	          // 1:布局文件，2：fragment所在的viewGroup，3：是否绑定在根布局
	          View view = inflater.inflate(R.layout.test_fragment, container, false);
	          TextView textView = view.findViewById(R.id.textview1);
	          textView.setText(title);
	          return view;
	      }
	  }
	  ```
	- ```java
	  // Activity.java
	  getSupportFragmentManager()
	    .beginTransaction()
	    .add(R.id.listContainer, ListFragment.newInstance("标题"))
	    .commit();
	  ```