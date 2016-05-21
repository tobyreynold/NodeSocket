# NodeSocket
This is a simple chat room.

## nodejs原生http模块的困惑

socket.io里面教程里面有一段这样的代码
``` bash
var http = require('http').Server(app);
http.listen(3000, function(){
  console.log('listening on *:3000');
});
```

不难理解就是3000端口启动一个http服务，但是通过express cli 生成的./bin/www里面的代码也存放这启动http服务的代码，代码是这样的：

``` bash
var http = require('http');
var port = normalizePort(process.env.PORT || '3000');
app.set('port', port);
var server = http.createServer(app);
server.listen(port);
```

这时候俺就震惊了，同是Require的http，为毛启动一个服务会有两种createServer()和Server()两种？你去查nodejs[官方文档](https://nodejs.org/api/http.html)，发现也只有第一种，那么第二种是什么鬼？

stackoverflow上给出了[答案](http://stackoverflow.com/questions/26921117/http-createserverapp-v-http-serverapp)，其实这时候就得翻nodejs的源码了，源码在这里https://github.com/nodejs/node-v0.x-archive/blob/523929c9272a53c9429616564a45f2af59670e47/lib/http.js#L1903-L1905，第1903行到1905行，从上面的url上可以看到一个小技巧，就是代码后面hash值接L50-L60的意思就是第50行到60行。
