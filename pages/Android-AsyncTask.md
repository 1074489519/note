- 简单使用
	- ```java
	  public class MainActivity extends AppCompatActivity {
	      private ViewPager viewPager;
	      private static final String TAG = "MainActivity";
	  
	      @Override
	      protected void onCreate(Bundle savedInstanceState) {
	          super.onCreate(savedInstanceState);
	          setContentView(R.layout.activity_main);
	  
	          new DownloadAsyncTask().execute("jack");
	      }
	  	// <入参，进度，结果>
	      public class DownloadAsyncTask extends AsyncTask<String, Integer, Boolean> {
	          /**
	           * 在异步任务之前，在主线程中
	           * */
	          @Override
	          protected void onPreExecute() {
	              super.onPreExecute();
	              // 可操作UI
	          }
	  
	          /**
	           * 在另外一个线程中处理事件
	           * @param params 入参
	           * @retur 结果
	           */
	          @Override
	          protected Boolean doInBackground(String... params) {
	              for (int i = 0; i < 10000; i++) {
	                  publishProgress(i);// 调整进度，onProgressUpdate会执行
	                  Log.i(TAG, "doInBackground:" + params[0]);
	              }
	              return true;
	          }
	          /**
	           * 在主线程中，执行结果 处理
	           * */
	          @Override
	          protected void onPostExecute(Boolean aBoolean) {
	              super.onPostExecute(aBoolean);
	          }
	  
	          /**
	           * 在主线程，进度变化时执行，第二个参数
	           * */
	          @Override
	          protected void onProgressUpdate(Integer... values) {
	              super.onProgressUpdate(values);
	              // 收到进度，然后处理
	          }
	      }
	  }
	  ```
-