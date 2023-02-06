- 概述
	- Spring框架用于处理关系性数据库的模块
	- 对JDBC API进行封装，极大简化开发工作量
	- JdbcTemplate是Spring JDBC核心类，提供数据CRUD方法
- CRUD
	- 查询
		- 查询单条数据
			- ```java
			   public User findById(Integer eno) {
			          String sql = "select * from users where eno = ?";
			          User user = jdbcTemplate.queryForObject(sql, new Object[]{eno}, new BeanPropertyRowMapper<User>(User.class));
			          return user;
			      }
			  ```
		- 查询符合数据
			- ```java
			  public List<User> findByDname(String dname) {
			    String sql = "select * from users where dname = ?";
			    List<User> list = jdbcTemplate.query(sql, new Object[]{"研发部"}, new BeanPropertyRowMapper<User>(User.class)));
			    return list;
			  }
			  ```
		- 查询并返回Map
			- ```java
			  public List<Map<String, Object>> findMapByDname(String dname) {
			    String sql = "select * from users where dname = ?";
			    List<Map<String, Object>> maps = jdbcTemplate.queryForList(sql, new Object[]{dname}));
			    return maps;
			  }
			  ```
	- 插入
	  collapsed:: true
		- ```java
		  public void insert(User user) {
		    String sql = "insert into users(eno, ename, salary,dname) values(?,?,?,?,?)";
		    jdbcTemplate.update(sql, new Object[]{
		      user.getEno(), user.getEname(), user.getSalary(), user.getDname()
		    });
		  }
		  ```
	- 更新
	  collapsed:: true
		- ```java
		  public int update(User user) {
		    String sql = "update users SET ename = ?, salary = ?, dname = ?) where eno = ?;
		    int count = jdbcTemplate.update(sql, new Object[]{
		       user.getEname(), user.getSalary(), user.getDname(), user.getEno()
		    });
		    return count;
		  }
		  ```
	- 删除
	  collapsed:: true
		- ```java
		  public int delete(String eno) {
		    String sql = "delete from users where eno = ?;
		    int count = jdbcTemplate.update(sql, new Object[]{
		       eno
		    });
		    return count;
		  }
		  ```
- 事务
	- 编程式事务
		- 概述
		  collapsed:: true
			- 编程式事务是指通过代码手动提交回滚事务的事务控制方法
			- SpringJDBC通过TransactionManager事务管理器实现事务控制
			- 事务管理器提供commit/rollback方法进行事务提交与回滚
		- 实例
		  collapsed:: true
			- ```java
			  private DataSourceTransactionManager dtm;
			  
			  public void batchImport() {
			    // 定义了事务默认的标准配置
			    TransactionDefinition definition = new DefaultTransactionDefinition();
			    // 开始一个事务,返回事务状态，事务状态说明当前事务的执行阶段
			    TransactionStatus status = dtm.getTransaction(definition);
			    try{
			      for(int i=0;i<10;i++) {
			        User user = new User();
			        user.setEname("员工" + i);
			        user.setSalary(8000);
			        user.setDname("市场部");
			        userDao.insert(user);
			      }
			      // 提交事务
			      dtm.commit(status);
			    }catch(RuntimeException e) {
			      // 回滚事务
			      dtm.rollback(status);
			      e.printStackTrace();
			    }
			  }
			  ```
	- 声明式事务
	  collapsed:: true
		- 概述
			- 指在不修改源码情况下通过配置形式自动实现事务控制，声明式事务本质就是AOP环绕通知
			- 当目标方法执行成功时自动提交事务
			- 当目标方法抛出运行时异常时，自动事务回滚
		- 配置过程
			- 配置TransactionManager事务管理器
			- 配置事务通知与事务属性
			- 为事务通知绑定PointCut切点
		- 实例
			- pom.xml
			- ```xml
			  <dependency>
			    <groupId>org.aspectj</groupId>
			    <artifactId>aspectjweaver</artifactId>
			    <version>1.9.5</version>
			  </dependency>
			  ```
			- applicationContext.xml
			- ```xml
			  
			  <!-- 1 事务管理器，用于创建事务/提交/回滚-->
			  <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
			    <property name="dataSource" ref="dataSource"/>
			  </bean>
			  <!-- 2 事务通知配置，决定哪些方法使用事务，哪些方法不使用事务 -->
			  <tx:advice id="txAdvice" transaction-manager="transactionManager">
			    <tx:attributes>
			      <!--     目标方法名为batchImport时，启用声明式事务，成功提交，运行时异常回滚       -->
			      <tx:method name="batchImport" propagation="REQUIRED"/>
			    </tx:attributes>
			  </tx:advice>
			  <!-- 定义声明式事务的作用范围 -->
			  <aop:config>
			    <aop:pointcut id="pointcut" expression="execution(* com.example..*Service.*(..))"/>
			    <aop:advisor advice-ref="txAdvice" pointcut-ref="pointcut" />
			  </aop:config>
			  ```
	- 注解形式声明式事务
		- 实例
			- applicationContext.xml
			- ```xml
			  <!-- 数据源 -->
			  <bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource">
			    <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
			    <property name="url" value="jdbc:mysql://localhost:3306/users"/>
			    <property name="username" value="root"/>
			    <property name="password" value="root"/>
			  </bean>
			  <!-- JdbcTemplate提供数据CRUD的API-->
			  <bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
			    <property name="dataSource" ref="dataSource"/>
			  </bean>
			  <!-- 事务管理器，用于创建事务/提交/回滚-->
			  <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
			    <property name="dataSource" ref="dataSource"/>
			  </bean>
			  <!--  启用注解式声明式事务  -->
			  <tx:annotation-driven transaction-manager="transactionManager"/>
			  ```
			- UserService.java
			- ```java
			  /*
			   * 声明式事务核心注解
			   * 放在类上，将声明式事务配置应用于当前类所有方法，默认事务传播为 REQUIRED
			   * */
			  @Transactional(propagation = Propagation.REQUIRED)
			  public class UserService {
			      @Resource
			      private UserDao userDao;
			  
			      // 不启用事务，并且该方法只读的
			      @Transactional(propagation = Propagation.NOT_SUPPORTED, readOnly = true)
			      public User findById(Integer eno) {
			          return userDao.findById(eno);
			      }
			  
			      public void batchImport() {
			          for (int i = 0; i < 10; i++) {
			              if(i == 3) {
			                  throw new RuntimeException("意料之外的错误");
			              }
			              User u = new User();
			              u.setEname("zhangsan");
			              u.setDname("san");
			              u.setDname("部门");
			              u.setSalary("30");
			              userDao.insert(u);
			          }
			      }
			  }
			  ```