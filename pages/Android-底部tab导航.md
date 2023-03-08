- MainActivity.java
	- ```java
	  public class MainActivity extends AppCompatActivity implements TabHost.TabContentFactory {
	      private ViewPager viewPager;
	  
	      @Override
	      protected void onCreate(Bundle savedInstanceState) {
	          super.onCreate(savedInstanceState);
	          setContentView(R.layout.activity_main);
	  
	          // 初始化总布局
	          TabHost tabHost = findViewById(R.id.tab_host);
	          tabHost.setup();
	  
	          // 三个tab做处理
	          // 1. init data
	          int[] titleIds = {
	                  R.string.home, R.string.message, R.string.me
	          };
	          int[] drawableIds = {
	                  R.drawable.main_tab_icon_home, R.drawable.main_tab_icon_home, R.drawable.main_tab_icon_home
	          };
	          // data --> view
	          for (int i = 0; i < titleIds.length; i++) {
	              View view = getLayoutInflater().inflate(R.layout.my_tab_layout, null, false);
	              ImageView icon = view.findViewById(R.id.main_tab_icon);
	              TextView title = view.findViewById(R.id.main_tab_text);
	              View tab = view.findViewById(R.id.tab_bg);
	              icon.setImageResource(drawableIds[i]);
	              title.setText(titleIds[i]);
	              tab.setBackgroundColor(getResources().getColor(R.color.white));
	              tabHost.addTab(
	                      tabHost.newTabSpec(getString(titleIds[i]))
	                              .setIndicator(view)
	                              .setContent(this)
	              );
	  
	          }
	  
	          // 三个fragment组成的viewPager
	          Fragment[] fragments = new Fragment[]{
	                  TestFragment.newInstance("home"),
	                  TestFragment.newInstance("message"),
	                  TestFragment.newInstance("my"),
	          };
	  
	          viewPager = findViewById(R.id.view_page);
	          viewPager.setAdapter(new FragmentPagerAdapter(getSupportFragmentManager()) {
	              @NonNull
	              @Override
	              public Fragment getItem(int position) {
	                  return fragments[position];
	              }
	  
	              @Override
	              public int getCount() {
	                  return fragments.length;
	              }
	          });
	          viewPager.addOnPageChangeListener(new ViewPager.OnPageChangeListener() {
	              @Override
	              public void onPageScrolled(int position, float positionOffset, int positionOffsetPixels) {
	  
	              }
	  
	              @Override
	              public void onPageSelected(int position) {
	                  if (tabHost != null) {
	                      tabHost.setCurrentTab(position);
	                  }
	              }
	  
	              @Override
	              public void onPageScrollStateChanged(int state) {
	  
	              }
	          });
	          tabHost.setOnTabChangedListener(new TabHost.OnTabChangeListener() {
	              @Override
	              public void onTabChanged(String s) {
	                  if (tabHost != null) {
	                      int position = tabHost.getCurrentTab();
	                      viewPager.setCurrentItem(position);
	                  }
	              }
	          });
	      }
	  
	  
	      @Override
	      public View createTabContent(String s) {
	          View view = new View(this);
	          view.setMinimumHeight(0);
	          view.setMinimumWidth(0);
	          return view;
	      }
	  }
	  ```
- TestFragment.java
	- ```java
	  public class TestFragment extends Fragment {
	      public static final String TITLE = "position";
	      private String mTitle;
	  
	      public static TestFragment newInstance(String title) {
	          TestFragment fragment = new TestFragment();
	          Bundle b = new Bundle();
	          b.putString(TITLE, title);
	          fragment.setArguments(b);
	          return fragment;
	      }
	  
	      @Override
	      public void onCreate(@Nullable Bundle savedInstanceState) {
	          super.onCreate(savedInstanceState);
	          if(getArguments() != null) {
	              mTitle = String.valueOf(getArguments().getString(TITLE));
	          }
	      }
	  
	      @Nullable
	      @Override
	      public View onCreateView(@NonNull LayoutInflater inflater, @Nullable ViewGroup container, @Nullable Bundle savedInstanceState) {
	          View view = inflater.inflate(R.layout.test_fragment, container, false);
	          TextView textView = view.findViewById(R.id.text_view);
	          textView.setText(mTitle);
	          return view;
	      }
	  }
	  
	  ```
