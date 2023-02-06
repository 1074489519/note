- 与JUnit4整合
	- Maven工程依赖`string-test`
	- 利用`@RunWith`与`@ContextConfiguration`描述测试用例类
	- 测试用例类从容器获取对象完成测试用例的执行
- 示例
	- test/java/SpringTest
	- ```java
	  // 将Junit4的执行权交由Spring Test，在测试用例执行前自动初始化IoC容器
	  @RunWith(SpringJUnit4ClassRunner.class)
	  @ContextConfiguration(locations = {"classpath:applicationContext.xml"})
	  public class SpringTest {
	    @Resource
	    private UserService userService;
	  
	    // 运行方法
	    @Test
	    public void testUserService() {
	      userService.createUser();
	    }
	  }
	  ```