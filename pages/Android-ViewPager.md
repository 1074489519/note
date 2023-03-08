- 案例
	- layout
		- ```java
		  public class MainActivity extends AppCompatActivity {
		      private ViewPager viewPager;
		      private int[] mLayoutIds = {
		              R.layout.view_first,
		              R.layout.view_second
		      };
		      private List<View> views;
		  
		      @Override
		      protected void onCreate(Bundle savedInstanceState) {
		          super.onCreate(savedInstanceState);
		          setContentView(R.layout.activity_main);
		  
		          viewPager = findViewById(R.id.view_page);
		          // 初始化数据
		          views = new ArrayList<>();
		          for (int i = 0; i < mLayoutIds.length; i++) {
		              View view = getLayoutInflater().inflate(mLayoutIds[i], null);
		              views.add(view);
		          }
		          // 设置Adapter
		          viewPager.setAdapter(mPagerAdapter);
		      }
		  
		      PagerAdapter mPagerAdapter = new PagerAdapter() {
		          @Override
		          public int getCount() {
		              return mLayoutIds.length;
		          }
		          @Override
		          public boolean isViewFromObject(@NonNull View view, @NonNull Object object) {
		              return view == object;
		          }
		          @NonNull
		          @Override
		          public Object instantiateItem(@NonNull ViewGroup container, int position) {
		              // 添加每一项的视图
		              View child = views.get(position);
		              container.addView(child);
		              return child;
		          }
		          @Override
		          public void destroyItem(@NonNull ViewGroup container, int position, @NonNull Object object) {
		              container.removeView(views.get(position));
		          }
		      };
		  
		  }
		  ```
	- fragment(弃用)
		- ```java
		  public class MainActivity extends AppCompatActivity {
		      private ViewPager viewPager;
		      @Override
		      protected void onCreate(Bundle savedInstanceState) {
		          super.onCreate(savedInstanceState);
		          setContentView(R.layout.activity_main);
		          viewPager = findViewById(R.id.view_page);
		          viewPager.setAdapter(new FragmentPagerAdapter(getSupportFragmentManager()) {
		              @NonNull
		              @Override
		              public Fragment getItem(int position) {
		                  return TestFragment.newInstance(position);
		              }
		              @Override
		              public int getCount() {
		                  return 4;
		              }
		          });
		      }
		  }
		  ```
	-