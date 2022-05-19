[怎样解决跨域问题](https://zhuanlan.zhihu.com/p/122206470)

## 使用nginx解决跨域问题

通过服务器的反向代理，将前端访问域名 跟后端服务器域名映射到同源的地址下，从而实现前端服务和后端服务的同源，那自然就不存在跨域问题

例子：

- 前端服务：http://localhost:3000
- 前端页面路由：http://localhost:3000/index.html
- 后端服务：http://localhost:3001
- 后端接口路由：http://localhost:3001/api/test.do

可以看出，两个服务处于跨域的状态

通过nginx的配置进行反向代理，即可实现前后端服务器同源，如下：

```nginx
server
{
    listen:80;
    server_name:localhost;
    
    location = / {
        proxy_pass http://localhost:3000;
    }
    
    location /api {
        proxy_pass http://localhost:3001;
        
        #允许指定跨域的方法，*代表所有
        add_header Access-Control-Allow-Methods *;
        #预检命令的缓存，如果不缓存每次会发送两次请求
        add_header Access-Control-Max-Age 3600;
        #带cookie的请求需要加上这个字段，并设置为true
        add_header Access-Control-Allow-Credentials true;
        #表示允许这个跨域调用（客户端发送请求的域名和端口）
        #$http_origin动态获取请求客户端请求的域  不用*的原因是因为cookie的请求不支持*号
        add_header Access-Control-Allow-Origin $http_origin;
        
        #表示请求头的字段  动态获取
        add_header Access-Control-Allow-Headers
            $http_access_control_request_headers;
        
        #options 预检命令，预检命令通过时才发送请求
        #检查请求的类型是不是预检命令
        if($request_method = OPTIONS){
            return 200;
        }
    }
}
```

