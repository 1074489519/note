- ## 概述
	- 当服务器收到HTTP请求时，服务器可以在响应头里面添加一个`Set-Cookie`选项。浏览器收到响应后通常会保存下`Cookie`，之后对该服务器每一次请求中都通过`Cookie`请求头部将`Cookie`信息发送给服务器。另外，`Cookie`的过期时间、域、路径、有效期、使用站点都可以根据需要来制定。
- ## `Set-Cookie` 相应头部 和 `Cookie` 请求头部
	- 服务器使用 `Set-Cookie` 响应头部想用户代理（一般浏览器）发送 `Cookie` 信息。
	- ```
	  Set-Cookie: <cookie 名>=<cookie 值>
	  ```
	- 服务器通过该头部告知客户端保存Cookie信息
	- ```
	  HTTP/1.0 200 OK
	  Content-Type: text/html
	  Set-Cookie: yummy_cookie=choco
	  Set-Cookie: tasty_cookie=strawberry
	  
	  [页面内容]
	  ```
	- 现在，对该服务器发送的每一项新请求，浏览器都会将之前保存的Cookie信息通过`Cookie`请求头部再发送给服务器。
	- ```
	  GET /smaple_page.html HTTP/1.1
	  Host: www.example.org
	  Cookie: yummy_cookie=choco; testy_cookie=strawberry
	  ```
