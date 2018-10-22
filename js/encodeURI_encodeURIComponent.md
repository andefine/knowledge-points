# `encodeURI`([mdn](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/encodeURI))和`encodeURIComponent`([mdn](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/encodeURIComponent))

- 不被转义的字符：
  + `encodeURI`不被转义的字符
  ```javascript
  A-Z a-z 0-9 ; , / ? : @ & = + $ - _ . ! ~ * ' ( ) #
  ```
  + `encodeURIComponent`不被转义的字符
  ```javascript
  A-Z a-z 0-9 - _ . ! ~ * ' ( )
  ```

- 文档指出
  + `encodeURI`
  > 请注意，encodeURI 自身无法产生能适用于HTTP GET 或 POST 请求的URI，例如对于 XMLHTTPRequests, 因为 "&", "+", 和 "=" 不会被编码，然而在 GET 和 POST 请求中它们是特殊字符
  + `encodeURIComponent`
  > 为了避免服务器收到不可预知的请求，对任何用户输入的作为URI部分的内容你都需要用encodeURIComponent进行转义。比如，一个用户可能会输入"Thyme &time=again"作为comment变量的一部分。如果不使用encodeURIComponent对此内容进行转义，服务器得到的将是comment=Thyme%20&time=again。请注意，"&"符号和"="符号产生了一个新的键值对，所以服务器得到两个键值对（一个键值对是comment=Thyme，另一个则是time=again），而不是一个键值对

- 实例对比
  + `encodeURI`
  ```javascript
  let url = 'http://wip38f.natappfree.cc?id=1344587&code=HRIU48932UGWE8'
  console.log(encodeURI(url)) // "http://wip38f.natappfree.cc?id=1344587&code=HRIU48932UGWE8"
  ```
  + `encodeURIComponent`
  ```javascript
  let url = 'http://wip38f.natappfree.cc?id=1344587&code=HRIU48932UGWE8'
  console.log(encodeURIComponent(url)) // "http%3A%2F%2Fwip38f.natappfree.cc%3Fid%3D1344587%26code%3DHRIU48932UGWE8"
  ```

- 至于`escape`
  官方表示在严格意义上，没有废弃，但不推荐使用。(万一以后浏览器都不支持了呢，想哭都来不及T_T\")