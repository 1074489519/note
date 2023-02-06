- `decodeURI()`函数用于解码js中的URL。它将编码的url字符串作为参数并返回已解码的字符串，用法：
- ```js
  var uri = "my profile.php?name=sammer&occupation=pāntiNG";
  var encoded_uri = encodeURI(uri);
  decodeURI(encoded_uri); // 'my%20profile.php?name=sammer&occupation=p%C4%81ntiNG'
  ```
-