- 引入
	- ```
	   <dependency>
	              <groupId>ch.qos.logback</groupId>
	              <artifactId>logback-classic</artifactId>
	              <version>1.2.11</version>
	          </dependency>
	  ```
	- resources/logback.xml
	- ```xml
	  <?xml version="1.0" encoding="UTF-8" ?>
	  <configuration>
	    <appender name="console" class="ch.qos.logback.core.ConsoleAppender">
	      <encoder>
	        <pattern>
	          %d{HH:mm:ss.SSS} [%thread] %-5level %logger{36} - %msg%n
	        </pattern>
	      </encoder>
	    </appender>
	    <!--
	        日志输出级别（优先级高到低）：
	        error: 错误 - 系统的故障日志
	        warn: 警告 - 存在风险或使用不当的日志
	        info: 一般性消息
	        debug: 程序内部用于调试信息
	        trace: 程序运行的跟踪信息
	        -->
	    <root level="debug">
	      <appender-ref ref="console"/>
	    </root>
	  </configuration>
	  ```