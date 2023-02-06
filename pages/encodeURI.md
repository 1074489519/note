- `encodeURI()`函数用于在JS中对URL进行编码。它将url字符串作为参数并返回编码的字符串
- **注意**： encodeURI()不会编码类似这样字符： `/ ? : @ & = + $ #`，如果需要编码这些字符，请使用[[encodeURIComponent]]。 用法：
- ```js
  var uri = "my profile.php?name=sammer&occupation=pāntiNG";
  var encoded_uri = encodeURI(uri); // 'my%20profile.php?name=sammer&occupation=p%C4%81ntiNG'
  ```