- activity_main.xml
  collapsed:: true
	- ```xml
	  <?xml version="1.0" encoding="utf-8"?>
	  <TabHost xmlns:android="http://schemas.android.com/apk/res/android"
	      android:id="@+id/tab_host"
	      android:layout_width="match_parent"
	      android:layout_height="match_parent"
	      android:orientation="vertical">
	  
	      <RelativeLayout
	          android:layout_width="match_parent"
	          android:layout_height="match_parent">
	  
	          <androidx.viewpager.widget.ViewPager
	              android:id="@+id/view_page"
	              android:layout_width="match_parent"
	              android:layout_height="match_parent"
	              android:layout_above="@id/tab_divider"
	              />
	  
	  
	          <FrameLayout
	              android:id="@android:id/tabcontent"
	              android:layout_width="match_parent"
	              android:layout_height="match_parent"
	              android:layout_above="@id/tab_divider"
	              android:visibility="visible">
	  
	          </FrameLayout>
	  
	          <View
	              android:id="@+id/tab_divider"
	              android:layout_width="match_parent"
	              android:layout_height="1dp"
	              android:background="#dfdfdf"
	              android:layout_above="@android:id/tabs"/>
	  
	          <TabWidget
	              android:id="@android:id/tabs"
	              android:layout_width="match_parent"
	              android:layout_height="60dp"
	              android:layout_alignParentBottom="true"
	              android:showDividers="none">
	  
	          </TabWidget>
	      </RelativeLayout>
	  </TabHost>
	  ```
- my_tab_layout.xml
  collapsed:: true
	- ```xml
	  <?xml version="1.0" encoding="utf-8"?>
	  <RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
	      android:id="@+id/tab_bg"
	      android:layout_width="match_parent"
	      android:layout_height="match_parent"
	      android:orientation="vertical">
	      <LinearLayout
	          android:id="@+id/main_content"
	          android:layout_width="wrap_content"
	          android:layout_height="wrap_content"
	          android:gravity="center"
	          android:layout_centerInParent="true"
	          android:orientation="vertical">
	          <ImageView
	              android:id="@+id/main_tab_icon"
	              android:layout_width="30dp"
	              android:layout_height="30dp"
	              android:layout_marginTop="4dp"
	              android:src="@mipmap/ic_launcher" />
	          <TextView
	              android:id="@+id/main_tab_text"
	              android:layout_width="wrap_content"
	              android:layout_height="wrap_content"
	              android:layout_marginTop="4dp"
	              android:textColor="@color/color_main_tab_txt"
	              android:text="@string/app_name" />
	      </LinearLayout>
	  </RelativeLayout>
	  ```
- res/color/color_main_tab_txt.xml
	- ```xml
	  <?xml version="1.0" encoding="utf-8"?>
	  <selector xmlns:android="http://schemas.android.com/apk/res/android">
	      <item android:state_selected="true" android:color="#4dd0c8" />
	      <item android:state_pressed="true" android:color="#4dd0c8" />
	      <item android:color="#cccccc" />
	  </selector>
	  ```
- res/drawable/main_tab_icon_home.xml
	- ```java
	  <?xml version="1.0" encoding="utf-8"?>
	  <selector xmlns:tools="http://schemas.android.com/tools" xmlns:android="http://schemas.android.com/apk/res/android">
	      <item android:state_selected="true" android:drawable="@mipmap/ic_launcher" />
	      <item android:state_pressed="true" android:drawable="@mipmap/ic_launcher" />
	      <item android:drawable="@mipmap/ic_launcher" tools:ignore="StateListReachable" />
	  </selector>
	  ```