- ## 定义`Cookie`的生命周期
	- Cookie的声明周期可以通过两种方式定义：
		- 会话期Cookie是最简单的Cookie：浏览器关闭之后它会被自动删除，也就是说它仅在会话期内有效。会话期Cookie不需要制定过期时间（`Expires`）或者有效期（`Max-Age`）。需要注意的是，有些浏览器提供了会话恢复功能，这种情况下即使关闭了浏览器，会话期Cookie也会被保留下来，就好像浏览器从来没有关闭一样，这会导致Cookie的生命周期无限期延长。
		- 持久性Cookie的生命周期取决于过期时间（`Expires`）或者有效期（`Max-Age`）指定一段时间。
	- 例如：
		- ```
		  Set-Cookie: id=asda; Expires=Wed, 21 Oct 2015 07:28:00 GMT;
		  ```
	- 如果您的站点对用户进行身份验证，则每当用户进行身份验证时，它都应重新生成并重新发送会话 Cookie，甚至是已经存在的会话 Cookie。此技术有助于防止[会话固定攻击（session fixation attacks） (en-US)](https://developer.mozilla.org/en-US/docs/Web/Security/Types_of_attacks)，在该攻击中第三方可以重用用户的会话。
- ## 限制访问 `Cookie`
	- 有两种方法可以确保`Cookie`被安全发送，并且不会被以外的参与者或脚本访问：`Secure`属性和`HttpOnly`属性。
	- 标记为`Secure`的Cookie只应通过被HTTPS协议加密过的请求发送给服务器，因此可以预防[man-in-the-middle](https://developer.mozilla.org/zh-CN/docs/Glossary/MitM)攻击者的攻击。但即使设置了`Secure`标记，敏感信息也不应该通过Cookie传输，因为Cookie有其固有的不安全性，`Secure`标记也无法提供确实的安全保障，例如，可以访问客户端硬盘的人可以读取它。
	- [[JavaScript]]的`Document.cookie` API无法访问带有`HttpOnly`属性的cookie；此类Cookie仅作用于服务器。例如，持久化服务器会话的Cookie不需要对JavaScript可用，而应具有`HttpOnly`属性。此预防措施有助于缓解[跨站点脚本（XSS） (en-US)](https://developer.mozilla.org/en-US/docs/Web/Security/Types_of_attacks)攻击。
	- 例如：
		- ```
		  Set-Cookie: id=asd; Expires=Wed, 21 Oct 2015 07:28:00 GMT; Secure; HttpOnly
		  ```
- ## `Cookie`的作用域
	- `Domain` 和 `Path` 标记定义了Cookie的作用域：即允许Cookie应该发送给哪些URL。
	- ### Domain属性
		- `Domain`指定了哪些主机可以接受Cookie。如果不指定，默认为 `origin`，不包含子域名。如果制定了`Domain`，则一般包含子域名。因此，制定`Domain`比省略它的限制要少。但是，当子域需要共享有关用户的信息时，这可能会有所帮助。
		- 例如，如果设置`Domain=mozilla.org`，则Cookie也包含在子域名中（如：`developer.mozilla.org`）。
	- ### Path属性
		- `Path`标识制定了主机下的哪些路径可以接受Cookie（该URL路径必须存在于请求URL中）。以字符`%x2F`("/")作为路径分隔符，子路径也会被匹配。
		- 例如：设置`Path=/docs`，则以下地址都会匹配：
			- `/docs`
			- `/docs/Web/`
			- `/docs/Web/HTTP`
	- ### SameSite attribute
		- `SameSite` Cookie 允许服务器要求某个 cookie 在跨站请求时不会被发送，（其中[Site(en-US)](https://developer.mozilla.org/en-US/docs/Glossary/Site)由可注册域定义），从而可以阻止跨站请求伪造攻击（[CSRF](https://developer.mozilla.org/zh-CN/docs/Glossary/CSRF)）。
		- SameSite cookies 是相对较新的一个字段，[所有主流浏览器都已经得到支持](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Set-Cookie#browser_compatibility)。
		- 例子：
		- ```
		  Set-Cookie: key=value; SomeSite=Strict
		  ```
		- `SomeSite`可以有下面三种值：
			- `None`：浏览器会在同站请求、跨站请求下继续发送cookies，不区分大小写
			- `Strict`：浏览器将只在访问相同站点时发送cookie。（在原有Cookies的限制条件上的加强，如上文"Cookie的作用域”所述
			- `Lax`：与`Strict`类似，但用户从外部站点导航至URL时（例如通过链接）除外。在新版本浏览器中，为默认选项，`Some-site cookies`将会为一些夸站子请求保留，如图片加载或者`frames`的调用，但只有当用户从外部站点导航到URL时才会发送。如link连接
		- > **备注：**以前，如果 SameSite 属性没有设置，或者没有得到运行浏览器的支持，那么它的行为等同于 None，Cookies 会被包含在任何请求中——包括跨站请求。大多数主流浏览器正在将[SameSite 的默认值迁移至 Lax](https://www.chromestatus.com/feature/5088147346030592)。如果想要指定 Cookies 在同站、跨站请求都被发送，现在需要明确指定 SameSite 为 None。
	- ### Cookie prefixes
		- cookie的机制使得服务器无法确认cookie是在安全来源上设置的，甚至无法确定cookie最初是在哪里设置的。
		- 子域上的易受攻击的应用程序可以使用`Domain`属性设置cookie，从而可以访问所有其他子域上的该cookie。会话固定估计中可能会滥用此机制。有关主要缓解方法，请参阅[会话劫持（ session fixation） (en-US)](https://developer.mozilla.org/en-US/docs/Web/Security/Types_of_attacks)。
		- 但是，作为[深度防御措施](https://en.wikipedia.org/wiki/Defense_in_depth_(computing))，可以使用 cookie 前缀来断言有关 cookie 的特定事实。有两个前缀可用：
			- `__Host-`
				- 如果cookie名称具有次前缀，则仅当它们也用`Secure`属性标记，是从安全来源发送的，不包含`Domain`属性，并将`Path`属性设置为`/`时，他才在`Set-Cookie`标头中接受。这样，这些cookie可以被视为"domain-locked"。
			- `__Secure-`
				- 如果cookie名称具有此前缀，则仅当它也用`Secure`属性标记，是从安全来源发送的，它才在 `Set-Cookie`标头中接受。该前缀限制要弱于`__Host-`前缀。
		- 带有这些前缀点Cookie，如果不符合其限制的会被浏览器拒绝。请注意，者确保了如果子域要创建带有前缀的cookie，那么它将药么局限于该子域，要么被完全忽略。由于应用服务器仅在确定用户是否已通过身份验证或 [[CSRF]]令牌正确时才检查特定的cookie名称，因此，这有效地充当了针对会话劫持的防御措施。
		- > **备注：**在应用程序服务器上，Web 应用程序**必须**检查完整的 cookie 名称，包括前缀 —— 用户代理程序在从请求的[ `Cookie` ](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Cookie)标头中发送前缀之前，**不会**从 cookie 中剥离前缀。
	- ### JavaScript通过Document.cookie访问Cookie
		- 通过`Document.cookie`属性可创建新的Cookie，也可通过该属性访问非`HttpOnly`标记的Cookie。
		- ```js
		  document.cookie = "yummy_cookie=choco";
		  document.cookie = "tasty_cookie=strawberry";
		  console.log(document.cookie);
		  // logs "yummy_cookie=choco; tasty_cookie=strawberry"
		  ```
		- 通过JavaScript创建的Cookie不能包含HttpOnly标志。