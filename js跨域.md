# 跨域

## JSONP

在js中，我们直接用XMLHttpRequest请求不同域上的数据时，浏览器是不允许的。但是，在页面上引入不同域上的js脚本文件却是可以的，JSONP正是利用这个特性来实现的。

优点：兼容性更好。
缺点：它只支持GET请求而不支持POST等其它类型的HTTP请求。

## CORS

服务器设置header `CORS: Access-Control-Allow-Origin：*`

## window.location.hash+iframe

原理同jsonp，iframe没有同源限制，利用form表单提交，method为POST，target为iframe name属性值，监听iframe onload事件。

```html
  <form action="/upload" name="form" enctype="multipart/form-data" method="post" target="hidden-iframe">
    <iframe name="hidden-iframe" id="hidden-iframe"></iframe>
  </form>
  <script>
    document.querySelector('#hidden-iframe').onload = function(e) {
      const search = e.target.contentWindow.location.search;
    }
  </script>
```

缺点：需要接口特殊处理返回数据

## postMessage+ifrme

随着HTML5的发展，html5工作组提供了两个重要的接口：`postMessage(send)` 和 `onmessage`

## nginx反向代理
nginx.conf
```bash
server {
  listen       9000;  #配置第一台服务器
  server_name  localhost;
  location /api/ {
    rewrite ^/api/(.*)$ /$1 break;   #所有对后端的请求加一个api前缀方便区分，真正访问的时候移除这个前缀
    # API Server
    proxy_pass http://www.serverA.com;  #将真正的请求代理到serverA,即真实的服务器地址，ajax的url为/api/user/1的请求将会访问http://www.serverA.com/user/1
  }
}
```