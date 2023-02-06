- xml声明
	- xml声明说明xml文档的基本信息，包括版本号与字符集，写在xml第一行
	- ```xml
	  <?xml version="1.0" encoding="UTF-8"?>
	  
	  ```
- 语意约束
	- DTD
		- ```dtd
		  dtd文件	
		  <?xml version="1.0" encoding="UTF-8"?>
		  <!ELEMENT hr (employee)> // hr下只能有一个employee节点
		  ```
		- ```xml
		  xml中引入
		  书写格式
		  <!DOCTYPE 根节点 SYSTEM "dtd文件路径">
		  实例
		  <!DOCTYPE hr SYSTEM "hr.dtd">
		  ```
	- Schema
- DOM模型和Dom4j
	- DOM4j
		- 易用、开源的库，用于解析XML。他应用于java平台，具有性能优异、功能强大和极其易使用的特点
		- Dom4j讲XML视为Document对象
		- XML标签被Dom4j定义为Element对象
- XPath路径表达式
	- 概述
		- 是XML文档中查找数据的语言
		- 可极大的提高在提取数据时的开发效率
		- 本质是掌握各种形式表达式的使用技巧
	- 常用的基本表达式
		- |表达式|描述|
		  |--|--|
		  |nodename|选取此节点的所有子节点|
		  |/|从根节点选取|
		  |// |从匹配选择的当前节点选择文档中的节点，而不考虑他们的位置|
		  |.|选择当前节点|
		  |..|选取当前节点的父节点|
		  |@|选取属性|